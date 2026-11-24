# Spacing tokens: context and usage rules

Goal: This document describes the spacing token files and the rules for using them. It is written for both people and AI agents. It is the spacing counterpart to `color-tokens.md` and follows the same tier model (primitive → semantic). Component dimensions are in `sizing-tokens.md`. Breakpoints and page widths are in `layout-tokens.md`.

Status (2026-11-24): **target state.** Tailwind CSS v4.3.5 is installed and its default theme is the primitive tier. The semantic tokens in section 4 are **proposed** and have not been generated yet: `tokens/primitives/spacing.json` is empty and no semantic spacing file exists. Where code or Figma disagree with this document, this document is the target.

## 1. Files

| File | Tier | Contents |
| --- | --- | --- |
| Tailwind default theme (`tailwindcss/theme.css`) | Primitive | The `--spacing` base unit and the scale it generates. **Referenced, not recreated.** |
| `tokens/primitives/spacing.json` | Primitive | Empty today. If generated, it is derived from the Tailwind theme at build time and never hand-authored |
| `tokens/semantic/spacing.json` | Semantic | 24 role-based tokens (section 4). All are aliases to primitive steps |

There is one semantic file. Spacing has no light and dark modes (section 6).

## 2. Format and structure

- **Format:** W3C Design Tokens Community Group (DTCG) 2025.10. Dimensions are objects: `{ "value": 16, "unit": "px" }`.
- **Naming:** the token path is the identifier, as in color. Path example: `space.inset.md`. Reference form: `{space.inset.md}`.
- **Primitive paths:** semantic tokens alias a Tailwind step as `{spacing.4}`. DTCG names cannot contain `.`, so half steps use a hyphen: `spacing.0-5` is Tailwind `0.5`. `spacing.px` is `1px`.
- **No tier in paths.** The tier is carried by the file name and folder.
- **Type:** `$type: dimension` is set once on the top-level group and inherited.
- **Descriptions:** every semantic token has a `$description` stating its purpose and where it is used.
- **CSS output:** each semantic token becomes a Tailwind theme variable in the `--spacing-*` namespace (section 5, rule 12). Path `space.inset.md` becomes `--spacing-inset-md` and the utility `p-inset-md`. The value is `calc(var(--spacing) * 4)`.

## 3. Primitive tier

Raw values only. Components and agents must not use primitive steps directly. **Use Tailwind's default scale exactly as shipped. Do not redefine or extend it.**

**Base unit:** `--spacing: 0.25rem` (4px at the default root size). Every step is `n × 0.25rem`. The numeric utilities (`p-4`, `gap-6`, `size-10`, `w-12`) all resolve to `calc(var(--spacing) * n)`.

**The same scale drives** padding (`p-*`), margin (`m-*`), gap (`gap-*`, `gap-x-*`, `gap-y-*`) and `inset-*`. It also drives sizing utilities (`size-*`, `w-*`, `h-*`), which are covered in `sizing-tokens.md`.

Named scale (this is the full set this system treats as available):

| Step | rem | px | | Step | rem | px |
| --- | --- | --- | --- | --- | --- | --- |
| `0` | 0 | 0 | | `16` | 4 | 64 |
| `px` | 0.0625 | 1 | | `20` | 5 | 80 |
| `0.5` | 0.125 | 2 | | `24` | 6 | 96 |
| `1` | 0.25 | 4 | | `28` | 7 | 112 |
| `1.5` | 0.375 | 6 | | `32` | 8 | 128 |
| `2` | 0.5 | 8 | | `36` | 9 | 144 |
| `2.5` | 0.625 | 10 | | `40` | 10 | 160 |
| `3` | 0.75 | 12 | | `44` | 11 | 176 |
| `3.5` | 0.875 | 14 | | `48` | 12 | 192 |
| `4` | 1 | 16 | | `52` | 13 | 208 |
| `5` | 1.25 | 20 | | `56` | 14 | 224 |
| `6` | 1.5 | 24 | | `60` | 15 | 240 |
| `7` | 1.75 | 28 | | `64` | 16 | 256 |
| `8` | 2 | 32 | | `72` | 18 | 288 |
| `9` | 2.25 | 36 | | `80` | 20 | 320 |
| `10` | 2.5 | 40 | | `96` | 24 | 384 |
| `11` | 2.75 | 44 | | | | |
| `12` | 3 | 48 | | | | |
| `14` | 3.5 | 56 | | | | |

Breakpoints and container widths are in `layout-tokens.md`. Fractions, keywords and component dimensions are in `sizing-tokens.md`.

