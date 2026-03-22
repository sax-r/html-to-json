---
name: html-to-elementor
description: Convert HTML (or React/Tailwind markup) into valid, importable Elementor JSON templates. Use this skill whenever the user wants to turn HTML code into an Elementor template, generate Elementor JSON from a design, convert a landing page or section from HTML to Elementor format, create Elementor-compatible JSON for pasting or importing. Also trigger when the user mentions "Elementor JSON", "Elementor template from HTML", "convert to Elementor", or wants to build Elementor sections programmatically. Use even for partial conversions like single sections, headers, footers, or hero blocks.
---

# HTML to Elementor JSON Converter

Convert HTML/CSS markup into valid Elementor JSON templates that can be **imported via the Elementor template library** or **pasted directly into `_elementor_data` postmeta** via WP-CLI.

## Before you start

Read these reference files based on what you need:

- `references/elementor-json-schema.md` — **Always read this first.** The official JSON structure, element types, and settings keys from Elementor developer docs + real export analysis.
- `references/widget-mapping.md` — How to map HTML elements to Elementor widgets with real examples.
- `references/examples.md` — Complete input/output examples (hero section, features grid, CTA).
- `references/gotchas.md` — **Read this before generating.** Critical lessons from real-world conversions: widget sanitization, icon loading, anchor scrolling, theme conflicts.

## Two output formats

Ask the user which format they need:

### 1. Import Template (`.json` file)
Full template file you can import in Elementor > Templates > Import:
```json
{
  "title": "My Section",
  "type": "page",
  "version": "0.4",
  "page_settings": [],
  "content": [ ...elements... ]
}
```

### 2. Content-only (for `_elementor_data`)
Just the `content` array, for pasting into postmeta via WP-CLI:
```json
[ ...elements... ]
```

If the user doesn't specify, default to **Import Template** format since it's more portable.

## Core conversion rules

### 1. Container hierarchy (Flexbox-first)
Elementor uses containers (not the old sections/columns). Every layout wrapper becomes a container element:

```
Outer container: content_width = "full" (stretches to viewport)
  └─ Inner container: content_width = "boxed" (constrains to ~1140px)
      └─ Widgets and nested containers
```

- Use `flex_direction: "row"` for horizontal layouts, `"column"` for vertical stacking
- Map CSS `justify-content` to Elementor's `flex_justify_content` setting
- Map CSS `align-items` to Elementor's `flex_align_items` setting
- Map CSS `gap` to Elementor's `flex_gap` setting (format: `{"unit": "px", "size": "20", "sizes": [], "column": "20", "row": "20", "isLinked": "1"}`)

### 2. Widget selection strategy

**PRINCIPLE: Always prefer native Elementor widgets over the `html` widget.** Native widgets are editable in the visual editor, respond to global styles, and work with Elementor's responsive controls. The `html` widget should be a last resort.

| HTML element | Elementor widget | Notes |
|-------------|-----------------|-------|
| `<h1>`-`<h6>` plain text | `heading` | Works great for plain text headings |
| `<h1>`-`<h6>` with icons/HTML | `html` | Only because `heading` sanitizes `<i>`, `<span style>` |
| `<p>`, rich text | `text-editor` | Handles `<strong>`, `<em>`, `<a>`, `<ul>`, `<ol>`, inline styles |
| `<img>` | `image` | Always use native -- gives responsive controls |
| `<a>` as button | `button` | Always use native -- gives hover states, sizing |
| `<ul>`/`<ol>` | `text-editor` | Put the list HTML in the `editor` field |
| `<ul>` with FA icons | `icon-list` | Native widget with FA icon support per item |
| Navigation-style links | `icon-list` | Numbered/bulleted link lists (like TOC) |
| `<hr>` | `divider` | Native widget with color, width, gap controls |
| Empty spacing | `spacer` | Native widget with responsive height |
| SVG / embed / `<script>` | `html` | No native alternative exists |
| CSS/font loader | `html` | For injecting `<link>` and `<style>` tags |

**When to use `html` widget (last resort only):**
- Headings with FA icons or colored `<span>` (heading widget sanitizes these)
- Loading external CSS/fonts (`<link>` tags)
- Injecting `<style>` blocks (like `scroll-behavior: smooth`)
- Complex SVG or embed code
- Markup that has no native Elementor equivalent

**When NOT to use `html` widget:**
- Table of contents -- use `text-editor` with `<ol>` list, or `icon-list` widget
- Paragraphs with bold/links -- use `text-editor`
- Bullet lists -- use `text-editor` with `<ul>` or `icon-list` widget
- Buttons -- use `button` widget
- Images -- use `image` widget
- Data blocks / styled boxes -- use `text-editor` with inline styles in `editor` field
- Anything the user might want to edit visually later

See `references/widget-mapping.md` for full details and `references/gotchas.md` for sanitization specifics.

### 3. Responsive settings
Elementor uses suffixed keys for breakpoints. Desktop is the base (no suffix):

```json
{
  "padding": {"unit": "px", "top": "60", "right": "40", "bottom": "60", "left": "40", "isLinked": ""},
  "padding_tablet": {"unit": "px", "top": "40", "right": "20", "bottom": "40", "left": "20", "isLinked": ""},
  "padding_mobile": {"unit": "px", "top": "20", "right": "15", "bottom": "20", "left": "15", "isLinked": ""}
}
```

