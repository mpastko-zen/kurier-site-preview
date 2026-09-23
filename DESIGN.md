# Kurier Design System — "Datasheet" (Design 2)

A re-skin guide for applying the Kurier visual system to another website. Everything here was extracted from the shipped pages (homepage, testnet, login, blog index, four posts) so the values are the real ones, not approximations.

Reference implementation: https://mpastko-zen.github.io/kurier-site-preview/ (homepage, `/testnet/`, `/login/`, `/blog/`).

---

## 1. The idea in one paragraph

A **technical datasheet printed on paper**, not a software landing page. Flat colour, zero border radius, 2px ink rules dividing everything, a faint exposed 12-column grid behind the page, print crop marks at section corners, poster-scale condensed uppercase headlines, mono annotations, and one graphic language: **squares on a grid** (the "scatter field"). Hover states invert to ink-on-paper. Nothing glows, nothing is rounded, nothing has a drop shadow.

When in doubt: remove the effect, add a rule.

---

## 2. Colour

Four brand colours, used flat.

| Role | Hex | Use |
|---|---|---|
| Lavender (dominant) | `#5C72FF` | Solid blocks (signup form, section bands), headline accent spans, step numerals, links, active markers |
| Ink (dark) | `#020212` | Text on paper; page ground in dark mode; graphic plates are always ink |
| Mint (secondary) | `#B5FFA5` | Small markers only: the square on network/section tags, the "kick" cell of the logo, list bullets, the selected item in menus. **Never a hover colour, never a large fill** (the user rejected both). |
| Paper (white) | `#FCFCFC` | Page ground in light mode; text on ink and on lavender |

Derived neutrals (light theme): `--ink-2: #2E2E48` (secondary text), `--muted: #63637A`, `--paper-2: #F1F1F6`, rules `rgba(2,2,18,.18)`, grid `rgba(2,2,18,.07)`.

**Dark mode** inverts paper and ink (`#020212` ground, `#FCFCFC` text); lavender and mint stay the same. Link text lightens to `#8A9AFF` on ink. The lavender bands and the ink plates look identical in both themes.

Contrast note: lavender on white is ~3.9:1, so use it for large type, blocks and underlined links, never for small body copy.

### Token block (copy verbatim)

```css
:root{
  color-scheme:light;
  --paper:#FCFCFC; --paper-2:#F1F1F6; --ink:#020212; --ink-2:#2E2E48; --muted:#63637A;
  --rule:#020212; --rule-soft:rgba(2,2,18,.18); --grid:rgba(2,2,18,.07);
  --blue:#5C72FF; --blue-ink:#FCFCFC; --blue-text:#5C72FF; --blue-soft:rgba(92,114,255,.10);
  --mint:#B5FFA5; --mint-ink:#020212;
  --invert-bg:#020212; --invert-fg:#FCFCFC;
  --display:"Archivo","Arial Narrow",Impact,sans-serif;
  --body:"IBM Plex Sans",system-ui,-apple-system,"Segoe UI",sans-serif;
  --mono:"IBM Plex Mono",ui-monospace,SFMono-Regular,Menlo,monospace;
  --gutter:clamp(16px,3.5vw,48px);
}
@media (prefers-color-scheme: dark){
  :root:not([data-theme="light"]){
    color-scheme:dark;
    --paper:#020212; --paper-2:#0B0B24; --ink:#FCFCFC; --ink-2:#C9C9DA; --muted:#8C8CA6;
    --rule:#FCFCFC; --rule-soft:rgba(252,252,252,.22); --grid:rgba(252,252,252,.06);
    --blue:#5C72FF; --blue-ink:#FCFCFC; --blue-text:#8A9AFF; --blue-soft:rgba(92,114,255,.16);
    --mint:#B5FFA5; --mint-ink:#020212;
    --invert-bg:#FCFCFC; --invert-fg:#020212;
  }
}
:root[data-theme="dark"]{
  color-scheme:dark;
  --paper:#020212; --paper-2:#0B0B24; --ink:#FCFCFC; --ink-2:#C9C9DA; --muted:#8C8CA6;
  --rule:#FCFCFC; --rule-soft:rgba(252,252,252,.22); --grid:rgba(252,252,252,.06);
  --blue:#5C72FF; --blue-ink:#FCFCFC; --blue-text:#8A9AFF; --blue-soft:rgba(92,114,255,.16);
  --mint:#B5FFA5; --mint-ink:#020212;
  --invert-bg:#FCFCFC; --invert-fg:#020212;
}
```

Theme rules: bare `:root` is the light palette; `@media (prefers-color-scheme: dark)` redefines tokens guarded as `:root:not([data-theme="light"])`; `:root[data-theme="dark"]` redefines them again so a toggle wins in both directions. A header toggle stamps `data-theme` on `<html>` and remembers the choice in `localStorage` (`kurier-theme`).

---

## 3. Typography

