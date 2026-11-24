# Scheduler UI — design system

Plain CSS, no build step, no framework. Works with vanilla HTML/JS, React, Vue, etc.

| File | What it is |
|---|---|
| `tokens.css` | All design values as CSS custom properties (color, type, spacing, radius, shadow, sizes, motion) + light/dark themes. |
| `components.css` | All component classes (`ui-*`). Requires `tokens.css`. |
| `demo.html` | Reference app page (shell + calendar + forms + toast + dark mode). Copy patterns from here. |

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap">
<link rel="stylesheet" href="design-system/tokens.css">
<link rel="stylesheet" href="design-system/components.css">
```

Preview: `python3 -m http.server 8765 --directory design-system` → http://localhost:8765/demo.html

---

## Core rules

1. **Never hard-code values.** Use `var(--token)` for every color, spacing, radius, shadow, font size. If a value is missing, add a token to `tokens.css` — don't inline it.
2. **Classes are prefixed `ui-`**, BEM-style: block `ui-card`, element `ui-card__title`, modifier `ui-btn--soft`.
3. **Color = tone class.** Add one to any toned component (button, badge, status, alert, avatar, progress, stat icon, dot, toast, calendar event):
   `tone-primary` (default) · `tone-secondary` · `tone-success` · `tone-warning` · `tone-danger` · `tone-info` · `tone-neutral`
4. **State = ARIA attribute**, not a class:
   `aria-current="page"` (nav, pagination, breadcrumb) · `aria-pressed` (segmented) · `aria-selected` (tabs) · `aria-checked` (switch) · `aria-invalid="true"` (fields) · `disabled`.
   Layout state uses `is-*` classes: `is-open`, `is-collapsed`, `is-today`, `is-weekend`.
5. **Dark mode:** `<html data-theme="dark">`. Components need nothing extra.
6. **Icons:** inline SVG with `stroke="currentColor"`, class `ui-icon` (20px) or `ui-icon--sm` (16px); decorative icons get `aria-hidden="true"`. Icon-only buttons need `aria-label`.

## Tokens (most used)

| Group | Tokens |
|---|---|
| Surfaces | `--bg-page` `--bg-surface` `--bg-subtle` `--bg-input` `--bg-input-muted` `--bg-disabled` |
| Text | `--text-primary` `--text-secondary` `--text-muted` `--text-disabled` `--text-label` `--text-link` |
| Lines | `--border` (cards) `--divider` (rows) `--input-stroke` `--focus-stroke` |
| Families | `--{primary,secondary,success,warning,danger,info}-{solid,solid-fg,hover,pressed,surface,border,text}` |
| Chart/calendar fills | `--accent-1` … `--accent-7` (fills only, never text) |
| Type | `--text-{3xl,2xl,xl,lg,md,base,sm,xs,2xs}` with matching `--leading-*`; `--weight-{regular,medium,semibold,bold}` |
| Spacing (4px grid) | `--space-1` 4 · `-2` 8 · `-3` 12 · `-4` 16 · `-5` 20 · `-6` 24 · `-8` 32 · `-10` 40 · `-12` 48 · `-16` 64 |
| Radius | `--radius-xs` 4 · `-sm` 6 · `-md` 8 (controls, cards) · `-lg` 12 (modal, toast) · `-full` |
| Elevation | `--shadow-card` `--shadow-raised` `--shadow-overlay` · `--focus-ring` `--focus-ring-strong` `--focus-ring-danger` |
| Sizes | `--control-{sm 40, md 44, lg 56}` · `--avatar-{sm,md,lg,xl}` · `--navbar-height` 72 · `--sidebar-width` 275 · `--sidebar-collapsed` 86 |
| Motion | `--duration-fast` `--duration-base` `--ease-standard` |

Breakpoints (reference, not variables): sm 576 · md 768 · lg 992 · xl 1200 · xxl 1400. Sidebar becomes a drawer below 992px.

## Components

### Actions
```html
<button class="ui-btn">Save</button>
<button class="ui-btn ui-btn--soft tone-success">Approve</button>
<button class="ui-btn ui-btn--icon ui-btn--ghost" aria-label="Close"><svg class="ui-icon">…</svg></button>
```
- `ui-btn` variants: `--soft` `--outline` `--neutral` `--ghost` `--dark` `--danger-soft` · sizes `--sm` `--lg` · shape `--pill` `--round` · `--block` · icon-only `--icon`
- `ui-btn-group` — joined buttons
- `ui-segmented` (+ `--sm`) — view switcher; buttons use `aria-pressed`
- `ui-icon-btn` (+ `ui-icon-btn__dot`) — round navbar button
- `ui-link` — inline text link

### Forms
```html
<div class="ui-field">
  <label class="ui-label" for="t">Title <span class="ui-label__optional">(optional)</span></label>
  <input class="ui-input" id="t" aria-invalid="true">
  <p class="ui-hint ui-hint--error">Required</p>
