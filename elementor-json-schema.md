# Elementor JSON Schema Reference

Source: https://developers.elementor.com/docs/data-structure/ + real Elementor Pro exports

## Template structure (top-level)

```json
{
  "title": "Template Title",
  "type": "page",
  "version": "0.4",
  "page_settings": [],
  "content": []
}
```

| Key | Type | Description |
|-----|------|-------------|
| `title` | string | Display name in WP dashboard and Elementor editor |
| `type` | string | Document type: `page`, `header`, `footer`, `error-404`, `popup`, `section` |
| `version` | string | Always `"0.4"` (current Elementor data structure version) |
| `page_settings` | array or object | Empty `[]` if no settings, `{}` if page has custom settings |
| `content` | array | Array of top-level element objects |

---

## Two layout systems

Elementor has TWO layout systems. **Containers (Flexbox)** are the current default. **Sections/Columns** are legacy but still found on older sites. Generate Container-based JSON for new templates.

---

## Container element (Flexbox - current)

```json
{
  "id": "a1b2c3d4",
  "elType": "container",
  "isInner": false,
  "settings": {},
  "elements": []
}
```

### Container layout settings

**CRITICAL: Container flex settings use the `flex_` prefix.**

```json
"settings": {
  "content_width": "full",
  "flex_direction": "row",
  "flex_direction_tablet": "column",
  "flex_justify_content": "space-between",
  "flex_align_items": "center",
  "flex_gap": {
    "unit": "px",
    "size": "30",
    "sizes": [],
    "column": "30",
    "row": "30",
    "isLinked": "1"
  },
  "flex_wrap": "wrap"
}
```

#### content_width
- `"full"` -- stretches to full parent/viewport width
- `"boxed"` -- constrains to site content width (default ~1140px)

Custom boxed width:
```json
"content_width": "boxed",
"boxed_width": {"unit": "px", "size": "1600", "sizes": []}
```

#### flex_direction
`"row"`, `"column"`, `"row-reverse"`, `"column-reverse"`
Responsive: `flex_direction_tablet`, `flex_direction_mobile`

#### flex_justify_content
`"flex-start"`, `"center"`, `"flex-end"`, `"space-between"`, `"space-around"`, `"space-evenly"`
Responsive: `flex_justify_content_tablet`, `flex_justify_content_mobile`

#### flex_align_items
`"flex-start"`, `"center"`, `"flex-end"`, `"stretch"`
Responsive: `flex_align_items_tablet`, `flex_align_items_mobile`

#### flex_gap format
```json
"flex_gap": {
  "unit": "px",
  "size": "20",
  "sizes": [],
  "column": "20",
  "row": "20",
  "isLinked": "1"
}
```
`isLinked`: `"1"` = row and column gaps same. `""` = independent.

#### Container child sizing
```json
"settings": {
  "content_width": "full",
  "width": {"unit": "%", "size": 50},
  "_flex_size": "none",
  "_element_width": "initial"
}
```

### Container min-height

```json
"min_height": {"unit": "px", "size": "600", "sizes": []}
```
**NOTE: It's `min_height`, NOT `custom_height`.**

### Container backgrounds

Classic:
```json
"background_background": "classic",
"background_color": "#FFFFFF",
"background_image": {"url": "https://example.com/bg.jpg", "id": "", "size": ""},
"background_position": "center center",
"background_size": "cover"
```

Gradient:
```json
"background_background": "gradient",
"background_color": "#2B6CB0",
"background_color_b": "#2C5282",
"background_gradient_angle": {"unit": "deg", "size": 135, "sizes": []}
```

Overlay:
```json
"background_overlay_background": "classic",
"background_overlay_color": "#00000080",
"background_overlay_opacity": {"unit": "px", "size": 0.5, "sizes": []}
```

### Container spacing

