# Bootstrap Icons provider (optional adapter)

One concrete provider for the ds-base **icon contract**.
**Not part of the ds-base core** — the core ships no
icon set and is not tied to any icon library.

## Usage

```scss
@use "@spirocode/ds-base/adapters/icons/bootstrap";
```

```html
<i class="bi bi-arrow-right ds-icon"></i>
```

The core `.ds-icon` class (size, color inheritance,
layout, alignment) comes from `src/icons/_contract.scss`;
this adapter only renders the actual glyphs.

## Font files

| File | Source |
|---|---|
| `assets/bootstrap-icons.woff2` | inherited asset — **kept, not deleted** |
| `assets/bootstrap-icons.woff` | inherited asset — **kept, not deleted** |

Serve them at `/icons/` (default `$icon-provider: 'local'`),
or configure:

```scss
@use "@spirocode/ds-base/adapters/icons/bootstrap" with (
  $icon-provider: 'remote',   // any non-'local' value
  $icon-base-url: 'https://cdn.example.com/icons',
  // $icon-remote-slug: 'liferay-dm', // legacy: strips the
  //   extension dot (bootstrap-icons.woff2 →
  //   bootstrap-iconswoff2) for Delivery & Media URLs
);
```

## Configuration

| Variable | Default | Purpose |
|---|---|---|
| `$icon-provider` | `'local'` | `local` → `/icons/…`; other values → `$icon-base-url/…`; `'skip'` → no `@font-face` |
| `$icon-base-url` | `''` | base URL for remote providers |
| `$icon-set` | `'bootstrap'` | which set this adapter emits |
| `$icon-font-family` | `'bootstrap-icons'` | `font-family` used by `@font-face` |
| `$icon-font-display` | `block` | `font-display` |
| `$icon-remote-slug` | `'plain'` | remote URL slug style (see above) |

## Adding another provider

Create `adapters/icons/<provider>/` with its own rules
(Lucide, Google Material Symbols, custom SVG sets…).
No core token or architecture changes are needed.
