# Coming Soon Page — Tasks

> Claude-maintained spec, ported from the original Kiro spec (since removed). Status reflects
> the **actual codebase** as of 2026-06-13, which differs from the original plan. Keep this
> updated when the feature changes.

## Done

- [x] **1. Core settings + render target**
  - "Coming Soon Page" settings group in `config/settings_schema.json`.
  - `templates/page.coming-soon.json` renders the `coming-soon-page` section at
    `/pages/coming-soon`. _(R1.1, R5.1, R5.2)_

- [x] **2. Responsive presentation section**
  - `sections/coming-soon-page.liquid` with background image (srcset), overlay opacity,
    image-height presets, 9-position content grid, desktop/mobile alignment, color scheme.
    _(R1.2, R1.3, R1.4)_

- [x] **3. HubSpot form block**
  - `hubspot_form` block renders `settings.hubspot_embed_code`; hidden when blank. _(R2.1–R2.2)_

- [x] **4. Baker registration button**
  - `baker_registration` block links to `settings.baker_registration_url`, opens in a new tab
    with `rel="noopener noreferrer"` and an accessible label; hidden when URL blank. _(R3.1–R3.4)_

- [x] **4b. Baker description block** _(added beyond original plan)_
  - `baker_description` richtext block pitching baker signup above the button.

- [x] **6. Coming-soon mode activation system**
  - Detection + bypass logic, JS/meta-refresh redirection, SEO/OG/Twitter/JSON-LD meta, and
    redirect-away-when-disabled — all in `layout/theme.liquid`. Bypass list includes
    `/policies/` (uncommitted in working tree as of this writing). _(R1.1, R4.1–R4.3)_

## Removed / changed vs. plan

- [~] **5. On-page password link** — **removed**. The discreet password-link block was dropped;
  admin access relies on bypass conditions + direct `/password`. `password_link_text` setting
  remains but is unused. _(R4 satisfied via bypass instead.)_
- [~] Standalone `templates/coming-soon.json` — **removed** in favor of the page template.

## Open / not yet implemented

- [ ] **7. Richer error handling & validation**
  - Friendlier fallback/placeholder content for missing HubSpot embed or invalid/blank baker
    URL (currently blocks just render nothing); validation hints in the editor. _(R2, R3.1, R5.1)_

- [ ] **8. Accessibility & performance polish**
  - Verify heading hierarchy and ARIA across all blocks, full keyboard navigation, screen
    reader states; lazy-loading is on the banner image but confirm LCP/perf. _(R1.4, R2.1, R3.4)_

## Notes for whoever picks this up

- After any edit, run `shopify theme check`.
- `sections/coming-soon-page.liquid` schema has a trailing comma after the last block — valid
  here but don't propagate the pattern.
- Test the bypass matrix in an incognito window with `coming_soon_enabled` on (design-mode
  preview always bypasses, so it won't show the redirect there).
