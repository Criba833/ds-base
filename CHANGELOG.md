# Changelog

## 0.1.0 — 2026-10-05

First release of `@spirocode/ds-base` (extracted from
the previous application's Design System).

### Added

- Token foundation: primitives, semantic, spacing,
  typography, breakpoints, radius, shadows, motion.
- Sass functions: `color()`, `space()`, `font-size()`,
  `font-weight()`, `radius()`.
- Sass mixins: `mq()`, `rounded()`, color/spacing/
  typography/effect mixins.
- `:root` generation of CSS Custom Properties from
  tokens (runtime API).
- Base/reset styles re-enabled in the main entry.
- Public helpers with `ds-` namespace.
- Configurable brand slots (`$brand-primary`,
  `$brand-secondary`).
- Icon contract (`.ds-icon`, provider-agnostic) — the
  core ships no icon set.
- `README.md`, `CHANGELOG.md`, `.gitignore`.
- Build scripts: `pnpm build` → `dist/ds-base.css`,
  `pnpm build:reference` → Token Reference CSS.
- `docs/token-reference/` — visual token reference /
  smoke test (plain HTML + CSS, no framework, no new
  dependencies; minimal vanilla JS reads computed
  values). Brand slots configured in its
  `reference.scss`; primitives exported as `--ref-*`
  generated from `$color-primitives`.

### Changed

- Package renamed to `@spirocode/ds-base`.
- Source flattened: `src/styles/*` → `src/*`.
- All public helpers renamed with `ds-` prefix
  (`.text-body` → `.ds-text-body`, `.m-4` → `.ds-m-4`, …).
- `ds.color("primary"|"secondary")` now fails with an
  explicit error until the brand slots are configured.

### Moved

- Liferay adapter (Clay/StyleBook bridge,
  `section[fragment]`) → `adapters/liferay/`
  (outside the public core).
- Bootstrap Icons provider (font-face config, icon map)
  and its font assets → `adapters/icons/bootstrap/`
  (optional icon provider, outside the core).

### Fixed

- `body-lg` fluid clamp had `max < min`
  (`clamp(1rem, 0.9rem + 0.2vw, 0.9rem)` → constant 16px,
  smaller than `body`). Now
  `clamp(1.125rem, 1.05rem + 0.3vw, 1.25rem)`, slotting
  between `body` and `heading-xs` following the existing
  scale pattern (only token touched).

### Removed

- `.DS_Store` files.
- Redundant entry points (`styles.scss`,
  `src/styles/index.scss`, `components/_index.scss`).
- Previous-app reference code (`legacy/`): `.ds-btn` /
  `.ds-card`, `.ds-grid` / `.ds-section` scaffold and
  `blue-gou` / `green-gou` brand values — verified
  unreferenced before deletion; preserved in git history.