Source: `node_modules/tailwindcss/theme.css`, Tailwind v4.3.5. If Tailwind is upgraded, re-check this section.

## 4. Semantic tier

Role-based aliases. **Components and agents reference only these tokens.** Tailwind has no semantic spacing layer, so this tier is authored for this system. **All names and values below are proposed** and can be adjusted before the files are generated. Each table shows the primitive step the token resolves to, its value, the Tailwind utilities it produces, and what it is for.

### Inset (padding inside a container)

| Token | Step | Value | Utility | Use |
| --- | --- | --- | --- | --- |
| `space.inset.2xs` | `1` | 4px | `p-inset-2xs` | Tight padding: badges, small chips. |
| `space.inset.xs` | `2` | 8px | `p-inset-xs` | Compact controls, menu items, tooltips. |
| `space.inset.sm` | `3` | 12px | `p-inset-sm` | Inputs, buttons, list rows. |
| `space.inset.md` | `4` | 16px | `p-inset-md` | Default padding for cards, panels and popovers. |
| `space.inset.lg` | `6` | 24px | `p-inset-lg` | Roomy cards, dialogs, large tiles. |
| `space.inset.xl` | `8` | 32px | `p-inset-xl` | Hero and feature panels. |

### Stack (vertical space between siblings)

| Token | Step | Value | Utility | Use |
| --- | --- | --- | --- | --- |
| `space.stack.xs` | `1` | 4px | `gap-stack-xs` | Label and helper text, title and subtitle. |
| `space.stack.sm` | `2` | 8px | `gap-stack-sm` | Related items in a group. |
| `space.stack.md` | `4` | 16px | `gap-stack-md` | Default: form fields, paragraphs, list items. |
| `space.stack.lg` | `6` | 24px | `gap-stack-lg` | Groups of related content within a section. |
| `space.stack.xl` | `8` | 32px | `gap-stack-xl` | Distinct blocks within a section. |

In a column (`flex-col`) `gap-stack-*` only affects the vertical axis. In a wrapping flex row or a grid, use `gap-y-stack-*` so the horizontal axis is not changed.

### Inline (horizontal space between siblings)

| Token | Step | Value | Utility | Use |
| --- | --- | --- | --- | --- |
| `space.inline.xs` | `1` | 4px | `gap-inline-xs` | Icon and label inside a control. |
| `space.inline.sm` | `2` | 8px | `gap-inline-sm` | Tags, chips, breadcrumb items. |
| `space.inline.md` | `3` | 12px | `gap-inline-md` | Button groups, toolbar items. |
| `space.inline.lg` | `4` | 16px | `gap-inline-lg` | Nav items, form actions. |

In a row (`flex-row`) `gap-inline-*` only affects the horizontal axis. In a wrapping row or a grid, use `gap-x-inline-*`.

### Gap (space between tiles in two-dimensional layouts)

| Token | Step | Value | Utility | Use |
| --- | --- | --- | --- | --- |
| `space.gap.sm` | `2` | 8px | `gap-gap-sm` | Dense grids, thumbnail galleries. |
| `space.gap.md` | `4` | 16px | `gap-gap-md` | Default: card grids, bento layouts. |
| `space.gap.lg` | `6` | 24px | `gap-gap-lg` | Spacious grids and feature layouts. |

Use `space.gap.*` when items are laid out on both axes (CSS grid, wrapping card layouts). Use `stack` and `inline` for one-axis spacing.

### Gutter (page edge padding)

| Token | Step | Value | Utility | Use |
| --- | --- | --- | --- | --- |
| `space.gutter.sm` | `4` | 16px | `px-gutter-sm` | Page side padding on small screens. |
| `space.gutter.md` | `6` | 24px | `px-gutter-md` | Page side padding from `md` up. |
| `space.gutter.lg` | `8` | 32px | `px-gutter-lg` | Page side padding from `lg` up. |

### Section (space between page sections)

| Token | Step | Value | Utility | Use |
| --- | --- | --- | --- | --- |
| `space.section.sm` | `12` | 48px | `py-section-sm` | Vertical padding of a page section on small screens. |
| `space.section.md` | `16` | 64px | `py-section-md` | Default vertical section padding. |
| `space.section.lg` | `24` | 96px | `py-section-lg` | Large screens and hero sections. |

## 5. Usage rules

