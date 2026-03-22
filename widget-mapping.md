# Widget Mapping Reference

How to convert HTML elements to Elementor widget JSON.

**IMPORTANT: Read `references/gotchas.md` first.** The `heading` widget sanitizes HTML (strips icons, styled spans). Use the `html` widget instead when content includes `<i>`, `<span style="...">`, or any inline HTML.

---

## Heading widget

**HTML:** `<h1>`, `<h2>`, `<h3>`, `<h4>`, `<h5>`, `<h6>` -- **plain text only**
**widgetType:** `"heading"`

**WARNING:** This widget strips `<i>`, `<span style>`, and most inline HTML from the `title` field. If your heading needs icons or colored text, use the `html` widget with a `<h2>` tag inside instead.

```json
{
  "id": "aa11bb22",
  "elType": "widget",
  "widgetType": "heading",
  "isInner": false,
  "settings": {
    "title": "Welcome to Our Practice",
    "header_size": "h1",
    "align": "center",
    "title_color": "#2D3748",
    "typography_typography": "custom",
    "typography_font_family": "Playfair Display",
    "typography_font_size": {"unit": "px", "size": 48, "sizes": []},
    "typography_font_size_tablet": {"unit": "px", "size": 36, "sizes": []},
    "typography_font_size_mobile": {"unit": "px", "size": 28, "sizes": []},
    "typography_font_weight": "700",
    "typography_line_height": {"unit": "em", "size": 1.2, "sizes": []}
  },
  "elements": []
}
```

| Setting | Values | Notes |
|---------|--------|-------|
| `title` | string | The heading text (plain text, no HTML) |
| `header_size` | `"h1"` through `"h6"` | Semantic HTML tag |
| `align` | `"left"`, `"center"`, `"right"`, `"justified"` | Text alignment |
| `title_color` | hex color | Heading color |
| `link.url` | URL string | Makes heading a link |

---

## Text Editor widget

**HTML:** `<p>`, `<div>` with rich text, `<span>`, mixed inline content
**widgetType:** `"text-editor"`

```json
{
  "id": "cc33dd44",
  "elType": "widget",
  "widgetType": "text-editor",
  "isInner": false,
  "settings": {
    "editor": "<p>We provide compassionate, evidence-based therapy in a safe and supportive environment. Our therapists specialise in <strong>anxiety</strong>, <strong>depression</strong>, and <strong>relationship issues</strong>.</p>",
    "align": "center",
    "text_color": "#4A5568",
    "typography_typography": "custom",
    "typography_font_size": {"unit": "px", "size": 18, "sizes": []},
    "typography_font_size_mobile": {"unit": "px", "size": 16, "sizes": []},
    "typography_line_height": {"unit": "em", "size": 1.7, "sizes": []}
  },
  "elements": []
}
```

| Setting | Values | Notes |
|---------|--------|-------|
| `editor` | HTML string | Can contain `<p>`, `<strong>`, `<em>`, `<a>`, `<ul>`, `<ol>` |
| `align` | `"left"`, `"center"`, `"right"` | Text alignment |
| `text_color` | hex color | Default text color |

**Important:** The `editor` field accepts HTML. Escape any double quotes inside the HTML with `\"`.

---

## Button widget

**HTML:** `<a>` styled as button, `<button>`
**widgetType:** `"button"`

```json
{
  "id": "ee55ff66",
  "elType": "widget",
  "widgetType": "button",
  "isInner": false,
  "settings": {
    "text": "Book a Free Consultation",
    "link": {
      "url": "https://example.com/contact",
      "is_external": "",
      "nofollow": "",
      "custom_attributes": ""
    },
    "align": "center",
    "size": "md",
    "button_type": "default",
    "background_color": "#2B6CB0",
    "button_text_color": "#FFFFFF",
    "border_radius": {"unit": "px", "top": "6", "right": "6", "bottom": "6", "left": "6", "isLinked": true},
    "typography_typography": "custom",
    "typography_font_size": {"unit": "px", "size": 16, "sizes": []},
    "typography_font_weight": "600",
    "button_padding": {"unit": "px", "top": "15", "right": "30", "bottom": "15", "left": "30", "isLinked": false}
  },
  "elements": []
}
```

