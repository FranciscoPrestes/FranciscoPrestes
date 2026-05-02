# Design System — FranciscoPrestes Profile README

Captured from `/init` (interface-design) on 2026-05-02. Out-of-scope per
the skill (READMEs are marketing-leaning), but the user explicitly chose
to apply craft principles here.

---

## Direction

**Hybrid: rich-but-coherent CAIO landing.**

Reads in 10–15s for a recruiter / engineer peer / collaborator. Authority
+ craft + personality. The user rejected the sober "research paper" pass
("tava melhor antes") and explicitly said "carnaval, mas eu achei
interessante" — keep components rich, but ALL components must derive from
a single palette so it does not look like a badge dump.

## Palette — Tokyonight (single visual language)

Every component must draw from these four tokens. No off-palette colors.

| Token       | Hex       | Usage                                                              |
| ----------- | --------- | ------------------------------------------------------------------ |
| `accent-1`  | `#7aa2f7` | Primary blue. Section headers, primary chips, hero gradient start. |
| `accent-2`  | `#bb9af7` | Secondary purple. Secondary chips, point markers, hero gradient mid. |
| `accent-3`  | `#7dcfff` | Tertiary cyan. Tertiary chips, profile views, hero gradient end.   |
| `bg-dark`   | `#1a1b27` | Chip backgrounds. Chip text on accent fills.                       |
| `text-dark` | `#a9b1d6` | Body / labels in dark theme cards.                                 |
| `ink`       | `#1a1b27` | Body / labels in light theme cards.                                |

Stats / streak / activity / trophies use the `theme=tokyonight` preset so
they inherit the palette without per-card customization.

## Depth

Borders-only, low contrast. All cards `hide_border=true`. No shadows,
no gradients except the hero/footer wave (capsule-render).

## Typography

GitHub default sans for body. `font=Fira+Code` for the typing SVG only —
gives a developer-tool signature without overusing monospace.

## Component patterns

### Section header

```html
<img src="https://capsule-render.vercel.app/api?type=transparent&color=7aa2f7&height=70&section=header&text=SECTION%20NAME&fontSize=32&fontColor=7aa2f7&animation=fadeIn" width="100%"/>
```

`type=transparent` keeps it a label, not a banner. `color=fontColor=7aa2f7`
matches accent-1.

### Hero / footer wave

```html
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:7aa2f7,50:bb9af7,100:7dcfff&height=220&section=header&text=...&fontSize=58&fontColor=1a1b27&animation=fadeIn&fontAlignY=38&desc=...&descAlignY=60&descSize=18&descColor=1a1b27" width="100%"/>
```

Footer wave: same gradient inverted (`0:7dcfff,50:bb9af7,100:7aa2f7`),
`section=footer`, `height=120`.

### Accent chip (filled)

```html
<img src="https://img.shields.io/badge/Label-Value-7aa2f7?style=for-the-badge&logo=ICON&logoColor=1a1b27&labelColor=1a1b27"/>
```

Rotate fill across `7aa2f7`, `bb9af7`, `7dcfff` per chip group.

### Dark chip (outline-feel)

```html
<img src="https://img.shields.io/badge/Label-Value-1a1b27?style=for-the-badge&logo=ICON&logoColor=7aa2f7&labelColor=1a1b27"/>
```

Use when you need a longer chip without competing with accent chips.

### Theme-switching card

```html
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="...&theme=tokyonight..."/>
  <source media="(prefers-color-scheme: light)" srcset="...&theme=default..."/>
  <img src="...&theme=tokyonight..." alt="..."/>
</picture>
```

Used for every stats card and the snake animation.

### QuickChart radar

`https://quickchart.io/chart?bkg=transparent&w=560&h=560&c=<URL-ENCODED JSON5>`

- Dark variant: fill `rgba(122,162,247,0.32)`, border `rgb(122,162,247)`,
  points `rgb(187,154,247)`, labels `rgb(169,177,214)`.
- Light variant: fill `rgba(122,162,247,0.25)`, border `rgb(81,43,212)`,
  points `rgb(123,63,228)`, labels `rgb(26,27,39)`.
- `pointLabels.font.size=13, weight='600'`, `ticks.display=false`,
  `suggestedMin=0, suggestedMax=100`.

## Section order (left-to-right reading down the page)

1. Hero wave (capsule, full-width)
2. Typing SVG subtitle
3. Social badges row (LinkedIn, Chess.com, profile views)
4. Daily AI Insight (insight before bio = better hook)
5. About Me (3 accent chips + 2 dark chips + chess fun-fact chip)
6. Skills Radar (QuickChart, centered, 520px)
7. GitHub Metrics (lowlighter/metrics, full-width)
8. Tech Stack (skillicons grid, 6 grouped rows)
9. Contribution Field (3D rainbow + snake)
10. Footer wave (inverted gradient)
11. Quote (centered italic)

## Skills Radar — calibrated values

13 axes, range 70–100. Strong picks at Big Data 100 (master's), Architecture 99,
GenAI 99. Strategy 92 (intentionally not the peak so the technical picks
read first). Weak side floor is 70 — the user is "not incompetent
anywhere", just less invested.

| Axis                | Score |
| ------------------- | ----- |
| Big Data            | 100   |
| Architecture        | 99    |
| GenAI               | 99    |
| Digital Innovation  | 94    |
| Strategy            | 92    |
| Blockchain          | 92    |
| Cloud               | 90    |
| AI / ML             | 88    |
| Backend             | 85    |
| Mobile              | 70    |
| Frontend            | 78    |
| SRE                 | 75    |
| UX / Design         | 75    |

## Automation

| Workflow             | Cron               | Purpose                                                    |
| -------------------- | ------------------ | ---------------------------------------------------------- |
| `daily-insight.yaml` | `5 9 * * *`        | Rotate one of 50 curated AI/ML notes into README markers.  |
| `3d-contrib.yaml`    | `30 3 * * *`       | Regenerate isometric contribution chart SVGs.              |
| `main.yaml` (snake)  | `0 */6 * * *`      | Snake animation light + dark via Platane/snk@v3.           |
| `metrics.yaml`       | `30 */6 * * *`     | lowlighter/metrics — REQUIRES `METRICS_TOKEN` repo secret. |

## Defaults explicitly rejected

- Capsule wave + skillicons + shields for-the-badge ALL with mismatched
  palettes — caused the original "carnival" feel. Fixed by binding
  every component to the tokyonight tokens.
- Architecture Mermaid flowchart — visually weak, didn't actually
  communicate skills. Replaced by Skills Radar.
- Trophies plaque, profile-views as the only stat, generic "About me"
  bullet list with emoji — replaced with chips and metrics SVG.

## Things to avoid in future edits

- Adding a new component without a tokyonight color binding.
- Using `for-the-badge` shields with non-tokyonight hex.
- Letting any card render with default GitHub theme (always pin to
  tokyonight for dark, default for light, via `<picture>`).
- Reintroducing the typing SVG with a non-`#7aa2f7` `color=` param.
- Mixing capsule `type=waving` and `type=transparent` colors —
  transparent is always `#7aa2f7` for section headers.
