# Conversion Examples

Complete HTML-to-Elementor-JSON examples. Use these as patterns.

---

## Example 1: Therapist Hero Section

### Input HTML

```html
<section style="background-color: #F7FAFC; padding: 80px 0;">
  <div style="max-width: 1140px; margin: 0 auto; text-align: center; padding: 0 20px;">
    <h1 style="font-size: 48px; font-weight: 700; color: #1A202C; margin-bottom: 16px;">
      Find Peace of Mind
    </h1>
    <p style="font-size: 20px; color: #4A5568; max-width: 640px; margin: 0 auto 32px; line-height: 1.7;">
      Compassionate, evidence-based therapy to help you navigate
      life's challenges. Online and in-person sessions available.
    </p>
    <a href="/contact" style="display: inline-block; background: #2B6CB0; color: white; padding: 16px 32px; border-radius: 8px; font-size: 18px; font-weight: 600; text-decoration: none;">
      Book a Free Consultation
    </a>
  </div>
</section>
```

### Output JSON (Import Template format)

```json
{
  "title": "Therapist Hero Section",
  "type": "page",
  "version": "0.4",
  "page_settings": [],
  "content": [
    {
      "id": "a1f03b7c",
      "elType": "container",
      "isInner": false,
      "settings": {
        "content_width": "full",
        "flex_direction": "column",
        "flex_align_items": "center",
        "background_background": "classic",
        "background_color": "#F7FAFC",
        "padding": {"unit": "px", "top": "80", "right": "0", "bottom": "80", "left": "0", "isLinked": false},
        "padding_tablet": {"unit": "px", "top": "60", "right": "0", "bottom": "60", "left": "0", "isLinked": false},
        "padding_mobile": {"unit": "px", "top": "40", "right": "0", "bottom": "40", "left": "0", "isLinked": false},
        "html_tag": "section"
      },
      "elements": [
        {
          "id": "b2e14c8d",
          "elType": "container",
          "isInner": true,
          "settings": {
            "content_width": "boxed",
            "flex_direction": "column",
            "flex_align_items": "center",
            "padding": {"unit": "px", "top": "0", "right": "20", "bottom": "0", "left": "20", "isLinked": false}
          },
          "elements": [
            {
              "id": "c3d25e9f",
              "elType": "widget",
              "widgetType": "heading",
              "isInner": false,
              "settings": {
                "title": "Find Peace of Mind",
                "header_size": "h1",
                "align": "center",
                "title_color": "#1A202C",
                "typography_typography": "custom",
                "typography_font_size": {"unit": "px", "size": 48, "sizes": []},
                "typography_font_size_tablet": {"unit": "px", "size": 36, "sizes": []},
                "typography_font_size_mobile": {"unit": "px", "size": 28, "sizes": []},
                "typography_font_weight": "700",
                "_margin": {"unit": "px", "top": "0", "right": "0", "bottom": "16", "left": "0", "isLinked": false}
              },
              "elements": []
            },
            {
              "id": "d4e36fa0",
              "elType": "widget",
              "widgetType": "text-editor",
              "isInner": false,
              "settings": {
                "editor": "<p>Compassionate, evidence-based therapy to help you navigate life's challenges. Online and in-person sessions available.</p>",
                "align": "center",
                "text_color": "#4A5568",
                "typography_typography": "custom",
                "typography_font_size": {"unit": "px", "size": 20, "sizes": []},
                "typography_font_size_mobile": {"unit": "px", "size": 16, "sizes": []},
                "typography_line_height": {"unit": "em", "size": 1.7, "sizes": []},
                "_element_width": "initial",
                "_flex_size": "none",
                "_margin": {"unit": "px", "top": "0", "right": "0", "bottom": "32", "left": "0", "isLinked": false}
              },
              "elements": []
            },
            {
              "id": "e5f470b1",
              "elType": "widget",
              "widgetType": "button",
              "isInner": false,
              "settings": {
                "text": "Book a Free Consultation",
                "link": {
                  "url": "/contact",
                  "is_external": "",
                  "nofollow": ""
                },
                "align": "center",
                "background_color": "#2B6CB0",
                "button_text_color": "#FFFFFF",
                "border_radius": {"unit": "px", "top": "8", "right": "8", "bottom": "8", "left": "8", "isLinked": true},
                "typography_typography": "custom",
                "typography_font_size": {"unit": "px", "size": 18, "sizes": []},
                "typography_font_weight": "600",
                "button_padding": {"unit": "px", "top": "16", "right": "32", "bottom": "16", "left": "32", "isLinked": false}
              },
              "elements": []
            }
          ]
        }
      ]
    }
  ]
}
```

