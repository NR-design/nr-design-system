# Layout tokens: context and usage rules

Goal: This document describes the layout tokens (breakpoints, container widths and page widths) and the rules for using them. It is written for both people and AI agents. Space between and inside elements is in `spacing-tokens.md`. Component dimensions are in `sizing-tokens.md`.

Status (2026-11-24): **target state.** Tailwind CSS v4.3.5 is installed and its default theme is the primitive tier. The semantic tokens in section 4 are **proposed** and have not been generated yet: no layout token files exist. Where code or Figma disagree with this document, this document is the target.

## 1. Files

| File | Tier | Contents |
| --- | --- | --- |
| Tailwind default theme (`tailwindcss/theme.css`) | Primitive | Breakpoints (`--breakpoint-*`) and container widths (`--container-*`). **Referenced, not recreated.** |
| `tokens/primitives/layout.json` | Primitive | Does not exist yet. If generated, it is derived from the Tailwind theme at build time and never hand-authored |
| `tokens/semantic/layout.json` | Semantic | 3 role-based tokens (section 4). All are aliases to primitive container widths |

There is one semantic file. Layout has no light and dark modes (section 6).

## 2. Format and structure

- **Format:** W3C Design Tokens Community Group (DTCG) 2025.10. Dimensions are objects: `{ "value": 1280, "unit": "px" }`.
- **Naming:** the token path is the identifier, as in color and spacing. Path example: `layout.page`. Reference form: `{layout.page}`.
- **Primitive paths:** semantic tokens alias a container width as `{container.7xl}`. Breakpoints are `{breakpoint.md}`.
- **Type:** `$type: dimension` is set once on the top-level group and inherited.
- **Descriptions:** every semantic token has a `$description` stating its purpose and where it is used.
- **CSS output:** each semantic token becomes a Tailwind theme variable in the `--container-*` namespace (section 5, rule 9). Path `layout.page` becomes `--container-page` and the utility `max-w-page`.

## 3. Primitive tier

Raw values only. **Use Tailwind's defaults exactly as shipped. Do not redefine or extend them.**

**Breakpoints** (mobile-first, `min-width`). These are used as variants (`md:`, `lg:`), not as values.

| Variant | Value | px |
| --- | --- | --- |
| `sm` | 40rem | 640 |
| `md` | 48rem | 768 |
| `lg` | 64rem | 1024 |
| `xl` | 80rem | 1280 |
| `2xl` | 96rem | 1536 |

**Container widths.** Used by `max-w-*`, `w-*` and container query variants (`@3xs` to `@7xl`).

| Name | rem | px | | Name | rem | px |
| --- | --- | --- | --- | --- | --- | --- |
| `3xs` | 16 | 256 | | `2xl` | 42 | 672 |
| `2xs` | 18 | 288 | | `3xl` | 48 | 768 |
| `xs` | 20 | 320 | | `4xl` | 56 | 896 |
| `sm` | 24 | 384 | | `5xl` | 64 | 1024 |
| `md` | 28 | 448 | | `6xl` | 72 | 1152 |
| `lg` | 32 | 512 | | `7xl` | 80 | 1280 |
| `xl` | 36 | 576 | | | | |

**Layout utilities** (`flex`, `grid`, `grid-cols-*`, `col-span-*`, `row-span-*`, `order-*`, `justify-*`, `items-*`, `w-full`, `h-dvh` and so on) are used as Tailwind defines them. They are not tokens.

Source: `node_modules/tailwindcss/theme.css`, Tailwind v4.3.5. If Tailwind is upgraded, re-check this section.

## 4. Semantic tier

Role-based aliases. **Components and agents reference only these tokens for page and content widths.** Tailwind has no semantic layout layer, so this tier is authored for this system. **All names and values below are proposed.**

### Width (maximum width of a layout region)