1. **Semantic tokens only.** Never use a primitive step (`p-4`), an arbitrary value (`p-[13px]`) or a raw px or rem value in a component.
2. **Use the right group.** Padding inside a container is `inset`. Vertical space between siblings is `stack`. Horizontal space between siblings is `inline`. Space between tiles in a grid is `gap`. Page edge padding is `gutter`. Space between page sections is `section`.
3. **Gap, not margin.** Space siblings with `gap-*` on the parent. Do not use margins or `space-x-*` and `space-y-*` for sibling spacing. Margins are for `auto` alignment (`mx-auto`) only. Avoid negative margins.
4. **One owner per gap.** The parent sets the space between its children. Children do not add their own margin to adjust it.
5. **Logical properties.** Use `ps-*`, `pe-*`, `ms-*`, `me-*` (and `start-*`, `end-*`) instead of left and right so layouts follow text direction. Use `px-*` and `py-*` freely. `gap-*` is already direction-neutral.
6. **Page edges and sections.** Page container side padding uses `space.gutter.*`. Vertical padding of full-width sections uses `space.section.*`. `space.stack.*` and `space.inline.*` are not for page margins.
7. **Grids and bento layouts.** Space tiles with `space.gap.*`, pad tile contents with `space.inset.*`, and space content inside a tile with `space.stack.*`. Suggestion: keep the gap between tiles equal to or smaller than the tile padding so the tiles read as separate cards. Which tiles span columns or rows (`col-span-*`, `row-span-*`) and the column tracks (`grid-cols-*`) are layout decisions made with Tailwind utilities and are not tokens.
8. **Responsive.** Mobile-first, using the breakpoints in `layout-tokens.md`. Only `gutter` and `section` may step up at breakpoints, and only to the next token in the same group: `px-gutter-sm md:px-gutter-md lg:px-gutter-lg`. `inset`, `stack`, `inline` and `gap` do not change at breakpoints.
9. **Radius is separate.** Corner radius is covered by the radius tokens, not by spacing.
10. **Tailwind.** Do not redefine `--spacing` or the default scale. Add each semantic token to the theme as `--spacing-<group>-<size>` (for example `--spacing-inset-md: calc(var(--spacing) * 4)`). Two consequences:
    - The `--spacing-*` namespace feeds every sizing and spacing utility, so `w-inset-md` and `m-gap-md` also compile. Use each token only for its stated purpose.
    - Numeric utilities (`p-4`) still compile, so rule 1 is enforced by convention and review, not by the build.
11. **Do not invent tokens.** If no token fits, say so and propose a new semantic token rather than using a nearby one or a primitive.

## 6. Modes and density

- Spacing has one semantic file. Light and dark use identical values, so there is no mode switch and no mode in token paths.
- There is no density mode (compact, comfortable) and no responsive token values. Responsive behavior comes only from rule 8.
- If a density mode is added later, it should follow color's model: identical paths, the mode carried by the file name.

## 7. Verification

Checked 2026-11-24 against the installed Tailwind v4.3.5.

- **Primitive values:** the base unit in section 3 matches `node_modules/tailwindcss/theme.css`. The scale table is arithmetic from `--spacing: 0.25rem`.
- **Default utilities:** `npm run build` compiles `gap-4` to `gap: calc(var(--spacing) * 4)` and `p-6` to `padding: calc(var(--spacing) * 6)`.
- **Semantic variables:** compiling theme variables named `--spacing-inset-md`, `--spacing-gap-md` and `--spacing-gutter-md` produced `p-inset-md`, `ps-inset-md`, `gap-gap-md`, `gap-y-gap-md` and `px-gutter-md` as expected.
- **Not verified:** the semantic token JSON, the generated CSS and any component use. None of these exist yet.

## 8. Known gaps

- **No generated files.** `tokens/primitives/spacing.json` is empty, `tokens/semantic/spacing.json` does not exist, and no `tokens/dist` output or `src/styles` theme mapping is generated. The build tool for tokens is not chosen.
- **Semantic values are proposed.** Names, steps and counts (24 tokens) have not been reviewed against real components. Expect adjustments once components use them.
- **`gap-gap-*` reads awkwardly.** The `space.gap` group produces `gap-gap-md`. Renaming the group (for example `space.grid`) would give `gap-grid-md`.
- **Semantic-only rule is strict.** Rule 1 may leave margin, offset or one-off layout cases without a token. Add tokens rather than relaxing the rule, as in color.
- **No lint enforcement.** Nothing yet blocks numeric utilities (`p-4`) or arbitrary values in components.
- **No Figma variables.** A Spacing collection (primitive and semantic) needs to be created from sections 3 and 4.
- **No density or responsive token values**, and no pairing list in machine-readable form.
