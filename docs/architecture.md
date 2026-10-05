# ds-base — Architecture

```
ds-base
│
├── src/                  # Design System core (public API)
│   ├── tokens/           # primitives → semantic → brand slots
│   ├── functions/        # ds.color, ds.space, ds.font-size, …
│   ├── mixins/           # ds.mq, ds.rounded, ds.text-style, …
│   ├── base/             # reset + :root Custom Properties
│   ├── helpers/          # ds-* utility classes
│   ├── fonts/            # @font-face generator (local/remote)
│   └── icons/            # icon contract (.ds-icon) only
│
├── adapters/             # optional integrations (never auto-imported)
│   ├── icons/            # icon providers (bootstrap/)
│   └── liferay/          # Clay / Style Book / fragments bridge
│
├── docs/
│   └── token-reference/  # visual reference + token smoke test
│
└── dist/                 # generated artifacts (gitignored)
```

Token flow:

```mermaid
flowchart LR
  A[primitives<br/>src/tokens] --> B[semantic<br/>src/tokens]
  B --> C[":root CSS<br/>Custom Properties"]
  C --> D["runtime API<br/>var(--token)"]
  B --> E["Sass API<br/>ds.color() / ds.space() / …"]
  E --> F["build-time<br/>consumers"]
  C --> G["docs/token-reference"]
```

Rules:

- `src/` never imports `adapters/` or `docs/`.
- Adapters are opt-in: `@use ".../adapters/..."`.
- Generated output (`dist/`, `docs/token-reference/ds-base.css`)
  is never a source of truth — always rebuild from SCSS.
