# color-tokens

# Color tokens: context and usage rules

Goal: This document describes the color token files and the rules for using them. It is written for both people and AI agents.

Status (2026-11-21): this version follows `claude/color-ramp-decisions.md` (Tailwind 50–950 scale, OKLCH ramps). `color.primitive.tokens.json`, the semantic files and the Figma styles **have not been regenerated yet** and still hold the old 10–190 values. Where they disagree with this document, this document is the target.

Figma Documentation: *Nick’s Design Library 2.0*, frame **Colors** (node `524:673`).

## 1. Files

| File | Tier | Contents |
| --- | --- | --- |
| `color.primitive.tokens.json` | Primitive | 73 tokens: Green, Yellow, Gray, Blue, Red and Purple ramps (11 steps each), one `base` alias per ramp, and white |
| `color.semantic.light.tokens.json` | Semantic | 70 role-based tokens for light mode. All are aliases to primitives except `surface.scrim` and `surface.interactive.hover` |
| `color.semantic.dark.tokens.json` | Semantic | 70 role-based tokens for dark mode. Same exceptions as light |

Load `color.primitive.tokens.json` together with one semantic file. A semantic file on its own has unresolved aliases.

## 2. Format and structure

- **Format:** W3C Design Tokens Community Group (DTCG) 2025.10. Colors are objects: `{ "colorSpace": "srgb", "components": [r, g, b], "hex": "#rrggbb" }`, with optional `alpha`.
- **Naming:** the token path is the identifier. There is no separate name field. Path example: `color.action.primary.default`. Reference form: `{color.action.primary.default}`.
- **No tier or mode in paths.** Tier and mode are carried by the file name (`{category}.{tier}[.{mode}].tokens.json`). Light and dark files expose identical paths, so code references one name and the theme decides the value.
- **Type:** `$type: color` is set once on the top-level `color` group and inherited.
- **Descriptions:** every semantic token has a `$description` stating its purpose and its required pairing.
- **`$extensions`:** primitives carry Figma metadata under the `com.figma` key, in the shape Figma’s own export uses (`variableId`, `scopes`). `variableId` and `scopes` are added once the Figma variables exist. The Figma variable name is not stored: it is the token path (for example `Green/600` is `color.green.600`). Tools that don’t recognize the key should ignore it.

## 3. Primitive tier

Raw values only. Components and agents must not reference primitives directly.

**Scale:** Tailwind steps, 50 to 950 (11 stops): 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950. **The same step means the same lightness in every ramp**. Ramps are generated in OKLCH with a fixed target lightness per step, hue and chroma from each brand base, gamut-clipped to sRGB.

Target lightness: each step has a fixed lightness (OKLCH L, 0–1) shared by every ramp: 50 .975, 100 .95, 200 .90, 300 .83, 400 .74, 500 .65, 600 .56, 700 .48, 800 .39, 900 .30, 950 .22. The base step is the one exception and keeps its own.

**Base:** each brand base keeps its exact hex and sits at the step nearest its lightness, so the base step differs per ramp. Each ramp has a `base` alias (for example `color.gray.base` → `{color.gray.900}`; Figma `Gray/Base`).

| Family | Role | Base | Lightest (50) | Darkest (950) |
| --- | --- | --- | --- | --- |
| `green` | Brand primary | 600 `#3f7f75` | `#f1f9f7` | `#00201c` |
| `yellow` | Brand primary accent | 300 `#ffbc42` | `#fff5e7` | `#261700` |
| `purple` | Brand secondary accent | 700 `#5145a6` | `#f5f6ff` | `#190b48` |
| `gray` | Neutral ramp | 900 `#303030` | `#f7f7f7` | `#1b1b1b` |
| `blue` | Info hue | 700 `#275d8c` | `#f2f8fd` | `#001c35` |
| `red` | Error hue | 700 `#a3342b` | `#fff4f2` | `#3b0000` |
| `white` | Pure white | n/a | n/a | n/a |

Full ramps (bold = base):

