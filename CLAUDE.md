# CLAUDE.md

Guidance for Claude Code when working in this repository.

## Project

**Taartmarkt** — a Shopify storefront theme for a cake/bakery marketplace (NL). It is a
customized fork of Shopify's **Dawn** theme (`v15.3.0`, see `config/settings_schema.json` →
`theme_info`). The site is currently pre-launch and ships a **coming-soon page** feature
(see `.claude/specs/coming-soon-page/`).

This is a standard Shopify Liquid theme — there is no build step, package.json, or framework.
Changes are written directly to `.liquid`, `.json`, `.css`, and `.js` files and deployed with
the Shopify CLI.

## Repository layout

Standard Shopify theme structure:

- `layout/` — `theme.liquid` (global HTML shell; hosts the coming-soon activation logic) and
  `password.liquid`.
- `templates/` — JSON templates mapping pages to sections (`index.json`, `product.json`,
  `page.coming-soon.json`, etc.). `customers/` holds account templates.
- `sections/` — section Liquid files with `{% schema %}` blocks. ~55 sections, mostly stock
  Dawn plus customizations (`coming-soon-page.liquid`, `main-page.liquid`).
- `snippets/` — reusable Liquid partials. Project-specific ones include
  `custom-cake-options.liquid`, `postcode-checker.liquid`, and the PageFly snippets
  (`pagefly-main-css.liquid`, `pagefly-main-js.liquid`).
- `assets/` — CSS, JS, images, icons (Dawn's component CSS lives here).
- `config/` — `settings_schema.json` (theme editor settings definitions) and
  `settings_data.json` (saved values; auto-managed by Shopify, avoid hand-editing).
- `locales/` — translation files (`*.json` for storefront, `*.schema.json` for editor labels).
  Stock Dawn keys are referenced as `t:...` in schemas.
- `.claude/specs/` — Claude-maintained feature specs (requirements / design / tasks +
  activation and testing guides), kept in sync with the **actual** implementation state.
  Originally ported from a Kiro spec that has since been removed.

## Working with this theme

### Local development & deploy

Requires the Shopify CLI (`@shopify/cli`, `@shopify/theme`). The user runs auth/dev commands
themselves — suggest the `! <command>` prefix in the prompt for interactive ones.

```bash
shopify theme dev            # local dev server (http://127.0.0.1:9292), live reload
shopify theme check          # lint/validate Liquid + schema
shopify theme push --development
shopify theme pull --development
```

Always run `shopify theme check` after editing Liquid or schema JSON.

### Conventions

- **Match Dawn idioms.** New sections follow Dawn's structure: asset stylesheet tags at top,
  an optional `{%- style -%}` block scoped by `#Section-{{ section.id }}` /
  `.section-{{ section.id }}`, the markup, then a `{% schema %}` block with `settings`,
  `blocks`, and `presets`. Look at a neighboring section before writing a new one.
- **Schema JSON is strict.** Trailing commas in a `{% schema %}` block break the theme.
  (Note: `sections/coming-soon-page.liquid` currently has a trailing comma after the last
  block in its schema — Shopify tolerates it, but don't copy that pattern.)
- **Theme settings vs. section settings.** Global settings live in `config/settings_schema.json`
  and are read via `settings.<id>`. Per-section settings live in the section's own schema and
  are read via `section.settings.<id>`; block settings via `block.settings.<id>`.
- **Liquid whitespace.** Use `{%-`/`-%}` and `{{-`/`-}}` to control whitespace as Dawn does.
- **CSS** is plain CSS in `assets/` (Dawn `component-*.css` / `section-*.css`). No preprocessor.
- **Don't hand-edit auto-generated files**: `config/settings_data.json` and JSON templates
  carry an "auto-generated / may be overwritten by the theme editor" header — prefer changing
  the section/schema and letting the editor regenerate, unless a deliberate code change is needed.

### Git

- Default working branch is `development`; PRs target `main`.
- `Update from Shopify for theme ...` commits are machine-generated syncs from the Shopify
  admin theme editor — expect interleaving with hand-authored commits.
- Commit messages follow Conventional Commits scoped to the feature, e.g.
  `feat(coming-soon-page): ...`, `refactor(coming-soon-page): ...`, `style(...): ...`.
- Only commit/push when the user asks.

## Coming-soon feature (current state)

The marquee custom feature. Full detail in `.claude/specs/coming-soon-page/`. In brief:

- **Activation** lives in `layout/theme.liquid` (the `{%- liquid ... -%}` block near the top
  computes `coming_soon_active` / `bypass_coming_soon`; redirection + SEO meta tags follow).
  Toggled by `settings.coming_soon_enabled`.
- **Bypass conditions** (no redirect): password page, Shopify design/preview mode,
  `admin`/`staff` customer tags, `/admin`, `/api/`, `/webhooks/`, cart/checkout, `/policies/`,
  and the coming-soon page itself (loop guard).
- **Presentation**: `sections/coming-soon-page.liquid` rendered via the page template
  `templates/page.coming-soon.json` at `/pages/coming-soon`. Redirection points there.
- **Blocks**: heading, paragraph, `hubspot_form` (reads `settings.hubspot_embed_code`),
  `baker_description`, `baker_registration` (links to `settings.baker_registration_url`,
  opens in a new tab).
- The standalone `templates/coming-soon.json` and a dedicated password-link block from the
  original plan were **removed**; the page-template approach replaced them.

When changing this feature, keep `.claude/specs/coming-soon-page/tasks.md` updated.
