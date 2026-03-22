# Gotchas & Lessons Learned

Critical issues discovered from real-world HTML-to-Elementor conversions. Read this before generating JSON.

---

## 0. Native Elementor widgets first, `html` widget last

**This is the most important principle.** Always use native Elementor widgets when possible. They are editable in the visual editor, respond to global styles, support responsive controls, and the client can modify them without touching code.

The `html` widget renders raw HTML which means: no visual editing, no responsive controls, no global style inheritance, and the client needs to edit code to change anything.

**Use `html` widget ONLY for:**
- Headings that need FA icons or colored `<span>` tags (heading widget sanitizes these away)
- Loading external CSS/fonts via `<link>` tags
- Injecting `<style>` blocks (smooth scroll, etc.)
- SVG/embed content with no native equivalent

**Use native widgets for everything else:**
- TOC / link lists -> `icon-list` or `text-editor` with `<ol>`
- Paragraphs, data blocks, styled text -> `text-editor` (supports inline styles in `editor` field)
- Buttons -> `button` widget
- Images -> `image` widget
- Dividers -> `divider` widget
- Spacing -> `spacer` widget or container padding/margin

---

## 1. Heading widget sanitizes HTML

**Problem:** The `heading` widget's `title` field strips most HTML tags. If you put `<i class="fa-solid fa-shield">` or `<span style="color:red;">` in the title, Elementor removes them on save.

**What gets stripped:** `<i>`, `<span style="...">`, `<div>`, `<img>`, most inline HTML.

**What survives:** `<br>`, `<span>` without style attribute (sometimes), basic `<strong>`/`<em>`.

**Solution:** When a heading needs icons, colored text spans, or any HTML markup, use the `html` widget instead:

```json
{
  "widgetType": "html",
  "settings": {
    "html": "<h2 style=\"font-family:Oswald,sans-serif;font-size:1.15rem;font-weight:600;color:#222;\"><i class=\"fa-solid fa-lock\" style=\"color:#f7941d;margin-right:8px;\"></i> Section Title</h2>"
  }
}
```

This renders exactly as written. The tradeoff is you lose Elementor's visual typography controls, but for icon headings it's the only reliable approach.

**When to use `heading` widget:** Plain text headings with no HTML, icons, or colored spans. The heading widget is fine for `"title": "About Us"` but not for `"title": "About <span style='color:red'>Us</span>"`.

---

## 2. Text-editor widget partially sanitizes HTML

**Problem:** The `text-editor` widget's `editor` field is more permissive than `heading`, but still sanitizes some HTML on save in the visual editor.

**What works reliably:** `<p>`, `<strong>`, `<em>`, `<a>`, `<br>`, `<ul>`, `<ol>`, `<li>`, `<h3>`, `<h4>`, basic inline styles on these tags.

**What can break:** Complex nested `<div>` structures, `<style>` blocks, `<script>` tags, deeply nested styling. Elementor may rewrite or strip these when the page is opened in the editor.

**Solution:** For complex formatted blocks (data cards, highlight boxes, styled lists with custom bullets), use the `html` widget. It passes content through without sanitization.

**Practical rule:** If your text-editor content has more than 2 levels of HTML nesting or uses `<div>` blocks with inline styles, switch to `html` widget.

---

## 3. Font Awesome icons don't render by default

**Problem:** Elementor loads its own icon library (eicons) but does NOT automatically load Font Awesome from CDN. If the source HTML uses FA classes like `fa-solid fa-check`, they won't render unless FA is already loaded by the theme or a plugin.

**Solution:** Add an `html` widget at the top of the page that loads FA:

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
```

Put this in a minimal container at the very start of the `content` array. The container should have zero padding/margin so it's invisible.

**Note:** Elementor Pro does include FA as an option in Settings > Advanced, but it's not always enabled. The CDN link is a safe fallback.

---

## 4. Google Fonts may not be available

**Problem:** If the source HTML uses fonts like Oswald, Playfair Display, etc. via Google Fonts, they won't render in Elementor unless: (a) the theme loads them, (b) they're set in Elementor's Global Fonts, or (c) they're loaded manually.

**Solution:** Add Google Fonts link in the same loader `html` widget:

```html
<link href="https://fonts.googleapis.com/css2?family=Oswald:wght@400;600;700&family=Raleway:wght@300;400;500;600&display=swap" rel="stylesheet">
```

**Better long-term approach:** Tell the user to add these fonts to Elementor Site Settings > Global Fonts so they're available site-wide. The CDN link is a quick fix for imported templates.

---

## 5. Anchor scrolling requires container wrapper

**Problem:** Setting `_css_id` on a widget adds `id="..."` to the widget's wrapper div, but anchor scrolling (`#section-name`) is unreliable when targeting widget wrappers because Elementor's CSS can interfere with scroll positioning.