| Step | Green | Yellow | Purple | Gray | Blue | Red |
| --- | --- | --- | --- | --- | --- | --- |
| 50 | #f1f9f7 | #fff5e7 | #f5f6ff | #f7f7f7 | #f2f8fd | #fff4f2 |
| 100 | #e6f2ef | #ffecce | #ecedff | #eeeeee | #e6f0f9 | #ffe9e5 |
| 200 | #cfe3df | #ffd799 | #dadbfa | #dedede | #d0e0f0 | #fad4ce |
| 300 | #b1cfc9 | **#ffbc42** | #c2c2f0 | #c7c7c7 | #b3cae1 | #efb8b0 |
| 400 | #8cb5ad | #db9e27 | #a4a4e0 | #ababab | #90afcd | #dd958b |
| 500 | #689b92 | #bb8306 | #8885cf | #8f8f8f | #6d93b8 | #ca7368 |
| 600 | **#3f7f75** | #996a00 | #6d68bc | #747474 | #4b79a3 | #b55146 |
| 700 | #2e6960 | #7c5500 | **#5145a6** | #5d5d5d | **#275d8c** | **#a3342b** |
| 800 | #1a4f47 | #5c3f00 | #40348b | #454545 | #164871 | #7d1c16 |
| 900 | #053630 | #3f2900 | #2a1e67 | **#303030** | #033052 | #5b0605 |
| 950 | #00201c | #261700 | #190b48 | #1b1b1b | #001c35 | #3b0000 |

**Pairing rule:** step 600 and darker passes AA (4.5:1) with white text in every ramp (4.6–5.0:1). Step 500 and lighter passes AA with black text (6.2–6.7:1). 600 is the transition step: black text on 600 is borderline (Gray 4.49, Yellow 4.42, Purple 4.36, Red 4.23) and should not be used.

## 4. Semantic tier

Role-based aliases. **Components and agents reference only these tokens.** Each table row shows the primitive the token resolves to in each mode, what it is for, and its measured contrast in light / dark mode against the values in section 3. Values below the target are in bold.

### Surface

