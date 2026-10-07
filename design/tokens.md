# Indus Cowork demo: design tokens

Source: Figma **Sarvam - Website**, the "Video" section (`1724:83225`). That section has five frames: Prompt, Research, Data, Presentation and Scheduled tasks, plus a Connector card.
The Figma file has **no published variables**, so I took these values from the frame code and checked the colours against the renders.
Only values that appear in those frames are listed. The `:root` block is in [`tokens.css`](./tokens.css).

## Fonts

| Role | Figma font | Weights used | Available here? | Fallback used in the prototype |
|---|---|---|---|---|
| Display (hero question, "Cowork" wordmark) | **Season Mix** | Medium | ❌ No | **Inter Display** → Inter → system sans |
| UI / body (everything else) | **Matter** (file uses `Matter-TRIAL`) | Regular, Medium, Semi Bold | ❌ No | **Inter** → -apple-system / Segoe UI / system-ui |
| Inline code | **Matter Mono** | Regular | ❌ No | ui-monospace / SF Mono / Menlo / DejaVu Sans Mono |

> Season Mix and Matter are commercial fonts from Displaay. They are not installed here, and the repo does not include them.
> Inter is the closest metric match, since Matter is a neutral grotesk. The prototype puts `"Season Mix"` and `"Matter"` first in each stack. If you embed it on web.sarvam.dev, where those `@font-face`s are already loaded, it picks up the real fonts with no change.

## Type scale

| Token | Font | Size / line-height | Tracking | Used for |
|---|---|---|---|---|
| `display` | Season Mix Medium | 32 / 38.4 | +0.41px | "What would you like to know?" |
| `title` | Season Mix Medium | 20 / 24 | −0.45px | "Cowork" sidebar title |
| `body` | Matter Regular (Semi Bold for emphasis) | 16 / 24 | −0.31px | chat messages, composer input |
| `heading` | Matter Regular | 16 / 19.2 | −0.31px | panel header ("Scheduled tasks") |
| `ui` | Matter Regular | 15 / 21.75 (22.5 in cards) | −0.23px | nav rows, tool-run lines, file names |
| `small` | Matter Regular | 14 / 21 | −0.15px | buttons, table headers, descriptions |
| `caption` | Matter Regular | 12 / 18 | 0 | badges, file meta, disclaimer |

## Colour

### Neutrals
| Token | Hex | Used for |
|---|---|---|
| `--color-ink` | `#141414` | primary text, send button, primary button |
| `--color-ink-secondary` | `#525252` | section labels, table headers |
| `--color-ink-muted` | `#6b7280` | "Ran code · Managed files" tool-run lines |
| `--color-ink-placeholder` | `#999999` | "Ask anything…", file meta, connector description |
| `--color-ink-faint` | `#b3b3b3` | disclaimer line |
| `--color-ink-done` | `#bebebe` | struck-through completed steps |
| `--color-surface` | `#ffffff` | panel, cards, composer |
| `--color-surface-sunken` | `#f5f5f5` | **user message bubble**, composer tray, app chrome |
| `--color-surface-hover` | `#f0f0f0` | selected nav row, secondary button |
| `--color-canvas` | `#f7f5f3` | warm page background |
| `--color-border` | `#e6e6e6` | cards, composer |
| `--color-border-subtle` | `#f0f0f0` | row rules, dividers |
| `--color-border-strong` | `#cccccc` | connector card |

### Semantic tints
| Token | Hex | Used for |
|---|---|---|
| `--color-success-bg` / `-fg` | `#e3f1d8` / `#385418` | "Active", "Daily at 09:00", "New" badges |
| `--color-success-icon` | `#80ae4d` | progress check-circles |
| `--color-info-bg` | `#e8effc` | scheduled-task icon tile |
| `--color-danger-bg` | `#fee2e2` | highlighted drop-off cell |
| `--color-warning-bg` | `#fffbeb` | insight row |
| `--color-slate` | `#374151` | spreadsheet header row |

## Spacing

`2 · 4 · 8 · 12 · 16 · 20 · 24 · 28 · 48` px. Typical uses:
- Panel side padding: 28
- Composer padding: 8
- Card padding: 16 (connector) or 20×16 (question card)
- Bubble padding: 16×12
- Stack gaps: 4, 8, 16 and 24

## Radii

| Token | Value | Used for |
|---|---|---|
| `--radius-xs` | 4 | inline code |
| `--radius-sm` | 8 | file and task icon tiles |
| `--radius-md` | 12 | app panel, table, connector card |
| `--radius-lg` | 16 | file attachment card |
| `--radius-xl` | 20 | user bubble, question card |
| `--radius-2xl` | 24 | composer |
| `--radius-pill` | 9999 | buttons, badges, nav rows (Figma uses 3996) |

## Sizes and elevation

- Icon: 16px
- Icon buttons: 36px (send, mic, attach), 28px for the small variant
- Panel header height: 64px
- Content max width: 720px
- Composer shadow: `0 12px 12px #f0f0f0` (the only shadow in the design)

## Not in Figma: needs your call

1. **Citation and accent colour.** The frames are fully monochrome; the only colours are the semantic tints above. The reference video uses blue for citations and the "Generating slides…" text.
   - My proposal: keep it monochrome. Citations would be `#141414` numbers on `#f5f5f5` pills.
   - Alternative: use the existing `#e8effc` tint as the only accent.
2. **User bubble.** Figma uses a light `#f5f5f5` bubble with dark text and a 20px radius. The reference video uses a black bubble. I'm following Figma.
3. **Acme Skincare deck palette** for scene 3. This is story content, not Sarvam UI, so it gets its own scoped tokens. Proposed values, taken from the video:
   - Deep green `#1f5c4a`
   - Cream `#f3efe6`
   - Sage `#5f8f7e`
