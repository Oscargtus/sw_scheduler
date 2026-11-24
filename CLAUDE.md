# time_scheduler

sw_scheduler: a scheduler for FAO student workers (calendar, shifts, bookings, team). The app itself is not built yet. The UI is defined by the design system in `design-system/`.

## Repository

- GitHub repository: `Oscargtus/sw_scheduler` (public), remote `origin`, branch `main`. This folder is the project root.
- Never commit the WowDash template folders (licensed third-party code) — keep them out of the repo.

## Design system — source of truth

- **Read `design-system/README.md` before writing any UI.** It lists every token and component class. Open `tokens.css` / `components.css` only when you need a detail the README doesn't cover.
- `design-system/demo.html` is the reference app page (shell, calendar, forms, toast, dark mode). Copy markup patterns from it.
- Load order on every page: Inter font → `tokens.css` → `components.css` → app CSS.

## UI rules

- Build UI from existing `ui-*` classes. Before writing new CSS, check the README for a component that already does it.
- No hard-coded colors, spacing, radii, font sizes or shadows in app code — use `var(--token)`. If a needed value doesn't exist, ask before adding a token.
- Color variants via `tone-*` classes; state via ARIA attributes (`aria-current`, `aria-pressed`, `aria-selected`, `aria-checked`, `aria-invalid`); layout state via `is-*` classes.
- New app-specific components: put them in the app's own CSS file, prefix them `app-`, and build them only from tokens. If one turns out reusable, propose moving it into `design-system/components.css` (prefix `ui-`) and documenting it in the README.
- Dark mode is `data-theme="dark"` on `<html>`; everything must work in both themes.
- Accessibility: icon-only buttons get `aria-label`, decorative icons `aria-hidden="true"`, keep visible focus (`--focus-ring*`).

## Editing the design system

- Do not change `design-system/tokens.css` or existing `ui-*` classes without asking — the whole app depends on them.
- When you add or change a component, update `design-system/README.md` and, if visual, `demo.html` in the same change.

## Out of scope

- The original WowDash admin templates were only used to extract the design system. Don't search, read or copy from them, even if they're still on disk.

## Local preview

```bash
python3 -m http.server 8765 --directory design-system
```
Then open http://localhost:8765/demo.html.
