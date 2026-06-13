# Coming Soon Page — Design

> Claude-maintained spec, ported from the original Kiro spec (since removed) and updated to
> match the shipped implementation (which diverged from the original plan — see "Divergences").

## Overview

Coming-soon mode is a global toggle that redirects storefront visitors to a dedicated
coming-soon page. It is built on Dawn's image-banner / email-signup-banner patterns and is
fully configurable from the theme editor.

## Architecture

### Files

| Concern | File |
| --- | --- |
| Activation + redirect + SEO meta | `layout/theme.liquid` |
| Presentation section | `sections/coming-soon-page.liquid` |
| Page template (render target) | `templates/page.coming-soon.json` → `/pages/coming-soon` |
| Global settings | `config/settings_schema.json` ("Coming Soon Page" group) |

### Activation mechanism (`layout/theme.liquid`)

A `{%- liquid -%}` block in `<head>` computes two flags:

- `coming_soon_active` = `settings.coming_soon_enabled`.
- `bypass_coming_soon` = true for any of: password page (`request.page_type == 'password'`,
  template/path contains `password`), `Shopify.designMode` or `request.preview_theme_id`,
  `customer.tags` containing `admin`/`staff`, being already on a `coming-soon` template/path
  (redirect-loop guard), `/admin`, `/api/`, `/webhooks/`, `cart`/`checkout` page types, and
  `/policies/`.

When `coming_soon_active and bypass_coming_soon == false`:
- The `<title>` becomes `Coming Soon - {{ shop.name }}`.
- SEO/social meta are emitted (`noindex/nofollow`, Open Graph, Twitter Card, JSON-LD
  structured data) using `coming_soon_heading` / `coming_soon_description` and the background
  image.
- In `<body>`, a redirect script (JS primary + behavior in design mode logs debug info)
  sends visitors to the resolved coming-soon page URL — the `pages['coming-soon']` page if it
  exists, else a page whose `template_suffix == 'coming-soon'`, else `/pages/coming-soon`.
- The `coming-soon-mode` body class is added only when actually on the coming-soon page.
- If mode is **disabled** but a visitor is on a `coming-soon` template, JS redirects them home.

### Presentation (`sections/coming-soon-page.liquid`)

A banner-style section (reuses Dawn's `section-image-banner.css`, `component-newsletter.css`,
`section-email-signup-banner.css`). Supports background image with srcset, overlay opacity,
image height presets, a 9-position content grid, desktop/mobile alignment, an optional text-box
background, and color scheme.

Block-based content (`section.blocks`), each `limit: 1`:

- `heading` — `inline-richtext` H1 with selectable size.
- `paragraph` — richtext description, body/subtitle style.
- `hubspot_form` — renders `settings.hubspot_embed_code`; hidden when blank.
- `baker_description` — richtext block pitching baker signup.
- `baker_registration` — `<a class="button button--primary">` to `settings.baker_registration_url`,
  `target="_blank" rel="noopener noreferrer"`, accessible label; hidden when URL blank. Button
  text comes from `block.settings.button_text` falling back to `settings.baker_button_text`.

A `presets` block wires all five blocks in order for one-click insertion.

## Data model — theme settings

The "Coming Soon Page" group in `config/settings_schema.json` (read via `settings.<id>`):

```
coming_soon_enabled        checkbox  (default false)   — master toggle
coming_soon_heading        text                        — SEO/meta heading
coming_soon_description    richtext                    — SEO/meta description
hubspot_embed_code         textarea                    — HubSpot form embed
baker_registration_url     url                         — Google Form URL
baker_button_text          text                        — default button label
password_link_text         text                        — (legacy; block removed)
coming_soon_background     image_picker                — meta/OG image
coming_soon_overlay_opacity range 0–100 (default 40)
coming_soon_color_scheme   color_scheme (default scheme-1)
```

Note the section also has its own `image`, `image_overlay_opacity`, etc. used for the visible
banner; the global `coming_soon_*` visual settings primarily feed the SEO/meta tags.

## Divergences from the original Kiro plan

- **Render target changed**: the planned standalone `templates/coming-soon.json` was removed;
  the page-template `templates/page.coming-soon.json` (`/pages/coming-soon`) is used instead.
- **Password-link block removed**: the discreet on-page password link block was dropped. Admin
  access relies on the bypass list + direct `/password`. The `password_link_text` setting
  remains but is unused.
- **Baker description block added**: a `baker_description` block (not in the original plan) was
  added to pitch baker signup above the registration button.
- **Policy-pages bypass added**: `/policies/` was added to the bypass conditions.

## Error handling

- Blank HubSpot embed / baker URL → the corresponding block renders nothing (no broken UI).
- Redirect-loop guard prevents bouncing once on the coming-soon page.
- Meta-refresh / JS dual approach in `theme.liquid` for the redirect.

## Testing

See [`testing-guide.md`](./testing-guide.md) for the full manual test matrix
(activation toggle, bypass conditions, HubSpot embed, baker button, responsive, a11y,
cross-browser). Validate Liquid/schema with `shopify theme check` and exercise locally with
`shopify theme dev`.