</div>
```
- `ui-input` / `ui-select` / `ui-textarea` — sizes `--sm` `--lg`; `ui-input--muted` for search
- `ui-input-icon` (+ `--end`) — icon inside the field
- `ui-input-group` + `ui-addon` — prefix/suffix
- `ui-check` — wraps a native checkbox/radio: `<label class="ui-check"><input type="checkbox"> Text</label>`
- `ui-fieldset` — groups of checks
- `ui-switch` — `<button class="ui-switch" role="switch" aria-checked="true" aria-label="…">`; toggle `aria-checked` in JS. `ui-switch-row` = label + switch row

### Status & feedback
- `ui-badge` (+ `--soft` `--outline` `--sm`) + tone
- `ui-status` + tone — pill with dot ("Confirmed")
- `ui-count` — red numeric counter · `ui-dot` + tone — color dot
- `ui-alert` + tone — `__icon` `__content` `__title` `__body` `__close`; `role="status"` (or `role="alert"` for urgent errors)
- `ui-toast` + tone inside `<div class="ui-toast-region" aria-live="polite">` — `__icon` `__message`
- `ui-tooltip` — static bubble (positioning is up to you)
- `ui-progress` (+ `--sm`) + tone, child `ui-progress__bar` with `style="width:48%"`, `role="progressbar"` + aria-value*; `ui-progress-label`
- `ui-avatar` (+ `--sm` `--lg` `--xl`) + tone, initials or `<img>`; `ui-avatar__presence`; `ui-avatar-group`
- `ui-empty` — `__icon` `__title` `__body`

### Navigation
- `ui-tabs` (+ `--pills`) — `role="tablist"`, buttons with `role="tab" aria-selected`
- `ui-pagination` — links with `aria-current="page"`
- `ui-breadcrumb` — `<ol>` inside `<nav aria-label="Breadcrumb">`, last `<li aria-current="page">`

### Content
- `ui-card` (+ `--padded`) — `__header` `__title` `__body` `__footer`
- `ui-stat` (on a `ui-card`) — `__top` `__label` `__value` `__icon` (+ tone) `__meta`
- `ui-table-wrap` > `ui-table` (+ `--borderless`); cells `is-numeric` `is-actions`; `ui-cell-person` (`__name` `__meta`)
- `ui-list` — agenda rows: `__item` `__main` `__title` `__meta` `__aside`
- `ui-modal` — on a native `<dialog>`: `__header` `__title` `__body` `__footer`; open with `dialog.showModal()`
- `ui-grid` — responsive auto-fit card grid; set `--ui-grid-min` (default 220px)

### App shell
```html
<div class="ui-shell">
  <aside class="ui-sidebar">            <!-- .is-collapsed = icon rail; .is-open = mobile drawer -->
    <div class="ui-sidebar__brand"><span class="ui-sidebar__logo">…</span><span class="ui-sidebar__name">App</span></div>
    <nav class="ui-nav" aria-label="Main">
      <div class="ui-nav__group">
        <span class="ui-nav__title">Schedule</span>
        <a class="ui-nav__link" href="#" aria-current="page"><svg class="ui-icon" aria-hidden="true">…</svg><span>Calendar</span></a>
      </div>
    </nav>
  </aside>
  <div class="ui-sidebar-backdrop"></div>   <!-- click closes the drawer -->
  <div class="ui-shell__main">
    <header class="ui-navbar"><div class="ui-navbar__start">…</div><div class="ui-navbar__end">…</div></header>
    <main class="ui-content">
      <div class="ui-page-header"><h1 class="ui-page-title">Calendar</h1><nav aria-label="Breadcrumb">…</nav></div>
      …
    </main>
  </div>
</div>
```

### Calendar (week view)
```html
<div class="ui-cal" style="--cal-start: 8; --cal-hours: 10">   <!-- 08:00, 10 hours tall -->
  <div class="ui-cal__head"><span></span>
    <div class="ui-cal__day is-today"><span class="ui-cal__dow">Mon</span><span class="ui-cal__num">24</span></div> …×7
  </div>
  <div class="ui-cal__body">
    <div class="ui-cal__times"><span style="--h:0">08:00</span> …</div>
    <div class="ui-cal__col">               <!-- add is-weekend for Sat/Sun -->
      <a class="ui-event tone-secondary" style="--start: 10; --end: 11.5">
        <span class="ui-event__title">Standup</span><span class="ui-event__time">10:00–11:30</span>
      </a>
    </div> …×7
  </div>
</div>
```
`--start` / `--end` are absolute hours; decimals allowed (13.5 = 13:30).

### Utilities
`ui-sr-only` (screen-reader-only text) · `ui-icon` / `ui-icon--sm`. Reduced motion is respected automatically.
