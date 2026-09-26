# DESIGN.md — amitkumargope.github.io

A plain-text design system reference for this site, in the format popularized by
[Google Stitch's DESIGN.md spec](https://stitch.withgoogle.com/docs/design-md/overview/)
and the [awesome-design-md](https://github.com/VoltAgent/awesome-design-md) collection.
Drop this into context before asking an AI agent (or a future you) to add or restyle
a section — it should stay visually consistent without re-deriving the rules by eye.

## 1. Visual Theme & Atmosphere

Industrial-instrumentation / technical-schematic aesthetic. Dark graphite canvas,
copper and teal signal accents, IBM Plex Mono for anything that reads like telemetry
(dates, stack tags, section indices), IBM Plex Sans for prose. Corner brackets on
panels and the hero (`::before`/`::after`) mimic an oscilloscope or blueprint frame.
Sharp, square corners are the default everywhere structural (panels, grid cells,
stat blocks) — this is intentional and should not be softened. A very small radius
(`--radius: 2px`) is reserved only for small interactive affordances (buttons, chips,
badges) as a tactile hint, never for structural blocks.

Mood: precise, technical, quietly confident. Not playful, not corporate-generic.

## 2. Color Palette & Roles

| Token | Hex | Role |
|---|---|---|
| `--graphite` | `#10161A` | Base page background |
| `--graphite-2` | `#171F24` | Card / elevated surface background |
| `--graphite-3` | `#1D262C` | Reserved for a further-elevated surface (not yet used) |
| `--paper` | `#E9E5DA` | Primary text |
| `--paper-dim` | `#C9C5BA` | Secondary / body text |
| `--copper` | `#C2703D` | Primary accent (CTAs, current-item marks) |
| `--copper-bright` | `#D98A54` | Copper hover/active state, hero accent span |
| `--teal` | `#5FA8A0` | Secondary accent — focus ring, "current" states, section eyebrows |
| `--line` | `#2C383D` | Default hairline border |
| `--line-bright` | `#3C4C52` | Hover/emphasized border |
| `--muted` | `#7E8B89` | Tertiary text (meta, labels, timestamps) |
| `--danger-red` | `#B4543A` | Reserved, not currently used in a live component |
| `--ring` | `= var(--teal)` | Focus-visible outline color (semantic alias, not a new value) |

Rule: never introduce a new raw hex value in a component rule — add or reuse a
token in `:root` first. This keeps a global theme edit (e.g. re-tinting the accent)
a one-line change.

## 3. Typography Rules

- Sans: **IBM Plex Sans** (400, 500, 600 — only load weights actually used).
- Mono: **IBM Plex Mono** (400, 500 — only load weights actually used).
- Headings (`h1`–`h4`): weight 600, `letter-spacing: -0.01em`.
- Mono is used exclusively for: nav ticks, dates/timestamps, stack tags, metrics,
  section index labels (`01 / SUMMARY`), badges. If it reads like a label or a
  measurement, it's mono. If it reads like a sentence, it's sans.
- Hero `h2`: `clamp(2rem, 4vw, 3rem)`, `max-width: 16ch`.
- Body base: `16px` / `line-height: 1.6`.

## 4. Component Stylings

**Buttons (`.btn`)** — mono font, 1px border in `--line-bright`, transparent
background. Hover: border + text shift to copper. `.primary` variant fills solid
copper. Active/press: `translateY(1px)` + a global `opacity: 0.7` on `:active`
for tactile feedback (mirrors the press-state pattern used by shadcn/ui).

**Chips/badges (`.chip`, `.now-badge`)** — mono, small, bordered, `--radius: 2px`.

**Cards (`.project-card`, `.repo-card`)** — 1px `--line` border, `--graphite-2`
background, square corners. On hover: lift `translateY(-3px)` + `--shadow-md` +
border brightens to `--line-bright`. This is the one place elevation/depth is used
(Fluent Design's "Depth" principle) — reserve it for cards the user can act on
(has a link), not for static display blocks.

**Panels** — square corners always, with a 14px corner-bracket decoration
(`::before`/`::after`) reusing `--line-bright`. Never add `border-radius` here.

**Focus states** — every interactive element gets `outline: 2px solid var(--ring)`
at `3px` offset via `:focus-visible`. Never remove focus outlines without a
replacement that's at least as visible.

## 5. Layout Principles

- Two-column desktop shell: `300px` sticky sidebar + fluid main column.
- Section panels use consistent padding: `3rem 3.5rem` desktop, `1.5rem` mobile.
- Grids (`skill-grid`, `beyond-grid`, `project-grid`, `repo-grid`) use
  `repeat(auto-fit, minmax(Npx, 1fr))` rather than fixed column counts — content
  reflows by available width, not by breakpoint guesswork.
- Breakpoints in use: `700px` (hide portrait), `880px` (sidebar → mobile topbar),
  `900px` (two-column → single-column grids).

## 6. Depth & Elevation

Two shadow tokens only — resist adding more:

- `--shadow-sm: 0 1px 2px rgba(0,0,0,0.35)` — reserved for small floating chrome.
- `--shadow-md: 0 8px 24px -8px rgba(0,0,0,0.55)` — card hover lift.

Elevation communicates "this is clickable and about to respond," not decoration.
Static content (stat cells, skill cells, timeline) stays flat with borders only.

## 7. Do's and Don'ts

- Do reuse a `:root` token before adding a new color.
- Do keep mono reserved for label/metric/timestamp text, not prose.
- Do respect `prefers-reduced-motion` — the global rule already disables
  transitions/animations; new motion must degrade through that, not bypass it.
- Don't round the corners of panels, the hero, stat cells, or the corner-bracket
  decorations — that's the site's visual signature.
- Don't add a third accent color; copper (primary) and teal (secondary/focus)
  is the full palette.
- Don't ship a Google Fonts weight that isn't referenced by any CSS rule —
  audit `font-weight` usage before changing the `@import`/`<link>` weight list.

## 8. Responsive Behavior

- `< 700px`: portrait photo hidden (frame doesn't fit hero comfortably).
- `< 880px`: sidebar replaced by a sticky mobile topbar + slide-down nav;
  `scroll-padding-top` set so anchor jumps clear the sticky topbar.
- `< 900px`: `.about-grid`, `.two-col`, `.contact-grid` collapse to one column.
- Touch targets: nav links and buttons keep ≥ 0.4rem vertical padding minimum;
  mobile nav links use 0.85rem for a larger tap area.

## 9. Agent Prompt Guide

Quick reference for "build me a new section that matches this site":

> Dark graphite background (`#10161A`), copper (`#C2703D`) as the one primary
> accent, teal (`#5FA8A0`) for focus rings and secondary emphasis. IBM Plex Mono
> for labels/metrics/timestamps, IBM Plex Sans for prose. Square corners on
> structural blocks; a 2px radius only on small buttons/chips. Cards lift 3px
> with a soft shadow on hover; nothing else animates beyond that and the hero
> waveform canvas. No rounded pill buttons, no gradients, no drop-shadowed text.