| Token | Light | Dark | Use | Contrast (light / dark) |
| --- | --- | --- | --- | --- |
| `surface.default` | `gray.50` (#f7f7f7) | `gray.950` (#1b1b1b) | Page and app background. | n/a |
| `surface.raised` | `white` (#ffffff) | `gray.800` (#454545) | Cards, popovers and panels that sit above the page. Pair with a `border.default` edge in light mode. | text.primary 17.2 / 8.3; text.secondary 6.6 / 5.7; text.placeholder 6.6 / **4.2**; border.strong 4.7 / 4.2 |
| `surface.subtle` | `gray.100` (#eeeeee) | `gray.900` (#303030) | Quiet panels, cards and table stripes on the default surface. | n/a |
| `surface.muted` | `gray.200` (#dedede) | `gray.800` (#454545) | Pressed or selected neutral areas, chips, dividers with fill. | n/a |
| `surface.input` | `white` (#ffffff) | `gray.950` (#1b1b1b) | Fill of text inputs, textareas and selects. Use on any surface, including `surface.muted` and `surface.raised`. Always draw a `border.strong` outline; the fill alone does not separate the field from its container. | text.primary 17.2 / 14.8; text.secondary 6.6 / 10.2; text.placeholder 6.6 / 7.5; border.strong 4.7 / 7.5 |
| `surface.inverse` | `gray.950` (#1b1b1b) | `gray.100` (#eeeeee) | High-contrast surface (toasts, tooltips). Use text.inverse on it. | n/a |
| `surface.brand-subtle` | `green.100` (#e6f2ef) | `green.950` (#00201c) | Alternate brand-tinted page and app background. | n/a |
| `surface.scrim` | `#1b1b1b` @ 50% alpha | `#1b1b1b` @ 70% alpha | Overlay behind modals and drawers. Source: color.gray.950 | n/a |
| `surface.interactive.hover` | `#1b1b1b` @ 8% alpha | `#ffffff` @ 6% alpha | Hover fill for list rows, menu items and cards. Overlay composited on the surface below. Source (light/dark): color.gray.950/White | see section 7 |
| `surface.interactive.selected` | `green.200` (#cfe3df) | `green.900` (#053630) | Selected row or item. | text.primary 12.9 / 11.5 |

### Text

| Token | Light | Dark | Use | Contrast (light / dark) |
| --- | --- | --- | --- | --- |
| `text.primary` | `gray.950` (#1b1b1b) | `gray.100` (#eeeeee) | Default body and heading text. | 16.1 / 14.8 on surface.default; 14.8 / 11.4 on surface.subtle; 12.8 / 8.3 on surface.muted |
| `text.secondary` | `gray.700` (#5d5d5d) | `gray.300` (#c7c7c7) | Supporting text, captions, helper text. | 6.1 / 10.2 on surface.default; 5.7 / 7.8 on surface.subtle; 4.9 / 5.7 on surface.muted |
| `text.placeholder` | `gray.700` (#5d5d5d) | `gray.400` (#ababab) | Placeholder text in inputs. Pair with `surface.input`. | 6.6 / 7.5 on surface.input; 6.1 / 7.5 on surface.default; 5.7 / 5.7 on surface.subtle; 4.9 / 4.2 on surface.muted (not for inputs; use `surface.input`) |
| `text.disabled` | `gray.400` (#ababab) | `gray.700` (#5d5d5d) | Disabled text. Exempt from contrast requirements; do not use for readable content. | exempt |
| `text.inverse` | `gray.100` (#eeeeee) | `gray.950` (#1b1b1b) | Text on surface.inverse. | 14.8 / 14.8 on surface.inverse |
| `text.brand` | `green.700` (#2e6960) | `green.400` (#8cb5ad) | Brand-colored text such as eyebrows and emphasized labels. | 5.9 / 7.7 on surface.default; 5.5 / 7.6 on surface.brand-subtle |
| `text.link` | `green.900` (#053630) | `green.300` (#b1cfc9) | Inline links. Always pair with an underline or another non-color cue. | 12.4 / 10.4 on surface.default; 11.5 / 7.9 on surface.subtle |
| `text.selection.background` | `yellow.200` (#ffd799) | `yellow.700` (#7c5500) | Highlighted text. | n/a |
| `text.selection.foreground` | `gray.950` (#1b1b1b) | `gray.100` (#eeeeee) | Text inside a selection. | 12.7 / 5.7 on text.selection.background |

### Icon

| Token | Light | Dark | Use | Contrast (light / dark) |
| --- | --- | --- | --- | --- |
| `icon.default` | `gray.950` (#1b1b1b) | `gray.100` (#eeeeee) | Default icon color. | n/a |
| `icon.subtle` | `gray.600` (#747474) | `gray.400` (#ababab) | De-emphasized icons. | 4.4 / 7.5 on surface.default; 4.0 / 5.7 on surface.subtle; 3.5 / 4.2 on surface.muted |
| `icon.brand` | `green.600` (#3f7f75) | `green.400` (#8cb5ad) | Brand-colored icons. | 4.4 / 7.7 on surface.default |
| `icon.inverse` | `gray.100` (#eeeeee) | `gray.950` (#1b1b1b) | Icons on surface.inverse. | 14.8 / 14.8 on surface.inverse |

### Border

| Token | Light | Dark | Use | Contrast (light / dark) |
| --- | --- | --- | --- | --- |
| `border.default` | `green.200` (#cfe3df) | `green.900` (#053630) | Dividers and decorative borders. Not sufficient alone to identify an input. | n/a |
| `border.strong` | `gray.600` (#747474) | `gray.400` (#ababab) | Input, checkbox and control outlines. Needs 3:1 non-text contrast. | 4.4 / 7.5 on surface.default; 4.0 / 5.7 on surface.subtle; 3.5 / 4.2 on surface.muted |
| `border.disabled` | `gray.200` (#dedede) | `gray.800` (#454545) | Border on disabled controls. | exempt |
| `border.brand` | `green.600` (#3f7f75) | `green.400` (#8cb5ad) | Selected or brand-emphasized borders. | 4.4 / 7.7 on surface.default |
| `border.focus` | `green.800` (#1a4f47) | `yellow.300` (#ffbc42) | Keyboard focus ring. Draw with a 2px gap between ring and component so it is measured against the surface, not the component fill. | 8.7 / 10.3 on surface.default; 8.0 / 7.9 on surface.subtle; 6.9 / 5.7 on surface.muted |

### Action (buttons and controls)

| Token | Light | Dark | Use | Contrast (light / dark) |
| --- | --- | --- | --- | --- |
| `action.primary.default` | `green.600` (#3f7f75) | `green.400` (#8cb5ad) | Primary button fill. Pair with action.primary.on. | 4.4 / 7.7 on surface.default |
| `action.primary.hover` | `green.700` (#2e6960) | `green.300` (#b1cfc9) | Primary button hover. | n/a |
| `action.primary.pressed` | `green.800` (#1a4f47) | `green.200` (#cfe3df) | Primary button pressed. | n/a |
| `action.primary.on` | `white` (#ffffff) | `green.900` (#053630) | Text and icons on any action.primary state. | 4.7 / 5.9 on action.primary.default; 6.4 / 8.0 on action.primary.hover; 9.3 / 10.0 on action.primary.pressed |
| `action.secondary.default` | `green.300` (#b1cfc9) | `green.900` (#053630) | Accent button fill. Pair with action.secondary.on. | n/a |
| `action.secondary.hover` | `green.400` (#8cb5ad) | `green.800` (#1a4f47) | Accent button hover. | n/a |
| `action.secondary.pressed` | `green.500` (#689b92) | `green.700` (#2e6960) | Accent button pressed. | n/a |
| `action.secondary.on` | `green.950` (#00201c) | `green.100` (#e6f2ef) | Text and icons on any action.secondary state. | 10.3 / 11.6 on action.secondary.default; 7.6 / 8.1 on action.secondary.hover; 5.5 / 5.5 on action.secondary.pressed |
| `action.disabled.background` | `gray.100` (#eeeeee) | `gray.900` (#303030) | Disabled control fill. | exempt |
| `action.disabled.foreground` | `gray.400` (#ababab) | `gray.700` (#5d5d5d) | Disabled control text and icons. | exempt |

### Feedback (status messages)

| Token | Light | Dark | Use | Contrast (light / dark) |
| --- | --- | --- | --- | --- |
| `feedback.info.background` | `blue.100` (#e6f0f9) | `blue.950` (#001c35) | Info banner fill. | n/a |
| `feedback.info.foreground` | `blue.950` (#001c35) | `blue.400` (#90afcd) | Info text and icons on feedback.info.background. | 14.9 / 7.6 on feedback.info.background |
| `feedback.info.border` | `blue.700` (#275d8c) | `blue.400` (#90afcd) | Info banner border. | 6.5 / 7.6 on surface.default; 5.8 on surface.subtle; 4.2 on surface.muted and surface.raised (dark) |
| `feedback.success.background` | `green.100` (#e6f2ef) | `green.950` (#00201c) | Success banner fill. | n/a |
| `feedback.success.foreground` | `green.800` (#1a4f47) | `green.300` (#b1cfc9) | Success text and icons on feedback.success.background. | 8.1 / 10.3 on feedback.success.background |
| `feedback.success.border` | `green.600` (#3f7f75) | `green.500` (#689b92) | Success banner border. | 4.4 / 5.5 on surface.default |
| `feedback.warning.background` | `yellow.50` (#fff5e7) | `yellow.950` (#261700) | Warning banner fill. | n/a |
| `feedback.warning.foreground` | `yellow.800` (#5c3f00) | `yellow.200` (#ffd799) | Warning text and icons on feedback.warning.background. | 9.0 / 12.8 on feedback.warning.background |
| `feedback.warning.border` | `yellow.600` (#996a00) | `yellow.400` (#db9e27) | Warning banner border. | 4.4 / 7.3 on surface.default; 4.1 / 5.6 on surface.subtle; 3.5 / 4.1 on surface.muted |
| `feedback.error.background` | `red.100` (#ffe9e5) | `red.950` (#3b0000) | Error banner fill. | n/a |
| `feedback.error.foreground` | `red.950` (#3b0000) | `red.400` (#dd958b) | Error text and icons on feedback.error.background. | 15.2 / 7.4 on feedback.error.background |
| `feedback.error.border` | `red.700` (#a3342b) | `red.400` (#dd958b) | Error banner border. | 6.4 / 7.2 on surface.default; 5.5 on surface.subtle; 4.0 on surface.muted and surface.raised (dark) |
| `feedback.success.solid.background` | `green.700` (#2e6960) | `green.400` (#8cb5ad) | Success badges and filled alerts. | n/a |
| `feedback.success.solid.foreground` | `white` (#ffffff) | `green.950` (#00201c) | Success badges and filled alerts. | 6.4 / 7.6 on feedback.success.solid.background |
| `feedback.warning.solid.background` | `yellow.300` (#ffbc42) | `yellow.300` (#ffbc42) | Warning badges and filled alerts. | n/a |
| `feedback.warning.solid.foreground` | `gray.950` (#1b1b1b) | `gray.950` (#1b1b1b) | Warning badges and filled alerts. | 10.3 / 10.3 on feedback.warning.solid.background |
| `feedback.error.solid.background` | `red.700` (#a3342b) | `red.500` (#ca7368) | Error badges and filled alerts. | n/a |
| `feedback.error.solid.foreground` | `white` (#ffffff) | `gray.950` (#1b1b1b) | Error badges and filled alerts. | 6.8 / 5.1 on feedback.error.solid.background |
| `feedback.info.solid.background` | `blue.700` (#275d8c) | `blue.500` (#6d93b8) | Info badges and filled alerts. | n/a |
| `feedback.info.solid.foreground` | `white` (#ffffff) | `gray.950` (#1b1b1b) | Info badges and filled alerts. | 6.9 / 5.3 on feedback.info.solid.background |

### Data visualization

Chart colors. Every token is ≥3:1 on both `surface.default` and `surface.subtle` (WCAG 1.4.11). Contrast columns show `surface.default` / `surface.subtle`.

#### Single series

| Token | Light | Dark | Use | Contrast (light) | Contrast (dark) |
| --- | --- | --- | --- | --- | --- |
| `dataviz.single` | `green.600` (#3f7f75) | `green.500` (#689b92) | Charts with exactly one series or category. Brand color. Never combine with `dataviz.categorical.*`. | 4.4 / 4.0 | 5.5 / 4.2 |

#### Categorical (2 to 5 series)

| Token | Light | Dark | Use | Contrast (light) | Contrast (dark) |
| --- | --- | --- | --- | --- | --- |
| `dataviz.categorical.1` | `blue.700` (#275d8c) | `blue.300` (#b3cae1) | Series A. First series, primary hue. | 6.5 / 6.0 | 10.2 / 7.8 |
| `dataviz.categorical.2` | `yellow.600` (#996a00) | `yellow.300` (#ffbc42) | Series B. Pairs with series A as the safest hue pair for color-vision deficiency. | 4.4 / 4.1 | 10.3 / 7.9 |
| `dataviz.categorical.3` | `blue.900` (#033052) | `blue.500` (#6d93b8) | Series C. Darker blue; always pair with a distinct marker, dash or pattern from series A. | 12.7 / 11.7 | 5.3 / 4.1 |
| `dataviz.categorical.4` | `gray.600` (#747474) | `gray.400` (#ababab) | Series D. Neutral; use for "Other" or "Total". | 4.4 / 4.0 | 7.5 / 5.7 |
| `dataviz.categorical.5` | `red.700` (#a3342b) | `red.500` (#ca7368) | Series E. | 6.4 / 5.9 | 5.1 / 3.9 |

#### Sequential (single hue, low to high magnitude)

| Token | Light | Dark | Use | Contrast (light) | Contrast (dark) |
| --- | --- | --- | --- | --- | --- |
| `dataviz.sequential.1` | `green.600` (#3f7f75) | `green.500` (#689b92) | Lowest magnitude. | 4.4 / 4.0 | 5.5 / 4.2 |
| `dataviz.sequential.2` | `green.700` (#2e6960) | `green.400` (#8cb5ad) | Low-mid magnitude. | 5.9 / 5.5 | 7.7 / 5.9 |
| `dataviz.sequential.3` | `green.800` (#1a4f47) | `green.300` (#b1cfc9) | Mid magnitude. | 8.7 / 8.0 | 10.4 / 7.9 |
| `dataviz.sequential.4` | `green.900` (#053630) | `green.200` (#cfe3df) | High-mid magnitude. | 12.4 / 11.5 | 12.9 / 9.9 |
| `dataviz.sequential.5` | `green.950` (#00201c) | `green.100` (#e6f2ef) | Highest magnitude. | 16.0 / 14.8 | 15.0 / 11.5 |

**Values (neutral gray)**

| Token | Light | Dark | Use | Contrast (light) | Contrast (dark) |
| --- | --- | --- | --- | --- | --- |
| `dataviz.gridline` | `gray.300` (#c7c7c7) | `gray.800` (#454545) | Chart gridlines and tick rules. Decorative; values must also be labeled. | 1.6 / 1.5 | 1.8 / 1.4 |

## 5. Usage rules

1. **Semantic tokens only.** Never use a primitive, a raw hex value or an arbitrary color in a component.
2. **Always pair.** Put text and icons on a background using the matching foreground token: `action.primary.on` on `action.primary.*`, `action.secondary.on` on `action.secondary.*`, `text.inverse` and `icon.inverse` on `surface.inverse`, `feedback.*.foreground` on `feedback.*.background`, `feedback.*.solid.foreground` on `feedback.*.solid.background`.
3. **Contrast targets.** Text needs 4.5:1. Icons and control borders need 3:1. Disabled content is exempt, but must not carry readable information.
4. **Never rely on color alone.** Pair status colors with an icon and a text label. Links need an underline or other non-color cue.
5. **Borders.** `border.default` is for dividers and decoration only. Use `border.strong` for input, checkbox and other control outlines. Text inputs use `surface.input` as their fill, with a `border.strong` outline, on any surface.
6. **Focus.** Use `border.focus` for the keyboard focus ring, drawn with a 2px gap between ring and component so it is measured against the surface.
7. **Primary vs secondary actions.** Action colors use only tints and shades of Green. Primary (`action.primary.*`) is the main action on a screen. Secondary (`action.secondary.*`) is for supporting actions and is visually lighter than primary. Do not use Yellow, Purple or any feedback color (success, warning, error, info) for buttons.
8. **Modes.** Use the same token name in light and dark. Never hard-code a mode-specific value or branch on mode in a component.
9. **Scrim.** `surface.scrim` is `gray.950` (`#1b1b1b`) with alpha (50% light, 70% dark). It is a literal value rather than an alias due to DTCG alias constraints. Its description names the source. If `gray.950` changes, update the scrim to match.
10. **Hover.** `surface.interactive.hover` is an overlay, not a gray step: `gray.950` (`#1b1b1b`) at 8% in light mode, white at 6% in dark mode, composited over whatever surface is below. Like the scrim, it is a literal with alpha and a source note in its description.
11. **Do not invent tokens.** If no token fits, say so and propose a new semantic token rather than using a nearby one.
12. **Tailwind.** Ramp names are plain (`green`, `gray`, …) and replace Tailwind's same-named defaults. Remove the rest of the default palette (v4: `--color-*: initial;` before the custom definitions; keep `white`, `black`, `transparent`, `current`) so off-brand ramps such as `emerald` are unavailable. Check pre-styled UI libraries before adopting them.

### Data visualization

13. **Single series.** Use `dataviz.single` only when a chart has exactly one series or category. Never combine it with `dataviz.categorical.*`, and never use it as the first series of a multi-series chart.
14. **Multiple series.** Use `dataviz.categorical.1` to `5` in order; do not skip or reorder. With 2 or 3 series, use the first 2 or 3. Flag situations when data needs more than 5 series, do not add new colors.
15. **Separate touching marks.** Stacked bars, segments and areas get a 2px stroke between them, in the color of the surface behind the chart (`surface.default` or `surface.subtle`). Neighboring series are not 3:1 apart, so the separator provides the required contrast. Every categorical token is ≥3:1 on both surfaces, so the separator works on either.
16. **Never rely on color alone in charts.** Use direct labels, plus distinct markers and dash patterns for lines and pattern fills for areas. Categorical 1 and 3 are both blue and must always differ by marker, dash or pattern.
17. **Chart text.** Use `text.primary` and `text.secondary` for labels and axes, not series colors. In light mode, `dataviz.single`, `dataviz.categorical.2`, `dataviz.categorical.4` and `dataviz.sequential.1` fail 4.5:1 as text.
18. **Sequential steps.** Neighboring steps are not 3:1 apart. Show values with labels or tooltips and do not use step color as the only encoding.
19. **Gridlines.** Use `dataviz.gridline`, 1px, horizontal only. Gridlines are decorative, so always label values with axis ticks or direct labels. Do NOT use `border.default` or `border.strong` for gridlines.
20. **Chart surfaces.** Place charts only on `surface.default` or `surface.subtle`. Data visualization tokens are not verified for `surface.muted` or `surface.raised`, and several fail 3:1 there in dark mode.

## 6. Light and dark modes

- Light surfaces step from `gray.50` through `gray.100` and `gray.200`. The `white` primitive is used only for `surface.raised`, `surface.input` and for on-colors (`action.primary.on`, solid feedback foregrounds). The dark hover overlay (`surface.interactive.hover`) uses a literal `#ffffff` at 6% alpha instead, like the scrim.
- `surface.raised` is lighter than the page in both modes: `white` on `gray.50` in light, `gray.800` on `gray.950` in dark. In dark mode it shares its value with `surface.muted`. Dark surfaces step from `gray.950` through `gray.900` and `gray.800`.
- Dark action colors are lighter than in light mode (`green.400` instead of `green.600`), get lighter on hover and press, and their on-color is a dark green (`green.900`).
- `surface.inverse` is dark in light mode (`gray.950`) and light in dark mode (`gray.100`).
- The two semantic files must always have identical path sets. Check this after every change.

## 7. Verification

Checked 2026-11-21 with the WCAG 2.x relative-luminance formula against the values in section 3.

- **All required text pairs pass 4.5:1.** Dark `text.placeholder` on `surface.muted` and `surface.raised` (both Gray 800) is 4.2:1, so it is not used there. Inputs use `surface.input` (Gray 950 in dark), where placeholder is 7.5:1, so inputs can sit on any surface. Input `border.strong` is 4.7 / 7.5 on `surface.input`, and 4.2 on dark `surface.muted` and `surface.raised`, which passes 3:1.
- **Dark feedback borders on neutral surfaces:** all four pass 3:1 on every dark surface. Info and error use step 400 (info 5.8 on `surface.subtle`, 4.2 on `surface.muted` and `surface.raised`; error 5.5 and 4.0). Previously they used step 600 and failed off the page.
- **All icon, control border and focus pairs pass 3:1** on `surface.default`, `surface.subtle` and `surface.muted` in both modes.
- **All data visualization tokens pass 3:1** on `surface.default` and `surface.subtle` in both modes (gridline excepted; it is decorative). Narrowest: dark `dataviz.categorical.5` on `surface.subtle` (3.9:1). On dark `surface.muted` and `surface.raised` (Gray 800), `dataviz.categorical.3` (2.97:1) and `dataviz.categorical.5` (2.8:1) fail, and `dataviz.single` is 3.06:1; see rule 20.
- **Pairing rule (section 3)** holds for all six ramps.
- **Previous failures fixed:** light `text.secondary` on `surface.subtle` (was 4.2, now 5.7); light `border.strong` on `surface.default` (was 2.9, now 4.4).
- **Narrowest margin:** light `feedback.warning.border`, `icon.subtle` and `border.strong` on `surface.muted` are 3.5:1 against a 3:1 target. Any darkening of the light muted surface will make them fail.
- **Hover overlay composites** (verified):

| Mode | Surface | Composite | text.primary | text.secondary | icon.subtle |
| --- | --- | --- | --- | --- | --- |
| Light | `surface.default` (#f7f7f7) | #e5e5e5 | 13.7 | 5.2 | 3.7 |
| Dark | `surface.default` (#1b1b1b) | #292929 | 12.5 | 8.6 | 6.3 |
| Dark | `surface.subtle` (#303030) | #3c3c3c | 9.5 | 6.5 | 4.8 |
| Dark | `surface.muted` / `surface.raised` (#454545) | #505050 | 7.0 | 4.77 | 3.5 |

- **Disabled (exempt, for reference):** `border.disabled` 1.3 light on default / 1.8 dark on default, 1.4 dark on subtle; `action.disabled.foreground` on `action.disabled.background` 2.0 light / 2.0 dark.

## 8. Known gaps

- **Token files and Figma out of date.** `color.primitive.tokens.json`, both semantic files and the Figma paint styles still use the old 10–190 values. Regenerate them from section 3, remove the in-between Figma steps (150, 250, …), and create Figma variables (Primitives collection; Semantic collection with Light and Dark modes).
- **Semantic files need the remap in section 4**, plus: add `surface.input` and the 12 `dataviz.*` tokens, rename `jet` to `gray` and `xanthous` to `yellow`, and fix the `soild` typo (`feedback.*.solid.foreground`).
- **Semantic role names are not in Figma.** Only the primitives map to Figma variables. The semantic names and the light and dark assignments were authored for these files.
- **Figma’s native export omits `$description`.** Semantic descriptions (purpose and pairing) would be lost on a round trip through Figma’s export, so the JSON files stay the source of truth.
- **No machine-readable pairing list, status or deprecation fields.** Pairings and rules exist only in descriptions and in this document.
- **Light surfaces are close together.** With `surface.default` at Gray 50, `surface.subtle` (Gray 100) is only 1.08:1 against the page and `surface.raised` (white) is 1.07:1. Cards and panels need a `border.default` edge to separate reliably.
- **`surface.raised` and `surface.muted` are identical in dark mode** (Gray 800). A raised card can't be told apart from a muted area, such as a selected chip. Decide whether that's acceptable, or add a separate dark raised step (for example an elevation overlay).
- **Purple has no semantic tokens yet** and no ramp panel in the Figma Colors frame.
- **Open ramp tuning:** Yellow dark steps turn brown (optional slight hue shift toward orange); tint chroma on 50 and 100 (optional reduction).
- **Light hover overlay fails on `surface.muted`.** The composite (`#cecece`) gives `text.secondary` 4.2:1 and `icon.subtle` 2.97:1. Section 7 verifies the light overlay only on `surface.default`; it also passes on `surface.subtle` and `surface.raised`.
- **No chart colors for dark cards.** Charts cannot sit on dark `surface.raised` or `surface.muted` (see rule 20 and section 7). Decide whether charts in dark cards need their own tokens or a different card surface.
- **`border.default` matches `surface.interactive.selected`** in both modes (`green.200` / `green.900`), so a selected card loses its `border.default` edge.
- **No component tier, high-contrast mode, extra native surface levels or CI validation.**
- **Build tool not chosen.** No CSS, Tailwind theme, Swift or Kotlin outputs are generated yet.
