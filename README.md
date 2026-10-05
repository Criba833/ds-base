# @spirocode/ds-base

Design System foundation extracted from a working
Liferay project: **Sass at build time, CSS Custom
Properties at runtime.**

- Tokens (primitive → semantic)
- Sass functions and mixins
- Responsive helpers (`mq`)
- Typography, spacing, radius, shadow and motion
  utilities
- Base/reset styles
- CSS Custom Properties generated from tokens

ds-base ships **no visual identity**: `primary` and
`secondary` are configuration slots owned by the
consumer. It ships **no UI components** and **no
framework CSS** (no Tailwind, UnoCSS, shadcn/ui).

## Install

```bash
npm install @spirocode/ds-base
```

Requires Sass ^1.70.0 (peer dependency).

## Usage

Full entry point — Sass API **and** CSS output
(Custom Properties, base styles, helpers):

```scss
@use "@spirocode/ds-base";
```

API only — functions, mixins and tokens, **no CSS
output** (use when the full CSS is already loaded):

```scss
@use "@spirocode/ds-base/api" as ds;

.my-class {
  padding: ds.space(4);
  color: ds.color("text", "primary");
}
```

## Brand configuration

`primary` and `secondary` are slots. Point them at a
`[primitive, shade]` pair:

```scss
@use "@spirocode/ds-base" with (
  $brand-primary: ("blue", "500"),
  $brand-secondary: ("green", "500"),
);
```

Until configured: no `--color-primary` /
`--color-secondary` is emitted, `.ds-bg-primary` is
not emitted, and `ds.color("primary")` fails at
compile time with an explicit error.

The previous application's brand values
(`blue-gou` / `green-gou`) were removed from the core —
see git history for reference.

## Public helpers

All public helpers use the `ds-` namespace so they
cannot collide with Bootstrap, Clay or other
consumers:

| Helper | Output |
|---|---|
| `.ds-text-{size}` | `font-size` (caption, body, body-lg, heading-*) |
| `.ds-text-{primary\|secondary\|inverse}` | text color |
| `.ds-font-{weight}` | `font-weight` |
| `.ds-bg-{success\|error}` | background color |
| `.ds-bg-primary` | only when `$brand-primary` is set |
| `.ds-p-*`, `.ds-px-*`, `.ds-py-*` | padding |
| `.ds-m-*`, `.ds-mx-*`, `.ds-my-*` | margin |
| `.ds-gap-*`, `.ds-gap-x-*`, `.ds-gap-y-*` | gap |
| `.ds-radius-*` | `border-radius` |
| `.ds-even-columns` | responsive equal columns |
| `.ds-effect-lift`, `.ds-effect-scale` | interaction effects |

> `.ds-text-*` is shared by typography (font-size) and
> color helpers. Their keys never overlap, so both can
> coexist; keep this in mind when adding new tokens.

## Sass API (build time)

Functions: `ds.color()`, `ds.space()`, `ds.font-size()`,
`ds.font-weight()`, `ds.radius()`.

Mixins: `ds.mq()`, `ds.rounded()`, `ds.text-style()`,
`ds.line-clamp()`, `ds.text-color()`, `ds.background-color()`,
`ds.border-color()`, `ds.fill-color()`, `ds.stroke-color()`,
`ds.p()`, `ds.pt()`, `ds.pb()`, `ds.p-block()`,
`ds.p-block-end()`, `ds.p-inline()`, `ds.m()`, `ds.mt()`,
`ds.mb()`, `ds.gap()`, `ds.hover-lift()`,
`ds.hover-lift-shadow()`, `ds.hover-scale()`.

## Runtime API (CSS Custom Properties)

`--color-*`, `--spacing-*`, `--font-size-*`,
`--font-weight-*`, `--font-family-*`, `--radius-*`,
`--shadow-*`, `--motion-duration-*`, `--motion-easing-*`.

## Icons

ds-base core is **not tied to any icon library**. The
core defines only the **icon contract**; the consumer
picks a provider:

```
DS Core → Icon Contract → Provider / Adapter
                            ├── Lucide
                            ├── Google Material Symbols
                            ├── Bootstrap Icons (included adapter)
                            └── custom / future
```

### Icon contract (core)

`.ds-icon` — provider-agnostic visual rules:

- box `1em × 1em`, scales with inherited `font-size`
- inherits the surrounding text color (`currentColor`)
- `inline-block`, no flex shrink, vertically aligned

The core ships **no icon set** and does not assume how
a provider renders (SVG stroke, SVG fill, font, mask).
No `<Icon>` abstraction yet.

### Providers

Bootstrap Icons (inherited font set) ships as an
optional adapter — kept out of the core:

```scss
@use "@spirocode/ds-base/adapters/icons/bootstrap";
```

```html
<i class="bi bi-arrow-right ds-icon"></i>
```

Font files are in `adapters/icons/bootstrap/assets/`
(kept, documented — see its
[README](adapters/icons/bootstrap/README.md)).
Add Lucide/Google/custom providers as their own folders
under `adapters/icons/` — no core changes needed.

## Optional Liferay adapter

Clay / Style Book bridge and fragment wrapper. Not
imported by the core:

```scss
@use "@spirocode/ds-base/adapters/liferay";
```

## Token Reference

`docs/token-reference/` is a minimal visual smoke test
(plain HTML + CSS, no framework, no new dependencies —
only a few lines of vanilla JS to print computed
values). Every sample renders through real CSS Custom
Properties:

```bash
pnpm install
pnpm build            # dist/ds-base.css
pnpm build:reference  # docs/token-reference/ds-base.css
```

Then open `docs/token-reference/index.html`. Workflow:
change a primitive → rebuild → see the change live.

## Structure

```
.
├── index.scss            # public API + CSS output
├── api.scss              # Sass API only, no CSS
├── package.json
├── src/
│   ├── tokens/           # primitives, semantic, brand, spacing,
│   │                     # typography, breakpoints, radius,
│   │                     # shadows, motion
│   ├── functions/        # color, space, font-size, font-weight, radius
│   ├── mixins/           # mq, colors, spacing, typography, radius, effects
│   ├── base/             # reset + :root Custom Properties
│   ├── helpers/          # ds-* utility classes
│   ├── fonts/            # @font-face generator (local/remote)
│   ├── icons/            # icon CONTRACT only (.ds-icon, no icon set)
│   └── assets/fonts/     # Montserrat font files
├── adapters/
│   ├── liferay/          # optional Clay/StyleBook/Fragments bridge
│   └── icons/
│       └── bootstrap/    # optional Bootstrap Icons provider + assets
├── docs/
│   └── token-reference/  # visual token reference (plain HTML + CSS)
└── dist/                 # generated CSS (pnpm build, gitignored)
```

Roles: `src/` = Design System core · `adapters/` =
optional integrations (never imported by the core) ·
`docs/` = documentation and reference tools · `dist/` =
generated artifacts. See
[docs/architecture.md](docs/architecture.md).

## Fonts and icons deployment

- Local font provider emits `url('../../assets/fonts/…')`
  relative to the compiled CSS — arrange the output
  directory or use the `remote` provider
  (`$font-provider`, `$font-base-url`).
- The icon font files live in the adapter; the local icon
  provider emits `url('/icons/…')` (absolute) — serve them
  at `/icons/` or configure `$icon-provider` /
  `$icon-base-url` (see
  [adapters/icons/bootstrap/README.md](adapters/icons/bootstrap/README.md)).
- By default `$font-family-primary` is `system-ui`, so
  no `@font-face` is emitted at all.