---

## Example 2: Three-Column Features Grid

### Input HTML

```html
<section style="padding: 60px 0; background: white;">
  <div style="max-width: 1140px; margin: 0 auto; display: flex; gap: 30px; padding: 0 20px;">
    <div style="flex: 1; text-align: center; padding: 30px;">
      <h3 style="font-size: 22px; font-weight: 600; color: #1A202C; margin-bottom: 12px;">Individual Therapy</h3>
      <p style="font-size: 16px; color: #718096; line-height: 1.6;">One-to-one sessions tailored to your unique needs and goals.</p>
    </div>
    <div style="flex: 1; text-align: center; padding: 30px;">
      <h3 style="font-size: 22px; font-weight: 600; color: #1A202C; margin-bottom: 12px;">Couples Counselling</h3>
      <p style="font-size: 16px; color: #718096; line-height: 1.6;">Strengthen your relationship with guided, supportive sessions.</p>
    </div>
    <div style="flex: 1; text-align: center; padding: 30px;">
      <h3 style="font-size: 22px; font-weight: 600; color: #1A202C; margin-bottom: 12px;">Online Sessions</h3>
      <p style="font-size: 16px; color: #718096; line-height: 1.6;">Convenient therapy from the comfort of your own home.</p>
    </div>
  </div>
</section>
```

### Output JSON (Import Template format)

