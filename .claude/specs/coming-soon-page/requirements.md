# Coming Soon Page — Requirements

> Claude-maintained spec, ported from the original Kiro spec (since removed). Kept in sync with
> the actual implementation.

## Introduction

A coming-soon landing page for the pre-launch Taartmarkt Shopify site. While the main site is
under construction it captures visitor interest via a HubSpot email subscription form, lets
prospective bakers register through an external Google Form, and preserves password-protected
access for administrators. Coming-soon mode is toggled from theme settings and gates the whole
storefront via redirection logic in `layout/theme.liquid`.

## Requirements

### R1 — Coming-soon landing page
As a visitor to the under-construction site, I want a coming-soon page so I know the site is
temporarily unavailable but launching soon.

1. WHEN coming-soon mode is enabled THEN non-bypassed visitors SHALL be redirected to the
   coming-soon page instead of regular site content.
2. The page SHALL clearly communicate that the site is under construction.
3. The design SHALL reflect the brand (background image, color scheme, configurable copy).
4. The page SHALL be responsive across mobile, tablet, and desktop.

### R2 — Email subscription (HubSpot)
As a potential customer, I want to subscribe for launch notifications.

1. The page SHALL display a HubSpot form when an embed code is configured.
2. WHEN no embed code is configured THEN the form block SHALL be hidden (no broken UI).
3. Email validation and submission are handled by HubSpot's embedded form.
4–5. Success/error handling is delegated to the HubSpot form widget.

### R3 — Baker registration (Google Form)
As a prospective baker, I want to register my interest.

1. The page SHALL display a "Baker Registration" button when a form URL is configured.
2. WHEN clicked THEN the button SHALL open the Google Form in a new tab (`target="_blank"`,
   `rel="noopener noreferrer"`).
3. The original coming-soon tab SHALL remain open.
4. The button SHALL carry an accessible label indicating it opens in a new tab.

### R4 — Password / admin access
As a site administrator, I want to reach the full site while coming-soon mode is on.

1. The Shopify password page (`/password`) SHALL always bypass coming-soon mode.
2. Design/preview mode, `admin`/`staff` tagged customers, `/admin`, `/api/`, `/webhooks/`,
   cart/checkout, and `/policies/` SHALL also bypass.
3. Authorized users entering the correct password SHALL reach the full site normally.

> Note: the dedicated in-page "password link" block from the original Kiro plan (R4.1's
> discreet on-page link) was **removed** during implementation. Admin access relies on the
> bypass conditions and direct `/password` access instead.

### R5 — Configurability via theme settings
As the site owner, I want to configure the page without code changes.

1. Heading, description, button text, background image, overlay opacity, and color scheme
   SHALL be editable in the theme editor.
2. The HubSpot embed code SHALL be configurable in theme settings.
3. The Google Form URL SHALL be configurable in theme settings.
4. Changes SHALL apply immediately on save.

Global settings live under "Coming Soon Page" in `config/settings_schema.json`:
`coming_soon_enabled`, `coming_soon_heading`, `coming_soon_description`, `hubspot_embed_code`,
`baker_registration_url`, `baker_button_text`, `password_link_text`, `coming_soon_background`,
`coming_soon_overlay_opacity`, `coming_soon_color_scheme`.
