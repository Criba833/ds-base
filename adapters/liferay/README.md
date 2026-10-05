# Liferay adapter

Optional integration for consumers running inside **Liferay**
(Clay / Style Book / Fragments).

**This adapter is NOT part of the ds-base core.** The core never
imports it. Import it explicitly only when needed:

```scss
@use "@spirocode/ds-base/adapters/liferay";
```

## Contents

| File | Purpose |
|---|---|
| `_bridge.scss` | Translates ds-base tokens into the CSS Custom Properties that Clay / Style Book consume (`--brand-color-*`, `--btn-*`, `--body-bg`, `--border-radius`, …). Values are not redefined — only names are bridged. |
| `_fragments.scss` | `section[fragment] { display: contents }` so the Liferay fragment preview wrapper stays layout-transparent. |

## Notes

- Out of scope (documented, not implemented): button hover/focus
  states (`--btn-primary-hover-*`), `--font-weight-bolder`, and
  Clay components compiled with fixed colors (forms, navigation,
  alerts, cards, dropdown, modal, table, badge, pagination,
  tooltip, breadcrumb, toast).
- This adapter was extracted from the previous application in
  FASE 2. Verify the variable contract against the target
  Liferay theme before relying on it.