Three faces, all from Google Fonts:

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Archivo:ital,wdth,wght@0,62..125,400..900;1,62..125,400..900&family=IBM+Plex+Mono:wght@400;500;600&family=IBM+Plex+Sans:ital,wght@0,400;0,500;0,600;1,400&display=swap">
```

| Role | Face | Settings |
|---|---|---|
| Display (h1/h2/h3, proof names, big numerals) | **Archivo** variable | `font-variation-settings:"wdth" 62` (extra-condensed), weight 800 (900 for the hero), `text-transform:uppercase`, `letter-spacing:-.01em` to `-.02em`, `line-height:.86–.9` |
| Labels / eyebrows | **Archivo** | `"wdth" 125` (expanded), weight 600, uppercase, `font-size:.72rem`, `letter-spacing:.14em` |
| Sub-headings inside lists (dt, h3 in articles) | **Archivo** | `"wdth" 75`, weight 700, uppercase, ~1.2rem |
| Body | **IBM Plex Sans** | 1.0625rem / 1.55; lead paragraphs 1.25–1.5rem weight 500 |
| Annotations (nav, tags, dates, counters, code, footer links) | **IBM Plex Mono** | .74–.8rem, uppercase, `letter-spacing:.06–.1em` |

Scale:
- Hero h1: `clamp(3.2rem, 9.8vw, 10rem)`, weight 900, three forced lines on desktop (`<br>` hidden below 640px, `word-spacing:.08em` added there). The key phrase gets `color:var(--blue)` via a span.
- Section h2: `clamp(3rem, 7vw, 6.5rem)`.
- Page h1 on secondary pages (Blog, Log In): `clamp(3.4rem, 11vw, 11rem)`.
- Big numerals (steps): `clamp(5rem, 9vw, 9rem)` in lavender.

Copy rules used on Kurier (house style): "onchain" never "on-chain"; headings sentence-cased in HTML and uppercased by CSS; buttons are short imperatives.

---

## 4. Layout

- **Full-bleed sections**, no max-width container. Side gutter `--gutter: clamp(16px, 3.5vw, 48px)` on a `.pad` wrapper.
- **Exposed grid**: `body` carries `repeating-linear-gradient(90deg, var(--grid) 0 1px, transparent 1px calc(100% / 12))` — 12 faint columns behind everything.
- **Rules, not cards**: sections are separated by `border-top: 2px solid var(--rule)`; inner separators are 1px `var(--rule-soft)`. Cards exist only for blog posts, and even those are ruled cells in a grid with no gaps.
- **Crop marks**: every `.marks` section draws a mono `+` at its top-left and top-right corners (`::before/::after`, positioned at `calc(var(--gutter) - .35em)`).
- **Zero radius everywhere.** No `border-radius` in the whole system.
- **Section head pattern**: title left, call-to-action button right, aligned to the baseline (`grid-template-columns: minmax(0,1fr) auto; align-items:end`), stacking below 700px.
- Section vertical rhythm: `padding-block: clamp(3rem, 6vw, 5.5rem)`.
- Two-column content blocks use `minmax(0,5fr) minmax(0,7fr)` (heading | body) or `7fr | 5fr` (copy | form), collapsing to one column at 820–900px.

```css
*,*::before,*::after{box-sizing:border-box}
html{scroll-behavior:smooth}
body{margin:0;background:var(--paper);color:var(--ink);font-family:var(--body);font-size:1.0625rem;line-height:1.55;-webkit-font-smoothing:antialiased;overflow-x:hidden;
  background-image:repeating-linear-gradient(90deg,var(--grid) 0 1px,transparent 1px calc(100% / 12));}
a{color:inherit;text-decoration:none}
p{margin:0}
h1,h2,h3{margin:0;font-family:var(--display);font-variation-settings:"wdth" 62;font-weight:800;text-transform:uppercase;letter-spacing:-.01em;line-height:.9}
h2{font-size:clamp(3rem,7vw,6.5rem)}
h3{font-size:clamp(1.6rem,2.6vw,2.2rem)}
code{font-family:var(--mono);font-size:.9em;background:var(--blue-soft);color:var(--blue-text);padding:.05em .35em;border:1px solid var(--rule-soft)}
:focus-visible{outline:2px solid var(--blue);outline-offset:2px}
.form :focus-visible{outline-color:var(--blue-ink)}
.label{font-family:var(--display);font-variation-settings:"wdth" 125;font-weight:600;text-transform:uppercase;font-size:.72rem;letter-spacing:.14em}
.mono{font-family:var(--mono);font-size:.8rem;letter-spacing:.02em;text-transform:uppercase}
.txt-link{color:var(--blue-text);border-bottom:1px solid currentColor;transition:background .15s,color .15s}
.txt-link:hover{background:var(--blue);color:var(--blue-ink);border-color:var(--blue)}
.pad{padding-inline:var(--gutter)}
.rule{border-top:2px solid var(--rule)}

/* crop marks */
.marks{position:relative}
.marks::before,.marks::after{content:"+";position:absolute;top:-.55em;font-family:var(--mono);font-size:.85rem;color:var(--muted);line-height:1;pointer-events:none}
.marks::before{left:calc(var(--gutter) - .35em)}
.marks::after{right:calc(var(--gutter) - .35em)}