| Setting | Values | Notes |
|---------|--------|-------|
| `text` | string | Button label |
| `link.url` | URL string | Destination |
| `link.is_external` | boolean | Open in new tab |
| `size` | `"xs"`, `"sm"`, `"md"`, `"lg"`, `"xl"` | Preset sizes |
| `button_type` | `"default"`, `"info"`, `"success"`, `"warning"`, `"danger"` | Style preset |
| `background_color` | hex color | Button background |
| `button_text_color` | hex color | Button text color |
| `hover_color` | hex color | Text color on hover |
| `button_background_hover_color` | hex color | Background on hover |

---

## Image widget

**HTML:** `<img>`
**widgetType:** `"image"`

```json
{
  "id": "11223344",
  "elType": "widget",
  "widgetType": "image",
  "isInner": false,
  "settings": {
    "image": {
      "url": "https://example.com/therapist-office.jpg",
      "id": "",
      "size": "",
      "alt": "Therapy office",
      "source": "library"
    },
    "image_size": "full",
    "align": "center",
    "width": {"unit": "%", "size": 100, "sizes": []},
    "caption_source": "none",
    "link_to": "none",
    "border_radius": {"unit": "px", "top": "12", "right": "12", "bottom": "12", "left": "12", "isLinked": true}
  },
  "elements": []
}
```

| Setting | Values | Notes |
|---------|--------|-------|
| `image.url` | URL string | Image source |
| `image.id` | string | WP attachment ID (empty if external) |
| `image_size` | `"full"`, `"large"`, `"medium"`, `"thumbnail"` | WP image size |
| `width` | size object | Image display width |
| `align` | `"left"`, `"center"`, `"right"` | Image alignment |
| `link_to` | `"none"`, `"custom"`, `"file"` | Link behavior |

---

## Icon List widget

**HTML:** `<ul>` or `<ol>` (especially with icons)
**widgetType:** `"icon-list"`

```json
{
  "id": "55667788",
  "elType": "widget",
  "widgetType": "icon-list",
  "isInner": false,
  "settings": {
    "icon_list": [
      {
        "text": "Individual Therapy",
        "selected_icon": {"value": "fas fa-check-circle", "library": "fa-solid"},
        "_id": "6cde1af"
      },
      {
        "text": "Couples Counselling",
        "selected_icon": {"value": "fas fa-check-circle", "library": "fa-solid"},
        "_id": "496f7b1"
      },
      {
        "text": "Online Sessions Available",
        "selected_icon": {"value": "fas fa-check-circle", "library": "fa-solid"},
        "_id": "a3b8c2d"
      }
    ],
    "space_between": {"unit": "px", "size": 12, "sizes": []},
    "icon_color": "#2B6CB0",
    "icon_size": {"unit": "px", "size": 14, "sizes": []},
    "text_color": "#4A5568",
    "text_indent": {"unit": "px", "size": 8, "sizes": []}
  },
  "elements": []
}
```

Each item in the `icon_list` repeater needs a unique `_id`.

---

## HTML widget (for raw HTML / SVG)

**HTML:** Complex SVGs, custom HTML, embeds
**widgetType:** `"html"`

```json
{
  "id": "99aabb00",
  "elType": "widget",
  "widgetType": "html",
  "isInner": false,
  "settings": {
    "html": "<svg xmlns=\"http://www.w3.org/2000/svg\" viewBox=\"0 0 24 24\" fill=\"none\" stroke=\"currentColor\" stroke-width=\"2\"><path d=\"M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z\"/></svg>"
  },
  "elements": []
}
```

**Critical:** Escape ALL double quotes inside the HTML/SVG string with `\"`.

---

## Spacer widget

**HTML:** Empty `<div>` used for spacing, `<br>` elements
**widgetType:** `"spacer"`

```json
{
  "id": "ccddee01",
  "elType": "widget",
  "widgetType": "spacer",
  "isInner": false,
  "settings": {
    "space": {"unit": "px", "size": 40, "sizes": []},
    "space_tablet": {"unit": "px", "size": 30, "sizes": []},
    "space_mobile": {"unit": "px", "size": 20, "sizes": []}
  },
  "elements": []
}
```

