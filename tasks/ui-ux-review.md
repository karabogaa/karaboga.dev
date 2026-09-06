# UI/UX refresh — 2026-09-06

Implemented a wider portfolio layout, Manrope typography, blue/lavender light
theme, navy dark theme, an interactive iOS/Android device study, illustrated
project cards, a career timeline, technology groups, and a dedicated contact area.
The mobile header keeps section navigation visible. No framework or build step
was introduced. Original career facts were retained.

Added `cihattkaraboga@gmail.com` as an alternate address, including its own mailto
link and copy button. Both addresses are represented in the structured data.

Replaced the logo with a geometric bull mark. Updated the SVG, 16/32/48px ICO,
180/192/512px PNGs, 404 page, manifest colors, and 1200×630 social image.

## Verification

Executed locally in Chrome through Playwright:

- 320, 360, 390, 768, 1024, and 1440px widths × English/Turkish × light/dark:
  no horizontal overflow; navigation remains visible.
- Desktop screenshots in all language/theme combinations; mobile and 404 visual
  inspection; final logo and social-image inspection.
- axe WCAG A/AA scans: no detected violations on the home and 404 pages in both
  themes/languages; an additional mobile scan also passed.
- Theme and language switching, reload persistence, translated document title,
  shared language URLs, device preview switching, active section navigation,
  and every internal anchor passed.
- Both email buttons copy the correct address. Tested the failure path with denied
  clipboard access and confirmed the alternate address using Chrome's real clipboard.
- Content/navigation without JavaScript, disabled storage, keyboard skip link,
  and reduced-motion behavior passed.
- All 68 translation keys are covered; no stale translation keys remain.
- JSON-LD and manifest parse successfully; no JavaScript errors or failed asset
  requests occurred; `git diff --check` passed.

These checks used local Chrome; Safari and physical-device testing were not run.
No deployment or remote publication was performed.