| Token | Step | Value | Utility | Use |
| --- | --- | --- | --- | --- |
| `layout.page` | `7xl` | 1280px | `max-w-page` | Maximum width of the page container. Pair with `space.gutter.*` for side padding. |
| `layout.content` | `3xl` | 768px | `max-w-content` | Reading and article content, long-form text, single-column pages. |
| `layout.narrow` | `md` | 448px | `max-w-narrow` | Forms, sign-in and other focused single-task layouts. |

Page container pattern: `mx-auto w-full max-w-page px-gutter-sm md:px-gutter-md lg:px-gutter-lg`. Padding is inside the box, so the content area is the maximum width minus both gutters.

## 5. Usage rules

1. **Mobile-first.** Write the base style for the smallest screen and add `sm:`, `md:`, `lg:`, `xl:` and `2xl:` variants for larger ones. Do not use `max-*:` variants to build desktop-first layouts.
2. **Default breakpoints only.** Use the five Tailwind breakpoints. Do not use arbitrary breakpoints (`min-[900px]:`) or add custom ones. Breakpoint variants are exempt from the semantic-only rule because they are conditions, not values.
3. **What changes at a breakpoint.** Layout structure (column count, direction, visibility) and the `gutter` and `section` spacing steps (see `spacing-tokens.md`, rule 8). Other spacing and sizing tokens do not change at breakpoints.
4. **Container queries for components.** A component that can be placed in containers of different widths (cards, tiles, sidebars) adapts with container query variants (`@container`, `@md:`), not viewport breakpoints.
5. **Page and content width.** Use `layout.page`, `layout.content` and `layout.narrow` for maximum widths. Do not use a container width (`max-w-7xl`) or an arbitrary value (`max-w-[1200px]`) directly in a component.
6. **Container widths are not spacing.** Never use a container width or a breakpoint value for padding, margin, gap or component size. Use the spacing and sizing tokens.
7. **Reading width.** Body text and long-form content sits in `layout.content` or narrower. Do not let paragraphs run the full page width.
8. **Grids.** Column tracks and spans (`grid-cols-*`, `col-span-*`, `row-span-*`) are chosen per layout with Tailwind utilities. Tile spacing uses `space.gap.*` (see `spacing-tokens.md`, rule 7).
9. **Tailwind.** Do not redefine `--breakpoint-*` or the default `--container-*` values. Add each semantic token to the theme as `--container-<name>` (for example `--container-page: 80rem`). Two consequences:
   - The `--container-*` namespace also generates `w-*` and container query variants, so `w-page` and `@page:` also compile. Use each token only for its stated purpose.
   - Default container widths (`max-w-7xl`) still compile, so rule 5 is enforced by convention and review, not by the build.
10. **Do not invent tokens.** If no token fits, say so and propose a new semantic token rather than using a nearby one or a primitive.

## 6. Modes and density

- Layout has one semantic file. Light and dark use identical values, so there is no mode switch and no mode in token paths.
- There is no density mode and no responsive token values. Responsive behavior comes only from rules 1 to 4.

## 7. Verification

Checked 2026-11-24 against the installed Tailwind v4.3.5.

- **Primitive values:** the breakpoints and container widths in section 3 match `node_modules/tailwindcss/theme.css`.
- **Semantic variables:** compiling theme variables `--container-page: 80rem` and `--container-content: 48rem` produced `max-w-page`, `max-w-content` and the container query variant `@page:flex` as expected.
- **Not verified:** the semantic token JSON, the generated CSS and any component use. None of these exist yet.

## 8. Known gaps

- **No generated files.** `tokens/primitives/layout.json` and `tokens/semantic/layout.json` do not exist, and no `tokens/dist` output or `src/styles` theme mapping is generated. The build tool for tokens is not chosen.
- **Semantic values are proposed.** Names, steps and the count (3 tokens) have not been reviewed against real pages.
- **No dialog, drawer or sidebar widths.** Overlay and side panel widths have no token yet. Add them when those components are built.
- **No lint enforcement.** Nothing yet blocks direct container widths (`max-w-7xl`), arbitrary values or arbitrary breakpoints.
- **No Figma variables or layout grids.** Figma needs a Layout collection and matching layout grids created from sections 3 and 4.