/* section shell */
.sec{padding-block:clamp(3rem,6vw,5.5rem)}
.sec-head{display:grid;grid-template-columns:minmax(0,1fr) auto;gap:1.5rem 3rem;align-items:end;margin-bottom:clamp(1.5rem,4vw,3rem)}
.sec-head .lead{margin-top:1rem;color:var(--ink-2);max-width:56ch;font-size:1.05rem}
@media (max-width:700px){.sec-head{grid-template-columns:1fr;align-items:start}}
```

---

## 5. Components

### Header
Sticky, paper ground, 2px ink bottom rule, 64px tall. Cells separated by 1px soft rules: logo cell (right rule) · network switcher · nav (mono uppercase links, each with a left rule, hover inverts) · theme toggle (sun/moon icon + "Light"/"Dark" mono label) · hamburger below 900px. Current nav item is permanently inverted.

```css
/* header */
.top{position:sticky;top:env(safe-area-inset-top,0px);z-index:40;background:var(--paper);border-bottom:2px solid var(--rule)}
.top .row{display:flex;align-items:stretch;min-height:64px}
.top .logo{display:flex;align-items:center;padding-right:1.5rem;border-right:1px solid var(--rule-soft)}
.logo svg{height:30px;width:auto;display:block}
.foot .logo svg{height:26px}
.tag{display:flex;align-items:center;padding:0 1.1rem;border-right:1px solid var(--rule-soft);color:var(--blue-text);font-family:var(--mono);font-size:.78rem;font-weight:600;letter-spacing:.1em;text-transform:uppercase;white-space:nowrap}
.tag::before{content:"";width:8px;height:8px;background:var(--mint);outline:1px solid var(--rule);margin-right:.6rem}
.top nav{margin-left:auto;display:flex}
.top nav a{display:flex;align-items:center;padding:0 1.15rem;border-left:1px solid var(--rule-soft);font-family:var(--mono);font-size:.8rem;letter-spacing:.06em;text-transform:uppercase;transition:background .15s,color .15s}
.top nav a:hover,.top nav a[aria-current]{background:var(--invert-bg);color:var(--invert-fg)}
.menu-btn{display:none;margin-left:auto;border:0;border-left:1px solid var(--rule-soft);background:none;color:var(--ink);width:64px;cursor:pointer;align-items:center;justify-content:center}
.menu-btn svg{width:24px;height:24px}
.theme-btn{display:flex;align-items:center;gap:.6rem;padding:0 1.15rem;border:0;border-left:1px solid var(--rule-soft);background:none;color:var(--ink);font-family:var(--mono);font-size:.8rem;letter-spacing:.06em;text-transform:uppercase;cursor:pointer;transition:background .15s,color .15s}
.theme-btn:hover{background:var(--invert-bg);color:var(--invert-fg)}
.theme-btn svg{width:18px;height:18px}
.theme-btn .ico-sun,.theme-btn .ico-moon{display:none}
.theme-btn[data-mode="dark"] .ico-sun{display:block}
.theme-btn[data-mode="light"] .ico-moon{display:block}
.theme-btn .t-light,.theme-btn .t-dark{display:none}
.theme-btn[data-mode="dark"] .t-light{display:inline}
.theme-btn[data-mode="light"] .t-dark{display:inline}
@media (max-width:900px){
  .top nav{display:none;position:absolute;left:0;right:0;top:100%;flex-direction:column;background:var(--paper);border-bottom:2px solid var(--rule)}
  .top nav.open{display:flex}
  .top nav a{padding:1rem var(--gutter);border-left:0;border-top:1px solid var(--rule-soft)}
  .menu-btn{display:flex;margin-left:0}
  .theme-btn{margin-left:auto}
  .theme-btn .t-light,.theme-btn .t-dark{display:none!important}
  .tag{display:none}
  .net-btn{padding:0 .8rem}
  .net-btn svg{display:none}
}
```

### Network switcher (Mainnet / Testnet)
A mono button with a mint square and a chevron; opens a ruled dropdown listing each network with its hostname in small mono. Selected item's marker is mint, others lavender. Closes on outside click and Escape.

```css
/* network switcher */
.net{position:relative;display:flex;align-items:stretch;border-right:1px solid var(--rule-soft)}
.net-btn{display:flex;align-items:center;gap:.6rem;padding:0 1.1rem;background:none;border:0;color:var(--blue-text);font-family:var(--mono);font-size:.78rem;font-weight:600;letter-spacing:.1em;text-transform:uppercase;white-space:nowrap;cursor:pointer;transition:background .15s,color .15s}
.net-btn:hover,.net.open .net-btn{background:var(--invert-bg);color:var(--invert-fg)}
.net-btn svg{width:14px;height:14px;transition:transform .15s}
.net.open .net-btn svg{transform:rotate(180deg)}
.net-dot{width:8px;height:8px;background:var(--mint);outline:1px solid var(--rule)}
.net-menu{display:none;position:absolute;top:100%;left:-1px;min-width:220px;background:var(--paper);border:2px solid var(--rule);z-index:60}
.net.open .net-menu{display:block}
.net-menu a{display:grid;grid-template-columns:auto 1fr;align-items:center;gap:.2rem .7rem;padding:.75rem 1rem;font-family:var(--mono);font-size:.78rem;letter-spacing:.08em;text-transform:uppercase;border-bottom:1px solid var(--rule-soft);transition:background .15s,color .15s}
.net-menu a:last-child{border-bottom:0}
.net-menu a::before{content:"";width:8px;height:8px;background:var(--blue)}
.net-menu a[aria-selected="true"]::before{background:var(--mint)}
.net-menu a small{grid-column:2;font-size:.66rem;letter-spacing:.04em;text-transform:none;opacity:.7}
.net-menu a:hover{background:var(--invert-bg);color:var(--invert-fg)}
```

### Buttons
One style: 2px ink outline, transparent fill, Archivo `"wdth" 100` weight 700 uppercase .85rem with .06em tracking, arrow icon with square line caps. Hover inverts to ink fill / paper text. On lavender bands the same button is white-outlined and hovers to white fill with lavender text. No filled-colour button variants anywhere.

```css
/* buttons */
.btn{display:inline-flex;align-items:center;gap:.7rem;padding:.9rem 1.3rem;border:2px solid var(--rule);background:transparent;color:var(--ink);font-family:var(--display);font-variation-settings:"wdth" 100;font-weight:700;text-transform:uppercase;letter-spacing:.06em;font-size:.85rem;line-height:1;cursor:pointer;transition:background .15s,color .15s,border-color .15s}
.btn:hover{background:var(--invert-bg);color:var(--invert-fg);border-color:var(--invert-bg)}
.btn .arr{width:16px;height:16px}
.btn-blue{background:var(--blue);color:var(--blue-ink);border-color:var(--blue)}
.btn-blue:hover{background:var(--invert-bg);border-color:var(--invert-bg);color:var(--invert-fg)}
.btn-fill{background:var(--invert-bg);color:var(--invert-fg);border-color:var(--invert-bg)}
.btn-fill:hover{background:var(--blue);border-color:var(--blue);color:var(--blue-ink)}
```

### Hero
Headline → expanded label with a 2px rule running to the right edge (`.built::after`) → a ruled two-column band: copy (with a 2px right rule) | form block.

```css
/* hero */
.hero{padding-block:clamp(2rem,5vw,4rem) 0}
.hero h1{font-size:clamp(3.2rem,9.8vw,10rem);line-height:.86;letter-spacing:-.02em;font-weight:900}
.hero h1 .l2{color:var(--blue)}
@media (max-width:640px){.hero h1 br{display:none}.hero h1{word-spacing:.08em}}
.hero .built{margin-top:clamp(1rem,2.5vw,1.75rem);display:flex;align-items:center;gap:1rem}
.hero .built .label{font-size:clamp(.85rem,1.4vw,1.05rem)}
.hero .built::after{content:"";flex:1;height:2px;background:var(--rule)}
.hero-grid{margin-top:clamp(2rem,4vw,3rem);display:grid;grid-template-columns:minmax(0,7fr) minmax(0,5fr);border-top:2px solid var(--rule)}
.hero-copy{padding:clamp(1.5rem,3vw,2.5rem) clamp(1.5rem,4vw,3.5rem) clamp(1.5rem,3vw,2.5rem) 0;border-right:2px solid var(--rule);display:grid;gap:1.2rem;align-content:start;font-size:1.125rem}
.hero-copy p{max-width:62ch}
.hero-copy p:first-child{font-size:clamp(1.25rem,1.9vw,1.5rem);line-height:1.35;font-weight:500}
.hero-copy .actions{margin-top:.8rem;display:flex;gap:.75rem;flex-wrap:wrap}
.hero-copy strong{font-weight:600}
.form{background:var(--blue);color:var(--blue-ink);padding:clamp(1.5rem,3vw,2.5rem)}
.form .tag{border:0;padding:0;color:var(--blue-ink)}
.form .tag::before{background:var(--mint);outline:0}
.form h2{margin-top:1rem;font-size:clamp(2.2rem,3.6vw,3.2rem);letter-spacing:0}
.fields{display:grid;gap:0;margin-top:1.5rem;border:2px solid var(--blue-ink)}
.field{display:grid;grid-template-columns:120px 1fr;border-bottom:2px solid var(--blue-ink)}
.field:last-of-type{border-bottom:0}
.field label{padding:.8rem .9rem;font-family:var(--mono);font-size:.74rem;letter-spacing:.08em;text-transform:uppercase;display:grid;align-content:center;border-right:2px solid var(--blue-ink)}
.field label small{display:block;font-size:.62rem;opacity:.75;letter-spacing:.04em;text-transform:none}
.field input{width:100%;font:inherit;font-size:1rem;color:var(--blue-ink);background:transparent;border:0;padding:.8rem .9rem;min-width:0}
.field input::placeholder{color:rgba(252,252,252,.6)}
.field input:focus{outline:none;background:rgba(2,2,18,.18)}
.agree{display:flex;gap:.7rem;align-items:flex-start;margin-top:1.1rem;font-size:.9rem}
.agree input{width:18px;height:18px;margin-top:.15rem;accent-color:#fff;flex:none}
.agree a{color:inherit;border-bottom:1px solid currentColor}
.form .btn{width:100%;justify-content:space-between;margin-top:1.25rem;border-color:var(--blue-ink);color:var(--blue-ink)}
.form .btn:hover{background:var(--blue-ink);color:var(--blue)}
.form .status{min-height:1.2em;margin-top:.7rem;font-family:var(--mono);font-size:.78rem;opacity:.9}
@media (max-width:860px){
  .hero-grid{grid-template-columns:1fr}
  .hero-copy{border-right:0;border-bottom:2px solid var(--rule);padding-right:0}
  .field{grid-template-columns:100px 1fr}
}
```

### Form block (signup, login)
Lavender fill, white text. Fields are a bordered table: label cell (mono, uppercase, 120px, right rule) | input cell (transparent, white placeholder at 60%, focus darkens the cell). Rows divided by 2px white rules. A mono `(required)` note sits under the label. Checkbox uses `accent-color:#fff`. Submit is the outlined button, full width, arrow pushed right.

### Feature grid ("Why")
Four ruled columns between two 2px rules; each has a 44px outlined glyph box, a condensed h3 (~3rem), and body copy. Collapses 4 → 2 → 1.

```css
/* why */
.why{display:grid;grid-template-columns:repeat(4,1fr);border-top:2px solid var(--rule);border-bottom:2px solid var(--rule)}
.why article{padding:1.5rem 1.4rem 2rem 0;border-right:1px solid var(--rule-soft);margin-right:1.4rem;display:grid;grid-template-rows:auto auto 1fr;gap:1rem;transition:background .15s}
.why article:last-child{border-right:0;margin-right:0}
.why .glyph{width:44px;height:44px;border:2px solid var(--rule);display:grid;place-items:center}
.why .glyph svg{width:22px;height:22px}
.why h3{font-size:clamp(2rem,3.4vw,3rem)}
.why p{color:var(--ink-2);font-size:.98rem}
@media (max-width:960px){.why{grid-template-columns:1fr 1fr}.why article{border-bottom:1px solid var(--rule-soft);padding-top:1.4rem}.why article:nth-child(2n){border-right:0;margin-right:0}.why article:nth-last-child(-n+2){border-bottom:0}}
@media (max-width:520px){.why{grid-template-columns:1fr}.why article{border-right:0;margin-right:0;border-bottom:1px solid var(--rule-soft)}.why article:last-child{border-bottom:0}}
```

### Ruled list (proof types)
Rows: condensed uppercase name (`clamp(2rem,4.2vw,3.6rem)`) | mono uppercase description. Whole row inverts on hover and indents 1rem.

```css
/* proof list */
.prooflist{border-top:2px solid var(--rule)}
.proof{display:grid;grid-template-columns:minmax(0,5fr) minmax(0,7fr);gap:1.5rem;align-items:baseline;padding:1rem 0;border-bottom:1px solid var(--rule-soft);transition:background .15s,color .15s,padding-left .15s}
.proof:last-child{border-bottom:2px solid var(--rule)}
.proof:hover{background:var(--invert-bg);color:var(--invert-fg);padding-left:1rem;padding-right:1rem}
.proof .name{font-family:var(--display);font-variation-settings:"wdth" 62;font-weight:800;text-transform:uppercase;font-size:clamp(2rem,4.2vw,3.6rem);line-height:.95;letter-spacing:-.01em}
.proof .desc{font-family:var(--mono);font-size:.85rem;letter-spacing:.02em;text-transform:uppercase;color:inherit;opacity:.85}
@media (max-width:640px){.proof{grid-template-columns:1fr;gap:.4rem}}
```

### Lavender band (Randomness, Pricing, Networks)
Full-bleed `background: var(--blue)`, white text, token overrides so rules/links/buttons read white. Left: mono label with a mint square + h2 + copy + one white-outlined button. Right: `.uses` list — condensed uppercase lines between rules, optional mono `<small>` sub-line.

```css
/* verifiable randomness */
.vrf{background:var(--blue);color:var(--blue-ink);--ink:var(--blue-ink);--ink-2:rgba(252,252,252,.85);--rule:var(--blue-ink);--rule-soft:rgba(252,252,252,.35);--muted:rgba(252,252,252,.7);--blue-text:var(--blue-ink);--invert-bg:#FCFCFC;--invert-fg:#5C72FF;--blue-soft:rgba(252,252,252,.14)}
.vrf-grid{display:grid;grid-template-columns:minmax(0,7fr) minmax(0,5fr);gap:clamp(2rem,5vw,4rem);align-items:end}
.vrf .label{display:flex;align-items:center;gap:.6rem;margin-bottom:1rem;font-family:var(--mono);font-variation-settings:normal;font-weight:600;font-size:.78rem;letter-spacing:.1em}
.vrf .label::before{content:"";width:8px;height:8px;background:var(--mint)}
.vrf-copy{margin-top:1.5rem;display:grid;gap:1.1rem;font-size:clamp(1.15rem,1.7vw,1.4rem);line-height:1.4;max-width:56ch}
.vrf .actions{margin-top:1.75rem;display:flex;gap:.75rem;flex-wrap:wrap}
.vrf .btn{border-color:var(--blue-ink);color:var(--blue-ink);min-width:min(100%,320px);justify-content:space-between}
.vrf .btn:hover{background:var(--blue-ink);color:var(--blue);border-color:var(--blue-ink)}
.uses{list-style:none;margin:0;padding:0;border-top:2px solid var(--rule)}
.uses li{padding:.9rem 0;border-bottom:1px solid var(--rule-soft);font-family:var(--display);font-variation-settings:"wdth" 62;font-weight:800;text-transform:uppercase;font-size:clamp(1.8rem,3vw,2.6rem);line-height:1;letter-spacing:-.01em}
.uses li:last-child{border-bottom:2px solid var(--rule)}
.pricing .vrf-grid{align-items:start}
.pricing .uses{margin-top:.25rem}
@media (max-width:860px){.vrf-grid{grid-template-columns:1fr}}
```

### Definition list ("Who uses", "Why testnet")
`dt` in Archivo `"wdth" 75` 700 uppercase, `dd` in body colour, each row ruled.

```css
/* who */
.who{display:grid;grid-template-columns:minmax(0,5fr) minmax(0,7fr);gap:clamp(1.5rem,5vw,4rem)}
.who .lead{margin-top:1rem;color:var(--ink-2)}
.who dl{margin:0;border-top:2px solid var(--rule)}
.who .row{display:grid;grid-template-columns:minmax(0,1fr) minmax(0,1.3fr);gap:1rem;padding:1.1rem 0;border-bottom:1px solid var(--rule-soft);align-items:baseline}
.who .row:last-child{border-bottom:2px solid var(--rule)}
.who dt{font-family:var(--display);font-variation-settings:"wdth" 75;font-weight:700;text-transform:uppercase;font-size:1.25rem;letter-spacing:.01em}
.who dd{margin:0;color:var(--ink-2);font-size:1rem}
@media (max-width:820px){.who{grid-template-columns:1fr}}
@media (max-width:520px){.who .row{grid-template-columns:1fr;gap:.3rem}}
```

### Numbered steps
Giant lavender numerals over short copy, columns ruled, `align-content:start` so text tops align. Four or five columns → 2 → 1. Numbering is only used where order is real.

```css
/* steps */
.steps{list-style:none;margin:0;padding:0;display:grid;grid-template-columns:repeat(4,1fr);border-top:2px solid var(--rule);border-bottom:2px solid var(--rule)}
.steps li{padding:1.25rem 1.4rem 2rem 0;margin-right:1.4rem;border-right:1px solid var(--rule-soft);display:grid;grid-template-rows:auto 1fr;align-content:start;align-items:start;gap:.75rem}
.steps li:last-child{border-right:0;margin-right:0}
.steps .num{font-family:var(--display);font-variation-settings:"wdth" 62;font-weight:900;font-size:clamp(5rem,9vw,9rem);line-height:.8;color:var(--blue);letter-spacing:-.04em}
.steps p{font-size:1.05rem;color:var(--ink-2);max-width:24ch}
@media (max-width:900px){.steps{grid-template-columns:1fr 1fr}.steps li{border-bottom:1px solid var(--rule-soft)}.steps li:nth-child(2n){border-right:0;margin-right:0}.steps li:nth-last-child(-n+2){border-bottom:0}}
@media (max-width:480px){.steps{grid-template-columns:1fr}.steps li{border-right:0;margin-right:0;border-bottom:1px solid var(--rule-soft)}.steps li:last-child{border-bottom:0}}
```

### Roadmap table
A real `<table>`: ink header cells with condensed quarter labels, plain text rows divided by soft rules, no hover, no icons (items are not links). Horizontal scroll below 760px.

```css
/* roadmap table */
.rm-wrap{overflow-x:auto}
.rm{width:100%;border-collapse:collapse;min-width:760px;border:2px solid var(--rule)}
.rm th{background:var(--invert-bg);color:var(--invert-fg);text-align:left;padding:.9rem 1rem;font-family:var(--display);font-variation-settings:"wdth" 62;font-weight:800;font-size:2rem;text-transform:uppercase;letter-spacing:.01em;line-height:1;border-right:1px solid var(--paper)}
.rm th:last-child{border-right:0}
.rm td{vertical-align:top;padding:0;border-right:1px solid var(--rule-soft);width:33.333%}
.rm td:last-child{border-right:0}
.rm ul{list-style:none;margin:0;padding:0}
.rm li{padding:.95rem 1rem;border-bottom:1px solid var(--rule-soft);font-size:1rem;font-weight:500}
.rm li:last-child{border-bottom:0}
```

### Blog cards
Ruled grid cells (no gaps): 16:9 generated graphic plate · mono date in lavender · condensed title · body. Whole card inverts on hover. 3 columns (index uses 2) → 1.

```css
/* blog */
.posts{display:grid;grid-template-columns:repeat(3,1fr);gap:0;border-top:2px solid var(--rule);border-bottom:2px solid var(--rule)}
.post{display:grid;grid-template-rows:auto auto 1fr;border-right:1px solid var(--rule-soft);transition:background .15s,color .15s}
.post:last-child{border-right:0}
.post:hover{background:var(--invert-bg);color:var(--invert-fg)}
.post .thumb{aspect-ratio:16/9;max-width:100%;overflow:hidden;border-bottom:1px solid var(--rule-soft)}
.post .thumb svg{width:100%;height:100%;display:block}
.post .meta{padding:.8rem 1.2rem 0;font-family:var(--mono);font-size:.75rem;letter-spacing:.06em;text-transform:uppercase;color:var(--blue-text)}
.post:hover .meta{color:inherit}
.post .body{padding:.6rem 1.2rem 1.6rem;display:grid;gap:.7rem;align-content:start}
.post h3{font-size:clamp(1.5rem,2.2vw,1.9rem);letter-spacing:0}
.post p{font-size:.95rem;opacity:.8}
@media (max-width:900px){.posts{grid-template-columns:1fr}.post{border-right:0;border-bottom:1px solid var(--rule-soft)}.post:last-child{border-bottom:0}}
```

### Article (blog post)
68ch column; h2 with a 2px top rule and 2.75rem top margin; h3 with a lavender square marker in `"wdth" 75`; `ul` with mint square bullets; `ol` as ruled rows with mono `01 02 03` counters in lavender; `pre` on an ink plate (`--code-bg`) with a 2px border, IBM Plex Mono .86rem; a sticky mono "On this page" contents list built from the h2s.

### Footer
Full-bleed ink band (paper in dark mode): logo · two-column mono link list · copyright, legal links and square social icons in a bordered row.

```css
/* footer */
.foot{background:var(--invert-bg);color:var(--invert-fg);padding-block:3rem 2rem;margin-top:clamp(2rem,4vw,3.5rem)}
.foot .logo{color:var(--invert-fg)}
.foot-top{display:grid;grid-template-columns:minmax(0,1fr) auto;gap:2rem;align-items:start;padding-bottom:2.5rem;border-bottom:1px solid rgba(128,128,128,.4)}
.foot-links{display:grid;grid-template-columns:repeat(2,auto);gap:.55rem 3rem}
.foot-links a{font-family:var(--mono);font-size:.8rem;letter-spacing:.06em;text-transform:uppercase;opacity:.85;transition:opacity .15s}
.foot-links a:hover{opacity:1;color:var(--blue-text)}
.foot-bottom{display:flex;flex-wrap:wrap;gap:1rem 2rem;justify-content:space-between;align-items:center;padding-top:1.5rem;font-family:var(--mono);font-size:.76rem;letter-spacing:.04em;text-transform:uppercase;opacity:.85}
.legal{display:flex;gap:1.2rem;flex-wrap:wrap}
.legal a:hover,.legal button:hover{color:var(--blue-text)}
.legal button{background:none;border:0;padding:0;font:inherit;color:inherit;cursor:pointer;text-transform:inherit;letter-spacing:inherit}
.social{display:flex}
.social a{width:40px;height:40px;border:1px solid rgba(128,128,128,.5);margin-left:-1px;display:grid;place-items:center;transition:background .15s,color .15s}
.social a:hover{background:var(--blue);color:var(--blue-ink);border-color:var(--blue)}
.social svg{width:18px;height:18px}
@media (max-width:700px){.foot-top{grid-template-columns:1fr}}
```

### Theme toggle & mobile nav
Small vanilla JS: stamp `data-theme`, persist in `localStorage` (try/catch), toggle `.open` on the nav below 900px. Forms in mockups `preventDefault()` and print a status line.

---

## 6. Graphic language: squares on a grid

Every illustration is built from **20px squares on a 25px pitch** (5px gap). Colours: lavender majority, mint and white/ink as sparse "bits". Backgrounds for plates are always ink with a faint lavender line grid (60px pitch, 18–22% opacity).

Motifs in use:
- **Field K (the logo)**: 6×6 field of lavender squares at ~36% density with a 6-tall K standing out in white (ink on paper), the kick cell in mint. Wordmark: Archivo extra-condensed 900 outlined to paths. Inline version uses `currentColor` for the K and wordmark so it flips with the theme. Header 30px, footer 26px.
- **Scatter tile**: a seeded random field (used for the Random Hash Oracle post plate).
- **Signal Bar**: rows of squares whose density decays left→right (or top→bottom on the login plate) — "a proof settling to finality". Reserved for dividers, plates, social cards, loading states.
- **Square wave**: a stepped mint line with white markers at the transitions (WebSockets plate).
- **Dice**: mint pips on lavender tiles (RNG plate).
- **Outlined numerals/words**: big condensed text drawn as paths on a plate (x402 plate, share card).

Share card (1200×630) mimics the mobile hero: header band with the logo in a ruled cell, grid lines, the wrapped headline with the lavender span, the expanded label with its rule, a bottom rule.

Favicon: the Field K mark on ink. Ship `.ico` (16/32/48) + PNG 32/180/192 + SVG, with `?v=` cache-busting — Safari ignores SVG favicons.

Inline logo SVG (drop into a `.logo` link; set `color` on the parent):

```html
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 737.5 145" role="img" aria-label="Kurier"><rect x="0" y="0" width="20" height="20" fill="#5C72FF"/><rect x="25" y="0" width="20" height="20" fill="currentColor"/><rect x="100" y="0" width="20" height="20" fill="currentColor"/><rect x="0" y="25" width="20" height="20" fill="#5C72FF"/><rect x="25" y="25" width="20" height="20" fill="currentColor"/><rect x="75" y="25" width="20" height="20" fill="currentColor"/><rect x="25" y="50" width="20" height="20" fill="currentColor"/><rect x="50" y="50" width="20" height="20" fill="currentColor"/><rect x="75" y="50" width="20" height="20" fill="#5C72FF"/><rect x="100" y="50" width="20" height="20" fill="#5C72FF"/><rect x="25" y="75" width="20" height="20" fill="currentColor"/><rect x="50" y="75" width="20" height="20" fill="currentColor"/><rect x="75" y="75" width="20" height="20" fill="#5C72FF"/><rect x="125" y="75" width="20" height="20" fill="#5C72FF"/><rect x="0" y="100" width="20" height="20" fill="#5C72FF"/><rect x="25" y="100" width="20" height="20" fill="currentColor"/><rect x="50" y="100" width="20" height="20" fill="#5C72FF"/><rect x="75" y="100" width="20" height="20" fill="currentColor"/><rect x="25" y="125" width="20" height="20" fill="currentColor"/><rect x="75" y="125" width="20" height="20" fill="#5C72FF"/><rect x="100" y="125" width="20" height="20" fill="#B5FFA5"/><rect x="125" y="125" width="20" height="20" fill="#5C72FF"/><g fill="currentColor" transform="translate(172.55,145.00) scale(0.21137,-0.21137)"><path transform="translate(0,0)" d="M40 0V688H224V411L324 688H514L391 385L519 0H318L267 220L224 164V0Z"/><path transform="translate(494,0)" d="M254 -12Q187 -12 137.5 10.5Q88 33 62.0 86.5Q36 140 36 231V688H219V221Q219 186 224.5 165.0Q230 144 252 144Q276 144 282.0 165.0Q288 186 288 221V688H472V231Q472 140 445.5 86.5Q419 33 371.0 10.5Q323 -12 254 -12Z"/><path transform="translate(981,0)" d="M40 0V688H292Q369 688 412.0 660.0Q455 632 472.5 584.0Q490 536 490 477Q490 423 479.0 375.5Q468 328 434 293L510 0H321L273 231H224V0ZM224 376H267Q291 376 299.0 401.5Q307 427 307 459Q307 481 303.5 498.5Q300 516 291.5 527.0Q283 538 267 538H224Z"/><path transform="translate(1474,0)" d="M40 0V688H224V0Z"/><path transform="translate(1718,0)" d="M40 0V688H438V530H224V428H402V270H224V158H443V0Z"/><path transform="translate(2163,0)" d="M40 0V688H292Q369 688 412.0 660.0Q455 632 472.5 584.0Q490 536 490 477Q490 423 479.0 375.5Q468 328 434 293L510 0H321L273 231H224V0ZM224 376H267Q291 376 299.0 401.5Q307 427 307 459Q307 481 303.5 498.5Q300 516 291.5 527.0Q283 538 267 538H224Z"/></g></svg>
```

---

## 7. Interaction rules

- Hover = **inversion** (ink ↔ paper) or, on lavender, white fill. Never a colour shift to mint, never a glow, never a lift/shadow.
- Transitions 150ms, only on `background`, `color`, `border-color`, `padding-left`.
- `:focus-visible`: 2px lavender outline, 2px offset (white on lavender surfaces).
- Respect `prefers-reduced-motion` by killing transitions.
- Non-links get no hover state at all (roadmap rows).

---

## 8. Do / Don't

**Do**
- Set every headline in condensed uppercase; let it be huge.
- Divide with rules; align things to them.
- Use mono for anything that is metadata: dates, tags, counters, nav, footer links.
- Keep one accent phrase per headline in lavender.
- Put illustrations on ink plates built from squares.

**Don't**
- Round corners, add shadows, gradients, blur or glow.
- Use mint for hover states or large fills.
- Use lavender for small body text.
- Add icons to things that aren't links.
- Introduce a second display face or a second illustration style.
- Use "on-chain" (it's "onchain").

---

## 9. Re-skin checklist

1. Paste the token block and base rules; set `body` background/color from tokens; add the grid gradient.
2. Load the three fonts; map h1–h3 to Archivo condensed, labels to Archivo expanded, body to Plex Sans, metadata to Plex Mono.
3. Rebuild the header as ruled cells: logo · (switcher if multi-environment) · mono nav · theme toggle · hamburger.
4. Convert each section to: 2px top rule + crop marks + section-head (title left, one outlined button right).
5. Replace cards with ruled rows/cells; replace filled buttons with the outlined button; remove all radii.
6. Pick one lavender band per page for the section you most want noticed (max two).
7. Replace imagery with square-grid plates on ink; reuse the Signal Bar motif for anything decorative.
8. Add the Field K logo, favicons, and an OG card in the mobile-hero layout.
9. Check dark mode: ink ground, paper text, lavender/mint unchanged, plates unchanged.
10. Check 400px width: headline breaks removed, grids collapse to one column, nothing scrolls sideways except tables and code.