```json
{
  "title": "Three Services Features",
  "type": "page",
  "version": "0.4",
  "page_settings": [],
  "content": [
    {
      "id": "f6a581c2",
      "elType": "container",
      "isInner": false,
      "settings": {
        "content_width": "full",
        "flex_direction": "column",
        "background_background": "classic",
        "background_color": "#FFFFFF",
        "padding": {"unit": "px", "top": "60", "right": "0", "bottom": "60", "left": "0", "isLinked": false},
        "padding_mobile": {"unit": "px", "top": "40", "right": "0", "bottom": "40", "left": "0", "isLinked": false},
        "html_tag": "section"
      },
      "elements": [
        {
          "id": "a7b692d3",
          "elType": "container",
          "isInner": true,
          "settings": {
            "content_width": "boxed",
            "flex_direction": "row",
            "flex_direction_mobile": "column",
            "flex_gap": {"unit": "px", "size": "30", "sizes": [], "column": "30", "row": "30", "isLinked": "1"},
            "flex_gap_mobile": {"unit": "px", "size": "20", "sizes": [], "column": "20", "row": "20", "isLinked": "1"},
            "padding": {"unit": "px", "top": "0", "right": "20", "bottom": "0", "left": "20", "isLinked": false}
          },
          "elements": [
            {
              "id": "b8c7a3e4",
              "elType": "container",
              "isInner": true,
              "settings": {
                "content_width": "full",
                "flex_direction": "column",
                "flex_align_items": "center",
                "_flex_size": "none",
                "_flex_grow": 1,
                "padding": {"unit": "px", "top": "30", "right": "30", "bottom": "30", "left": "30", "isLinked": true}
              },
              "elements": [
                {
                  "id": "c9d8b4f5",
                  "elType": "widget",
                  "widgetType": "heading",
                  "isInner": false,
                  "settings": {
                    "title": "Individual Therapy",
                    "header_size": "h3",
                    "align": "center",
                    "title_color": "#1A202C",
                    "typography_typography": "custom",
                    "typography_font_size": {"unit": "px", "size": 22, "sizes": []},
                    "typography_font_weight": "600",
                    "_margin": {"unit": "px", "top": "0", "right": "0", "bottom": "12", "left": "0", "isLinked": false}
                  },
                  "elements": []
                },
                {
                  "id": "dae9c506",
                  "elType": "widget",
                  "widgetType": "text-editor",
                  "isInner": false,
                  "settings": {
                    "editor": "<p>One-to-one sessions tailored to your unique needs and goals.</p>",
                    "align": "center",
                    "text_color": "#718096",
                    "typography_typography": "custom",
                    "typography_font_size": {"unit": "px", "size": 16, "sizes": []},
                    "typography_line_height": {"unit": "em", "size": 1.6, "sizes": []}
                  },
                  "elements": []
                }
              ]
            },
            {
              "id": "ebfad617",
              "elType": "container",
              "isInner": true,
              "settings": {
                "content_width": "full",
                "flex_direction": "column",
                "flex_align_items": "center",
                "_flex_size": "none",
                "_flex_grow": 1,
                "padding": {"unit": "px", "top": "30", "right": "30", "bottom": "30", "left": "30", "isLinked": true}
              },
              "elements": [
                {
                  "id": "fc0be728",
                  "elType": "widget",
                  "widgetType": "heading",
                  "isInner": false,
                  "settings": {
                    "title": "Couples Counselling",
                    "header_size": "h3",
                    "align": "center",
                    "title_color": "#1A202C",
                    "typography_typography": "custom",
                    "typography_font_size": {"unit": "px", "size": 22, "sizes": []},
                    "typography_font_weight": "600",
                    "_margin": {"unit": "px", "top": "0", "right": "0", "bottom": "12", "left": "0", "isLinked": false}
                  },
                  "elements": []
                },
                {
                  "id": "0d1cf839",
                  "elType": "widget",
                  "widgetType": "text-editor",
                  "isInner": false,
                  "settings": {
                    "editor": "<p>Strengthen your relationship with guided, supportive sessions.</p>",
                    "align": "center",
                    "text_color": "#718096",
                    "typography_typography": "custom",
                    "typography_font_size": {"unit": "px", "size": 16, "sizes": []},
                    "typography_line_height": {"unit": "em", "size": 1.6, "sizes": []}
                  },
                  "elements": []
                }
              ]
            },
            {
              "id": "1e2d094a",
              "elType": "container",
              "isInner": true,
              "settings": {
                "content_width": "full",
                "flex_direction": "column",
                "flex_align_items": "center",
                "_flex_size": "none",
                "_flex_grow": 1,
                "padding": {"unit": "px", "top": "30", "right": "30", "bottom": "30", "left": "30", "isLinked": true}
              },
              "elements": [
                {
                  "id": "2f3e1a5b",
                  "elType": "widget",
                  "widgetType": "heading",
                  "isInner": false,
                  "settings": {
                    "title": "Online Sessions",
                    "header_size": "h3",
                    "align": "center",
                    "title_color": "#1A202C",
                    "typography_typography": "custom",
                    "typography_font_size": {"unit": "px", "size": 22, "sizes": []},
                    "typography_font_weight": "600",
                    "_margin": {"unit": "px", "top": "0", "right": "0", "bottom": "12", "left": "0", "isLinked": false}
                  },
                  "elements": []
                },
                {
                  "id": "304f2b6c",
                  "elType": "widget",
                  "widgetType": "text-editor",
                  "isInner": false,
                  "settings": {
                    "editor": "<p>Convenient therapy from the comfort of your own home.</p>",
                    "align": "center",
                    "text_color": "#718096",
                    "typography_typography": "custom",
                    "typography_font_size": {"unit": "px", "size": 16, "sizes": []},
                    "typography_line_height": {"unit": "em", "size": 1.6, "sizes": []}
                  },
                  "elements": []
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

**Key patterns in this example:**
- Outer container = `content_width: "full"` (background stretches full width)
- Inner container = `content_width: "boxed"` (content constrained to 1140px)
- Row of cards = inner container with `flex_direction: "row"` and `flex_direction_mobile: "column"` (stacks on mobile)
- Each card = nested container with `_flex_grow: 1` (equal widths)

---

## Example 3: CTA Banner with Background

### Input HTML

```html
<section style="background: linear-gradient(135deg, #2B6CB0, #2C5282); padding: 80px 20px; text-align: center;">
  <h2 style="color: white; font-size: 36px; font-weight: 700; margin-bottom: 16px;">Ready to Take the First Step?</h2>
  <p style="color: rgba(255,255,255,0.9); font-size: 18px; margin-bottom: 32px;">Get in touch today for a free 15-minute phone consultation.</p>
  <a href="tel:+441234567890" style="background: white; color: #2B6CB0; padding: 14px 28px; border-radius: 6px; font-weight: 600;">Call Now: 01onal 234 567890</a>