---

## Divider widget

**HTML:** `<hr>`, decorative lines
**widgetType:** `"divider"`

```json
{
  "id": "ffeedd02",
  "elType": "widget",
  "widgetType": "divider",
  "isInner": false,
  "settings": {
    "style": "solid",
    "weight": {"unit": "px", "size": 2, "sizes": []},
    "color": "#E2E8F0",
    "width": {"unit": "%", "size": 60, "sizes": []},
    "align": "center",
    "gap": {"unit": "px", "size": 20, "sizes": []}
  },
  "elements": []
}
```

---

## Google Maps widget

**HTML:** Embedded map iframes
**widgetType:** `"google_maps"`

```json
{
  "id": "aabb0011",
  "elType": "widget",
  "widgetType": "google_maps",
  "isInner": false,
  "settings": {
    "address": "10 Harley Street, London W1G 9PF",
    "zoom": {"unit": "px", "size": 15},
    "height": {"unit": "px", "size": 300}
  },
  "elements": []
}
```

---

## Form widget (Pro only)

**HTML:** `<form>` elements
**widgetType:** `"form"`

This is an Elementor Pro widget. Use the HTML widget as a fallback if the target site might not have Pro.

---

## Social Icons widget

**HTML:** Social media link lists
**widgetType:** `"social-icons"`

```json
{
  "id": "cc001122",
  "elType": "widget",
  "widgetType": "social-icons",
  "isInner": false,
  "settings": {
    "social_icon_list": [
      {
        "social_icon": {"value": "fab fa-facebook-f", "library": "fa-brands"},
        "link": {"url": "https://facebook.com/example", "is_external": true},
        "_id": "soc001"
      },
      {
        "social_icon": {"value": "fab fa-instagram", "library": "fa-brands"},
        "link": {"url": "https://instagram.com/example", "is_external": true},
        "_id": "soc002"
      }
    ],
    "shape": "circle",
    "align": "center"
  },
  "elements": []
}
```

---

## Common patterns (native-first)

### Table of Contents (TOC) -- using text-editor

Instead of an `html` widget, use `text-editor` with an ordered list. The `editor` field supports `<ol>`, `<a>`, and inline styles:

```json
{
  "id": "dd001122",
  "elType": "widget",
  "widgetType": "text-editor",
  "isInner": false,
  "settings": {
    "editor": "<ol>\n<li><a href=\"#s1\">Administrator danych osobowych</a></li>\n<li><a href=\"#s2\">Jakie dane zbieramy i w jakim celu?</a></li>\n<li><a href=\"#s3\">Pliki cookies</a></li>\n</ol>",
    "text_color": "#555555",
    "typography_typography": "custom",
    "typography_font_size": {"unit": "px", "size": 15, "sizes": []}
  },
  "elements": []
}
```

The user can edit individual list items visually in Elementor's text editor.

### Data block / info card -- using text-editor

Styled info boxes (like GDPR data processing details) work well as text-editor widgets with inline styles in the `editor` field:

```json
{
  "id": "ee112233",
  "elType": "widget",
  "widgetType": "text-editor",
  "isInner": false,
  "settings": {
    "editor": "<div style=\"background:#f7f7f7;border-radius:12px;padding:20px 24px;border:1px solid #e8e8e8;\"><p><strong>Zakres danych:</strong> imię i nazwisko, adres e-mail.</p><p><strong>Cel przetwarzania:</strong> odpowiedź na zapytanie.</p></div>"
  },
  "elements": []
}
```

The user can click into the text-editor and modify the text content visually, even though the outer div has inline styles.

### Highlight / callout box -- using text-editor

```json
{
  "id": "ff223344",
  "elType": "widget",
  "widgetType": "text-editor",
  "isInner": false,
  "settings": {
    "editor": "<div style=\"background:rgba(247,148,29,0.08);border-left:3px solid #f7941d;border-radius:0 12px 12px 0;padding:16px 20px;\"><p>Skontaktuj się z nami: <strong><a href=\"mailto:kontakt@example.pl\" style=\"color:#f7941d;\">kontakt@example.pl</a></strong></p></div>"
  },
  "elements": []
}
```
