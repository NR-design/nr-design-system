# Sizing tokens: context and usage rules

Goal: This document describes the sizing tokens (component dimensions and interactive target sizes) and the rules for using them. It is written for both people and AI agents. Space between and inside elements is in `spacing-tokens.md`. Breakpoints and page widths are in `layout-tokens.md`.

Status (2026-11-24): **target state.** Tailwind CSS v4.3.5 is installed and its default spacing scale is the primitive tier. The semantic tokens in section 4 are **proposed** and have not been generated yet: no sizing token files exist. Where code or Figma disagree with this document, this document is the target.

## 1. Files

| File | Tier | Contents |
| --- | --- | --- |
| Tailwind default theme (`tailwindcss/theme.css`) | Primitive | The `--spacing` base unit and the scale it generates. Shared with spacing. **Referenced, not recreated.** |
| `tokens/primitives/sizing.json` | Primitive | Does not exist yet. If generated, it is derived from the Tailwind theme at build time and never hand-authored |
| `tokens/semantic/sizing.json` | Semantic | 8 role-based tokens (section 4). All are aliases to primitive scale steps |

There is one semantic file. Sizing has no light and dark modes (section 6).

## 2. Format and structure

- **Format:** W3C Design Tokens Community Group (DTCG) 2025.10. Dimensions are objects: `{ "value": 40, "unit": "px" }`.
- **Naming:** the token path is the identifier, as in color and spacing. Path example: `size.control.md`. Reference form: `{size.control.md}`.
- **Primitive paths:** semantic tokens alias a scale step as `{spacing.10}`. DTCG names cannot contain `.`, so half steps use a hyphen: `spacing.0-5` is Tailwind `0.5`.
- **Type:** `$type: dimension` is set once on the top-level group and inherited.
- **Descriptions:** every semantic token has a `$description` stating its purpose and where it is used.
- **CSS output:** each semantic token becomes a Tailwind theme variable in the `--spacing-*` namespace (section 5, rule 8). Path `size.control.md` becomes `--spacing-control-md` and the utility `h-control-md`. The value is `calc(var(--spacing) * 10)`.

## 3. Primitive tier

Raw values only. **Sizing uses the same scale as spacing. Do not redefine or extend it.** The full scale (`0`, `px`, `0.5` to `96`, base unit `--spacing: 0.25rem`) is in `spacing-tokens.md`, section 3.

**The scale drives** `size-*`, `w-*`, `h-*`, `min-w-*`, `min-h-*`, `max-w-*` (numeric steps) and `max-h-*`.

**Fractions and keywords** (`w-1/2`, `w-full`, `w-screen`, `h-dvh`, `w-auto`, `w-fit`, `w-min`, `w-max`) are layout utilities, not tokens. Use them as Tailwind defines them. Named container widths (`max-w-7xl`) are in `layout-tokens.md`.

## 4. Semantic tier

Role-based aliases. **Components and agents reference only these tokens for control, icon and target dimensions.** Tailwind has no semantic sizing layer, so this tier is authored for this system. **All names and values below are proposed.**

### Target (minimum interactive size)

| Token | Step | Value | Utility | Use |
| --- | --- | --- | --- | --- |
| `size.target.min` | `6` | 24px | `min-h-target-min` | Minimum size of any interactive target (WCAG 2.2, 2.5.8 Target Size, Minimum). |
| `size.target.touch` | `11` | 44px | `min-h-target-touch` | Recommended touch target (WCAG 2.5.5, level AAA, and common platform guidance). |

### Control (height of buttons, inputs and selects)

| Token | Step | Value | Utility | Use |
| --- | --- | --- | --- | --- |
| `size.control.sm` | `8` | 32px | `h-control-sm` | Small button, input, select. |
| `size.control.md` | `10` | 40px | `h-control-md` | Default button, input, select. |
| `size.control.lg` | `12` | 48px | `h-control-lg` | Large button, input, select. Meets `size.target.touch`. |

