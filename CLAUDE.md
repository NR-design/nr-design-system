# CLAUDE.md

## Purpose
NR Design System: a React + TypeScript component library built on headless
primitives (Base UI or Radix), styled with shadcn/ui patterns and Tailwind,
driven by W3C DTCG design tokens.

## Structure
```
.
├── tokens/
│   ├── primitives/        # Raw DTCG values (color, spacing, typography, radius)
│   ├── semantic/          # Purpose-named aliases that reference primitives
│   └── dist/              # GENERATED CSS vars + TS constants — never hand-edit
├── src/
│   ├── components/
│   │   ├── ui/            # Core building blocks on headless primitives, shadcn/ui-styled
│   │   ├── extended/      # Wrappers around single-purpose libs (Sonner, Vaul, cmdk, NumberFlow)
│   │   └── motion/        # Animated / showcase components
│   ├── styles/            # Global CSS + Tailwind theme variable mappings
│   └── lib/               # Utilities (e.g. class-name merging)
├── docs/                  # Component docs and usage guidelines
├── CLAUDE.md
└── README.md
```

## Token flow
`tokens/primitives` → `tokens/semantic` → `tokens/dist`

1. **Primitives** hold raw values only (e.g. `color.blue.500`).
2. **Semantic** tokens alias primitives by intent (e.g. `color.bg.accent → {color.blue.500}`).
   Components consume semantic tokens, never primitives directly.
3. **Dist** is build output (CSS custom properties + TS constants). Regenerate; don't edit.

`src/styles/` maps the generated CSS vars into the Tailwind theme.

## Import rules
- `extended/` and `motion/` may import from `ui/`.
- `ui/` must never import from `extended/` or `motion/`.
- `extended/` and `motion/` do not import from each other.
- Any component folder may import from `src/lib/`; `src/lib/` imports no components.

## Rules
- New files go in an existing directory above; structure changes require
  updating this file first.
- Don't add dependencies without asking.