**Solution:** Wrap anchor targets in a dedicated container with `_css_id`:

```json
{
  "elType": "container",
  "isInner": true,
  "settings": {
    "content_width": "full",
    "_css_id": "section-name",
    "padding": {"unit": "px", "top": "0", "right": "0", "bottom": "0", "left": "0", "isLinked": true}
  },
  "elements": [ ...section content... ]
}
```

**Also required:** Inject `scroll-behavior: smooth` via the loader html widget:

```html
<style>html{scroll-behavior:smooth;}</style>
```

Without this, clicking TOC links causes an instant jump instead of smooth scroll.

---

## 6. Full-width layout depends on theme and page settings

**Problem:** Many themes (Phlox, Astra, GeneratePress, Hello) wrap Elementor content in their own boxed container. Setting `content_width: "full"` in the JSON only stretches to the theme's container width, not the viewport.

**The JSON template alone cannot fix this.**

**Solution:** Always remind the user to:
1. Set the page's **Page Layout** to "Elementor Full Width" or "Elementor Canvas" (in WordPress page editor sidebar, or Elementor Page Settings)
2. Or set the default in Elementor > Settings > General > Default Page Layout

**Elementor Canvas** removes the theme header/footer entirely. **Elementor Full Width** keeps theme header/footer but gives full-width content area.

---

## 7. isLinked values are inconsistent

**Problem:** In real Elementor exports, the `isLinked` field in spacing/sizing objects can be `true`, `false`, `""` (empty string), or `"1"` (string). This inconsistency comes from Elementor's internal handling.

**Practical rule:**
- Use `true` or `"1"` for linked (all sides equal)
- Use `false` or `""` for unlinked (different values per side)
- Both formats work on import, but be consistent within your template

---

## 8. Widget margin vs container padding

**Problem:** Spacing between widgets can be controlled two ways: widget `_margin` or container `flex_gap`. Using both creates double spacing.

**Best practice:**
- Use `flex_gap` on containers for uniform spacing between children
- Use `_margin` on individual widgets only for exceptions (e.g., extra space before a heading)
- Set `flex_gap` to `"0"` when you want full manual control via widget margins

---

## 9. Long documents: structure for editability

**Problem:** Legal pages, policies, and long-form documents have many sections. The temptation is to dump everything into `html` widgets for speed, but this makes the page impossible to edit visually.

**Best approach:**
- Use `text-editor` widgets for all body content (paragraphs, lists, data blocks with inline styles)
- Use `heading` widgets for plain text headings; `html` widget only for headings that need FA icons
- Wrap each section in a container with `_css_id` for anchor scrolling
- Use `icon-list` or `text-editor` with `<ol>` for the table of contents
- Apply typography via Elementor settings (`typography_typography: "custom"` etc.) rather than inline styles where possible

**Result:** The client can click on any section in Elementor's visual editor and change text, colors, and spacing without touching code.

---

## 10. Programmatic generation for large pages

For pages with many repeated patterns (9 sections of a privacy policy, 20 FAQ items, etc.), write a Python script to generate the JSON rather than hand-crafting it. This:
- Eliminates copy-paste errors
- Ensures consistent IDs (use `random.choice('0123456789abcdef')` for 8-char IDs)
- Makes it easy to iterate on the template
- Allows structural validation before output

Pattern:
```python
import json, random

def gen_id():
    return ''.join(random.choice('0123456789abcdef') for _ in range(8))

def make_section(anchor, title, content_html):
    return {
        "id": gen_id(),
        "elType": "container",
        "isInner": True,
        "settings": {"content_width": "full", "_css_id": anchor},
        "elements": [
            {"id": gen_id(), "elType": "widget", "widgetType": "html",
             "isInner": False, "settings": {"html": f"<h2>{title}</h2>"}, "elements": []},
            {"id": gen_id(), "elType": "widget", "widgetType": "html",
             "isInner": False, "settings": {"html": content_html}, "elements": []}
        ]
    }
```