</section>
```

### Output JSON (Content-only format for _elementor_data)

```json
[
  {
    "id": "41503c7d",
    "elType": "container",
    "isInner": false,
    "settings": {
      "content_width": "full",
      "flex_direction": "column",
      "flex_align_items": "center",
      "background_background": "gradient",
      "background_color": "#2B6CB0",
      "background_color_b": "#2C5282",
      "background_gradient_angle": {"unit": "deg", "size": 135, "sizes": []},
      "padding": {"unit": "px", "top": "80", "right": "20", "bottom": "80", "left": "20", "isLinked": false},
      "padding_mobile": {"unit": "px", "top": "50", "right": "15", "bottom": "50", "left": "15", "isLinked": false},
      "html_tag": "section"
    },
    "elements": [
      {
        "id": "5261ad8e",
        "elType": "widget",
        "widgetType": "heading",
        "isInner": false,
        "settings": {
          "title": "Ready to Take the First Step?",
          "header_size": "h2",
          "align": "center",
          "title_color": "#FFFFFF",
          "typography_typography": "custom",
          "typography_font_size": {"unit": "px", "size": 36, "sizes": []},
          "typography_font_size_tablet": {"unit": "px", "size": 28, "sizes": []},
          "typography_font_size_mobile": {"unit": "px", "size": 24, "sizes": []},
          "typography_font_weight": "700",
          "_margin": {"unit": "px", "top": "0", "right": "0", "bottom": "16", "left": "0", "isLinked": false}
        },
        "elements": []
      },
      {
        "id": "6372be9f",
        "elType": "widget",
        "widgetType": "text-editor",
        "isInner": false,
        "settings": {
          "editor": "<p>Get in touch today for a free 15-minute phone consultation.</p>",
          "align": "center",
          "text_color": "rgba(255,255,255,0.9)",
          "typography_typography": "custom",
          "typography_font_size": {"unit": "px", "size": 18, "sizes": []},
          "_margin": {"unit": "px", "top": "0", "right": "0", "bottom": "32", "left": "0", "isLinked": false}
        },
        "elements": []
      },
      {
        "id": "7483cfa0",
        "elType": "widget",
        "widgetType": "button",
        "isInner": false,
        "settings": {
          "text": "Call Now: 01234 567890",
          "link": {
            "url": "tel:+441234567890",
            "is_external": "",
            "nofollow": ""
          },
          "align": "center",
          "background_color": "#FFFFFF",
          "button_text_color": "#2B6CB0",
          "border_radius": {"unit": "px", "top": "6", "right": "6", "bottom": "6", "left": "6", "isLinked": true},
          "typography_typography": "custom",
          "typography_font_weight": "600"
        },
        "elements": []
      }
    ]
  }
]
```

**Key patterns in this example:**
- Gradient background: `background_background: "gradient"` with `background_color` and `background_color_b`
- No inner boxed container needed (simple centered content, padding handles spacing)
- Content-only format (just the array) for WP-CLI / _elementor_data usage
- `tel:` link on button for phone CTA
