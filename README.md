# India AI Startup Policy Explorer
**Artha Global · April 2026**

A self-contained static web app for exploring, filtering and exporting the study's policy recommendations — deployable on GitHub Pages with zero build tooling.

---

## File structure

```
artha-ai-explorer/
├── index.html              ← Single-page app (HTML + CSS + JS)
└── recommendations.json    ← All data. Edit this file to update content.
```

---

## Deploying to GitHub Pages

1. Push both files to a GitHub repository (e.g. `artha-global/ai-policy-explorer`)
2. Go to **Settings → Pages → Source → Deploy from branch** → select `main` / `root`
3. Your site is live at `https://artha-global.github.io/ai-policy-explorer`

**iFrame note:** GitHub Pages can be embedded in an `<iframe>` on your Artha website **only if** you add the following HTTP header to the GitHub Pages response:

```
X-Frame-Options: ALLOWALL
```

GitHub Pages does not let you set custom HTTP headers. The workaround:
- Use a `<meta>` tag in index.html: `<meta http-equiv="Content-Security-Policy" content="frame-ancestors *;">` (already included)
- This controls what *index.html* allows — but the *parent* page's CSP also matters
- If artha.global uses a strict `frame-ancestors` policy, you may need to host the explorer on a subdomain (`explorer.artha.global`) and embed from there

**Simpler alternative:** Link to the GitHub Pages URL with a styled button/card — avoid the iframe complexity entirely.

---

## Recommendation schema (`recommendations.json`)

Each recommendation is a JSON object:

```jsonc
{
  "id": "t-1",           // Unique ID. Prefix convention: t=talent, m=market, f=financing, p=policy, i=infra
  "domain": "talent",    // One of: "talent" | "market" | "financing" | "policy" | "infra"
  "tier": "now",         // One of: "now" | "build" | "monitor"
  "horizon": "0–2 years", // Free text shown in the detail drawer
  "title": "...",        // Short title (shown on card and timeline). ~60 chars max.
  "desc": "...",         // Full description shown in the detail drawer. 2–4 sentences.
  "actors": ["national"], // Array. Allowed values: "national" | "ecosystem" | "vc" | "academia"
  "evidence": [          // Array of strings. Each string is one evidence data point from the study.
    "...",               // Shown in the Evidence tab of the detail drawer.
    "..."
  ],
  "implications": [      // Array of strings. Each is one "if not acted" consequence.
    "...",               // Shown in the "If not acted" tab of the detail drawer.
    "..."
  ]
}
```

### Tier values and their meaning

| Value | Label | Meaning |
|-------|-------|---------|
| `"now"` | Act Now | High domestic control, 0–3 year horizon. India has full policy agency. |
| `"build"` | Build Toward | Structural reform needed, 3–7 year horizon. Achievable but slower. |
| `"monitor"` | Monitor | Low domestic leverage. Watch global trends and optimise existing programmes. |

### Domain values

| Value | Label |
|-------|-------|
| `"talent"` | Talent & R&D |
| `"market"` | Market Innovation |
| `"financing"` | Financing |
| `"policy"` | Policy & Regulation |
| `"infra"` | Infrastructure & GPU |

### Actor values

| Value | Label |
|-------|-------|
| `"national"` | Central Government |
| `"ecosystem"` | Startup Ecosystem |
| `"vc"` | VC / Investors |
| `"academia"` | Academia |

---

## How to update a recommendation

Open `recommendations.json` in any text editor. Find the recommendation by `id` and edit any field. Save. Reload the page.

**No build step. No npm. No compilation.**

---

## How to add a new recommendation

Copy any existing object, give it a new unique `id`, fill in all fields, and add it to the array. The app will automatically include it in all views, filters and exports.

---

## How to change priority/tier

Find the recommendation by `id`. Change `"tier": "now"` to `"build"` or `"monitor"` (or vice versa). Save. The card badge, filter, stats counter, narrative timeline, and matrix all update automatically.

---

## Export formats

The app exports the currently filtered set of recommendations as:
- **Markdown** — copy to clipboard, paste into Notion, Confluence, or any document
- **CSV** — download, open in Excel or Google Sheets

---

## Light / dark mode

Automatic via `prefers-color-scheme`. Manual toggle in the top-right corner. No preference stored (sandbox iframe limitation).