Same suffix pattern applies to: `flex_direction_tablet`, `flex_direction_mobile`, `flex_gap_tablet`, `flex_gap_mobile`, `flex_align_items_tablet`, etc.

**Always set mobile `flex_direction` to `"column"` for row layouts** unless the user explicitly needs horizontal mobile layout.

### 4. Element IDs
Every element needs a unique `id` -- an 8-character lowercase hex string. Generate random ones:
```
"id": "a1b2c3d4"
```

### 5. Styling translation
- CSS colors -> Elementor color settings (e.g., `background_color`, `title_color`)
- CSS `background-image` -> `background_background: "classic"` + `background_image.url`
- CSS `font-size` -> `typography_typography: "custom"` + `typography_font_size: {"unit": "px", "size": 32}`
- CSS `font-weight` -> `typography_font_weight: "600"`
- CSS `padding`/`margin` -> `padding`/`_margin` objects with `unit`, `top`, `right`, `bottom`, `left`, `isLinked`
- CSS `border-radius` -> `border_radius` with same structure

### 6. External dependencies (icons, fonts, smooth scroll)

If the source HTML uses Font Awesome, Google Fonts, or `scroll-behavior: smooth`, you MUST inject a loader. Add an invisible container at the very top of the `content` array with an `html` widget that loads the required resources:

```json
{
  "id": "...",
  "elType": "container",
  "isInner": false,
  "settings": {
    "content_width": "full",
    "padding": {"unit": "px", "top": "0", "right": "0", "bottom": "0", "left": "0", "isLinked": true},
    "margin": {"unit": "px", "top": "0", "right": "0", "bottom": "0", "left": "0", "isLinked": true}
  },
  "elements": [{
    "id": "...",
    "elType": "widget",
    "widgetType": "html",
    "isInner": false,
    "settings": {
      "html": "<link rel=\"stylesheet\" href=\"https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css\"><link href=\"https://fonts.googleapis.com/css2?family=Oswald:wght@400;600;700&display=swap\" rel=\"stylesheet\"><style>html{scroll-behavior:smooth;}</style>"
    },
    "elements": []
  }]
}
```

This ensures icons and fonts render even if the theme doesn't load them. The `scroll-behavior: smooth` enables anchor link scrolling from TOCs.

### 7. Anchor scrolling (TOC links, jump links)

Elementor's `_css_id` setting adds an `id` attribute to an element's wrapper div. To make anchor links like `#section-name` work:

**Wrap each anchor target in a container** with `_css_id` set to the anchor name:

```json
{
  "id": "...",
  "elType": "container",
  "isInner": true,
  "settings": {
    "content_width": "full",
    "_css_id": "section-name",
    "padding": {"unit": "px", "top": "0", "right": "0", "bottom": "0", "left": "0", "isLinked": true}
  },
  "elements": [ ...section content widgets... ]
}
```

Do NOT put `_css_id` directly on a widget -- it works on the widget wrapper but scroll targeting is unreliable. A dedicated container is more predictable.

### 8. Performance rules
- Avoid nesting containers more than 3 levels deep
- Use native Elementor widget settings for styling when possible
- Use `custom_css` only for pseudo-elements (::before, ::after) or complex hover transitions
- Minimize containers -- if a section only has stacked widgets, one container with `flex_direction: "column"` is enough
- For long documents (legal pages, policies), wrap each section in a container for anchor scrolling, and use native widgets inside for editability

### 9. Theme compatibility

Some themes (Phlox, Astra, etc.) wrap Elementor content in their own boxed container. The JSON template alone cannot force full-width layout. **Tell the user** to:
1. Set Page Layout to "Elementor Full Width" or "Elementor Canvas" in page settings
2. Or go to Elementor > Settings > General and set Default Page Layout

### 10. JSON validation checklist
Before outputting, verify:
- [ ] Every element has a unique 8-char hex `id`
- [ ] Every element has `elType` ("container" or "widget")
- [ ] Widgets have `widgetType` set
- [ ] All elements have `isInner` (boolean) and `elements` (array)
- [ ] `settings` is `[]` (empty array) when no settings, or `{}` (object) when it has settings
- [ ] No trailing commas in JSON
- [ ] All strings properly escaped (especially in `html` widget content)
- [ ] Responsive suffixes used correctly (`_tablet`, `_mobile`)
- [ ] The JSON is valid and parseable
- [ ] Font/icon dependencies are loaded via html widget if needed
- [ ] Anchor IDs are on containers, not widgets

## Step-by-step conversion process

1. **Analyze the HTML** -- identify layout structure, content, dependencies (fonts, icons), and any anchor links
2. **Read references** -- load `references/elementor-json-schema.md`, `references/widget-mapping.md`, and `references/gotchas.md`
3. **Plan widget strategy** -- use native Elementor widgets for everything possible; only use `html` widget for icon-headings and CSS/font loaders (see gotchas)
4. **Add dependency loader** -- if FA icons, Google Fonts, or smooth scroll needed, add loader container first
5. **Map structure** -- convert HTML wrappers to containers, content to widgets
6. **Apply styles** -- translate CSS to Elementor settings; use inline styles in `html` widgets for complex formatting
7. **Add responsive** -- set tablet and mobile overrides
8. **Set up anchors** -- wrap anchor targets in containers with `_css_id`
9. **Validate** -- run through the checklist above
10. **Output** -- provide JSON and remind user to set Page Layout to "Elementor Full Width"