### Icon

| Token | Step | Value | Utility | Use |
| --- | --- | --- | --- | --- |
| `size.icon.sm` | `4` | 16px | `size-icon-sm` | Icons inside compact controls. |
| `size.icon.md` | `5` | 20px | `size-icon-md` | Default icon. |
| `size.icon.lg` | `6` | 24px | `size-icon-lg` | Standalone and large icons. |

`size.control.sm` and `size.control.md` are below `size.target.touch`. On touch interfaces, use `size.control.lg` or extend the hit area to at least 44px with padding or a pseudo-element.

## 5. Usage rules

1. **Semantic tokens only.** Never use a primitive step (`h-10`), an arbitrary value (`h-[38px]`) or a raw px or rem value for control, icon or target dimensions.
2. **Controls share heights.** Buttons, inputs and selects placed side by side use the same `size.control.*` step.
3. **Control padding is spacing.** Horizontal padding inside a control uses `space.inset.*`, and the gap between its icon and label uses `space.inline.*` (see `spacing-tokens.md`). Do not size a control by padding alone.
4. **Icons are square.** Use `size-icon-*` (width and height together). Pick the icon size to match the control: `sm` in `size.control.sm`, `md` in `size.control.md`, `md` or `lg` in `size.control.lg`.
5. **Target size.** Every interactive element is at least `size.target.min` (24 × 24px). Touch-first interfaces use `size.target.touch` (44px). Space between adjacent targets does not replace target size.
6. **Sizing tokens are not spacing.** Do not use `size.*` tokens for padding, margin or gap.
7. **Other dimensions.** Where no token fits (images, avatars, charts, illustrations), size with Tailwind fractions and keywords (rule in section 3) or propose a new token. Do not use arbitrary pixel values.
8. **Tailwind.** Do not redefine `--spacing` or the default scale. Add each semantic token to the theme as `--spacing-<group>-<size>` (for example `--spacing-control-md: calc(var(--spacing) * 10)`). Two consequences:
   - The `--spacing-*` namespace feeds every sizing and spacing utility, so `p-control-md` and `gap-icon-sm` also compile. Use each token only for its stated purpose.
   - Numeric utilities (`h-10`) still compile, so rule 1 is enforced by convention and review, not by the build.
9. **Do not invent tokens.** If no token fits, say so and propose a new semantic token rather than using a nearby one or a primitive.

## 6. Modes and density

- Sizing has one semantic file. Light and dark use identical values, so there is no mode switch and no mode in token paths.
- There is no density mode (compact, comfortable). `size.control.*` already offers three heights. If a density mode is added later, it should follow color's model: identical paths, the mode carried by the file name.

## 7. Verification

Checked 2026-11-24 against the installed Tailwind v4.3.5.

- **Primitive values:** the scale is arithmetic from `--spacing: 0.25rem`. See `spacing-tokens.md`, section 7.
- **Semantic variables:** compiling theme variables `--spacing-control-md`, `--spacing-icon-md` and `--spacing-target-min` produced `h-control-md`, `size-icon-md` and `min-h-target-min` as expected.
- **Not verified:** the semantic token JSON, the generated CSS, target sizes in real components and any component use. None of these exist yet.

## 8. Known gaps

- **No generated files.** `tokens/primitives/sizing.json` and `tokens/semantic/sizing.json` do not exist, and no `tokens/dist` output or `src/styles` theme mapping is generated. The build tool for tokens is not chosen.
- **Semantic values are proposed.** Names, steps and the count (8 tokens) have not been reviewed against real components.
- **Partial coverage.** Only controls, icons and target sizes are tokenized. Avatars, badges, thumbnails, checkboxes, switches and similar components have no token yet.
- **No line-height or text pairing.** The relationship between control heights and the typography tokens is not defined.
- **No lint enforcement.** Nothing yet blocks numeric utilities (`h-10`) or arbitrary values in components.
- **No Figma variables.** A Sizing collection needs to be created from section 4.