```json
"padding": {"unit": "px", "top": "60", "right": "35", "bottom": "60", "left": "35", "isLinked": ""},
"padding_tablet": {"unit": "px", "top": "40", "right": "20", "bottom": "40", "left": "20", "isLinked": ""},
"padding_mobile": {"unit": "px", "top": "20", "right": "15", "bottom": "20", "left": "15", "isLinked": ""},
"margin": {"unit": "px", "top": "0", "right": "0", "bottom": "0", "left": "0", "isLinked": true}
```

**NOTE:** `isLinked` can be `true`, `false`, `""`, or `"1"` in real exports.

### Borders

```json
"border_border": "solid",
"border_width": {"unit": "px", "top": "1", "right": "0", "bottom": "0", "left": "0", "isLinked": false},
"border_color": "#EAEAEA",
"border_radius": {"unit": "px", "top": "8", "right": "8", "bottom": "8", "left": "8", "isLinked": true}
```

### Visibility controls

```json
"hide_desktop": "",
"hide_tablet": "",
"hide_mobile": "hidden-phone"
```

### Global color/font references

```json
"__globals__": {
  "background_color": "globals/colors?id=accent",
  "text_color": "globals/colors?id=primary"
}
```
Avoid in portable templates -- use explicit hex values instead.

---

## Section/Column elements (Legacy)

### Section
```json
{
  "id": "45157afd",
  "elType": "section",
  "isInner": false,
  "settings": {
    "structure": "20",
    "gap": "no",
    "content_position": "middle",
    "background_background": "classic",
    "background_color": "#F7FAFC",
    "padding": {"unit": "px", "top": "60", "right": "35", "bottom": "60", "left": "35", "isLinked": false}
  },
  "elements": [/* columns */]
}
```

`structure` values: `"10"` = 1 col, `"20"` = 2 equal, `"22"` = 66/33, `"30"` = 3 equal, `"40"` = 4 equal

### Column
```json
{
  "id": "3a744bf5",
  "elType": "column",
  "isInner": false,
  "settings": {
    "_column_size": 50,
    "_inline_size": null,
    "content_position_mobile": "center",
    "align": "space-between"
  },
  "elements": [/* widgets */]
}
```

---

## Widget element

```json
{
  "id": "e5f6g7h8",
  "elType": "widget",
  "widgetType": "heading",
  "isInner": false,
  "settings": {},
  "elements": []
}
```

### Common widget settings (Advanced tab - all widgets)

```json
{
  "_padding": {"unit": "px", "top": "20", "right": "0", "bottom": "20", "left": "0", "isLinked": false},
  "_margin": {"unit": "px", "top": "0", "right": "0", "bottom": "16", "left": "0", "isLinked": false},
  "_margin_mobile": {"unit": "px", "top": "0", "right": "0", "bottom": "10", "left": "0", "isLinked": false},
  "_element_width": "auto",
  "_element_width_tablet": "inherit",
  "_element_width_mobile": "auto",
  "_flex_size": "none",
  "hide_mobile": "hidden-phone"
}
```

---

## Typography settings

```json
{
  "typography_typography": "custom",
  "typography_font_family": "Poppins",
  "typography_font_size": {"unit": "px", "size": 18, "sizes": []},
  "typography_font_size_mobile": {"unit": "px", "size": 14, "sizes": []},
  "typography_font_weight": "600",
  "typography_text_transform": "uppercase",
  "typography_line_height": {"unit": "px", "size": 26, "sizes": []},
  "typography_letter_spacing": {"unit": "px", "size": 6.3, "sizes": []}
}
```

**MUST set `"typography_typography": "custom"` for overrides to work.**

---

## Third-party widgets

Real sites often use addon widgets. Common ones:

**Elementor Pro:** `flip-box`, `call-to-action`, `testimonial-carousel`, `form`, `social-icons`, `price-table`, `rating`, `testimonial`

**HFE (Header Footer Elementor):** `hfe-infocard`, `hfe-nav-menu`

**Auxin/Phlox:** `aux_logo`, `aux_menu_box`, `aux_modern_button`, `aux_text`, `aux_modern_heading`, `aux_icon_list`

When converting HTML, always use native Elementor widgets unless the user specifies addon availability.
