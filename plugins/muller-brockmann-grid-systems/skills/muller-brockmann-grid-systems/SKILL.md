---
name: muller-brockmann-grid-systems
description: Load when building any editorial/magazine/report/longform webpage that must read as rigorously grid-aligned, Swiss/International-Typographic-Style, or "Müller-Brockmann." Triggers: "magazine spread", "grid system", "Swiss design", "editorial layout", "show the grid / grid overlay toggle", "align everything to the grid". Also the reference for the corpus→skill→artifact "Tufte pattern" sizzle move on grid design, and for the recurring failure modes: a grid overlay that's "slapped on top / misaligned" (overlay not sharing the content box), and large display type that looks "off the grid" even when its box is on it (glyph side-bearing → needs optical alignment). Sibling to the Vignelli Canon skill (Vignelli = six-typeface canon + transit wayfinding); use THIS one for Müller-Brockmann modular magazine grids and the verifiable web-grid engineering.
---

# Müller-Brockmann Grid Systems — built real, visible, and verified

Josef Müller-Brockmann (1914–1996), Zurich; *Grid Systems in Graphic Design* (1981) is the corpus. The grid is treated as an ethic, not decoration: **"The grid system is an aid, not a guarantee. It permits a number of possible uses and each designer can look for a solution appropriate to his personal style. But one must learn how to use the grid; it is an art that requires practice."** This skill encodes that discipline AND — the part most attempts get wrong — the front-end engineering to make the grid genuinely load-bearing on the web, plus a harness that PROVES it.

> Two real review notes this skill exists to prevent:
> 1. *"the grid is just slapped on top and misaligned"* → the overlay wasn't in the same content box as the content (see §2.2).
> 2. *"the H in the headline is off the grid"* → the headline's BOX was on the grid but its INK wasn't; large glyphs carry a side-bearing (see §2.6). **Box-on-grid ≠ ink-on-grid.**

---

## PART 1 — THE DISCIPLINE (decide before drawing)
- **Objective order.** The grid brings "constructive thought," legibility, and "objective and functional" design. Restraint is the point; the system, not the ego, organizes the page.
- **Modular grid.** Divide the type area into a field of **modules** — columns AND rows — separated by consistent **gutters**, inside defined **margins**. Text and images occupy whole modules. Müller-Brockmann specimens common field counts (8 / 20 / 32 fields). For the web, a **12-column grid + 8px baseline** is a robust general default; a **6×6 or 4×8 modular field grid** when you want visible rows too.
- **Baseline grid.** Vertical rhythm is sacred: **leading = a whole multiple of the baseline unit**, and every element snaps to it. This is what makes facing columns and images line up across the page.
- **Typography.** A **grotesque sans** (Akzidenz-Grotesk / Helvetica; on the web Inter, Helvetica Now, Archivo). **Flush-left, ragged-right.** Few sizes, large jumps in **scale** for hierarchy; objective, not expressive. Big **numerals/data set large** is a signature move.
- **Palette.** Pure white paper, near-black ink, **one accent — red is canonical**. Avoid the warm-cream "Claude look"; **never blue/purple gradients** (hard house rule).
- **White space + asymmetry.** Generous margins; asymmetric compositions held in tension by the grid.

---

## PART 2 — MAKE THE GRID REAL ON THE WEB (the load-bearing engineering)
`grid_tokens.py` emits this whole scaffold correctly; the rules below are why it's built the way it is.

### 2.1 One source of truth
Put every grid parameter in `:root` CSS variables — `--cols, --gutter, --margin, --bl (baseline), --lh (leading=3×bl), --maxw`. **Content and the overlay both read these same variables.** Never hand-author the overlay separately or it will drift.

### 2.2 The overlay MUST live in the SAME content box as the content  ← #1 bug
Failure mode: content sits in a centered `max-width` container while the overlay is a **full-width sibling** of the section. On any viewport wider than `--maxw`, the centered content and the full-width overlay no longer share column positions → "slapped on top / misaligned."
**Fix:** put `.guides` *inside* the same `.wrap`, and draw the column guides with `left/right = var(--margin)` and the **same** `repeat(var(--cols),1fr)` + `column-gap:var(--gutter)`. Then the overlay columns **are** the content columns at every width. Add left/right margin lines at `var(--margin)`.

### 2.3 Place every element by column LINE via subgrid bands
Don't eyeball spans. Each horizontal **band** spans all columns and re-exposes them:
```css
.band{grid-column:1 / -1; display:grid; grid-template-columns:subgrid; column-gap:var(--gutter); align-items:start;}
@supports not (grid-template-columns:subgrid){ .band{grid-template-columns:repeat(var(--cols),1fr);} }
```
Children place with `grid-column: <startline> / <endline>` (e.g. `1 / 6`, `6 / 13`). Every headline, paragraph, photo, caption now snaps to identical lines.

### 2.4 Lock vertical rhythm to the baseline
- Leading = `--lh` (e.g. 24px = 3×8). **Every line-height a multiple of the baseline, in px (not unitless) for display type** — unitless line-heights on large type push the box off the grid.
- Every margin/padding a multiple of the baseline. Spread top/bottom padding a multiple too, so content starts on a line.
- **Media heights = multiples of the leading** (e.g. 240/360/432/480px) so a photo's top AND bottom both land on lines.
- Hairline rules sit inside a baseline-height band, not free-floating.

### 2.5 The toggle (sizzle within the sizzle)
A control (button **+ `G` key**) toggles `body.grid-on`; overlay fades 0→1. Overlay draws: translucent **numbered column fields**, the **baseline** (major line every `--lh`, faint minor every `--bl`), and **margin lines**. Showing the real grid the page is built on IS the demo.

### 2.6 OPTICAL ALIGNMENT — display ink, not its box  ← the subtle bug
A 180px headline whose layout box is exactly on line 1 still looks misaligned against body text, because the letterform's **ink** is inset by its **left side-bearing**. Cure at runtime:
```js
// after document.fonts.ready and on resize:
var cvs=document.createElement('canvas'),ctx=cvs.getContext('2d');
document.querySelectorAll('.masthead,.numeral,.shead h2,.h2b').forEach(function(el){
  el.style.marginLeft='0px';
  var cs=getComputedStyle(el),ch=(el.textContent||'').trim()[0]; if(!ch) return;
  if(cs.textTransform==='uppercase') ch=ch.toUpperCase();
  ctx.font=cs.fontStyle+' '+cs.fontWeight+' '+cs.fontSize+' '+cs.fontFamily; ctx.textAlign='left';
  var abl=ctx.measureText(ch).actualBoundingBoxLeft;     // +ve = ink overhangs left of box
  if(isFinite(abl)) el.style.marginLeft=abl.toFixed(2)+'px'; // shift box so INK lands on the line
});
```
Apply to the masthead, big numerals, and section headlines. It scales with fluid type (re-runs on resize) and uses the **actually-loaded** font, so it's correct in the user's browser.
**CRITICAL measurement caveat:** side-bearing is **font-specific**. If you measure with the wrong font you get the wrong nudge. Headless/sandbox Chrome usually lacks the webfont, so canvas falls back to a different grotesque (measured **−16px on the fallback vs −7px on real Inter** for the same `H`). To verify optics offline you must **embed the real webfont** via `@font-face` (local TTF). In production the runtime JS measures the loaded font and is correct.

---

## PART 3 — VERIFY (don't trust, measure)  → `verify_grid.js`
Render with headless Chrome (Puppeteer) and assert, at **several widths including > and < `--maxw`** (to catch centered-container drift, e.g. 1440 / 1180 / 900):
1. **Column adherence** — every placed `.band > *` left snaps to a column START and right to a column END (~0px). **Exclude the optically-aligned display elements** from this box check (their box is intentionally side-bearing-offset; they're validated in step 4). **Gotcha:** build BOTH the column-start set and the column-end set — a grid item spanning "to line N" ends at the *far* side of the gutter, so single-edge math falsely reports a one-gutter error.
2. **Overlay match** — each `.guides .col` rect equals the computed column rect (~0px).
3. **Baseline** — text tops modulo the baseline ≈ 0 (tolerance ≈ half a baseline; the box-top is a proxy — the leading does the real work).
4. **Optical ink** — each display element's ink-left (box − `actualBoundingBoxLeft`, real font) equals **its own** column line (nearest column-start to its box), not always line 1.

Sandbox Chrome flags that work: `--headless=new --no-sandbox --disable-gpu --disable-dbus --use-gl=angle --use-angle=swiftshader`. `file://` works for non-ES-module pages; the CLI `--screenshot` can hang on tall pages — drive via Puppeteer and screenshot per viewport. Read PNGs back with the image-capable Read tool to eyeball a **zoom crop of the top-left corner** (masthead vs body vs column line) — the fastest human check.

A clean run looks like: `col=0px overlay=0px baseline≤4px ink=0px` → `GRID VERIFY: PASS`.

---

## PART 4 — CRAFT DEFAULTS (so it looks excellent, not just aligned)
- **Palette:** white `#fff`, ink `#111`, one accent (Swiss red `#e4002b`). No warm-cream Claude look; no blue/purple gradients.
- **Type:** a real grotesque webfont (Inter / Helvetica Now / Archivo) for display + body; a **mono** (Space Mono / IBM Plex Mono) for folios, captions, grid annotations — reinforces the technical register. Non-Latin via Noto Sans JP etc.
- **Hierarchy** through scale + weight + white space, not color. Treat key data as **large numerals**. Kicker labels in mono caps. Per-spread folios.
- **Real photography.** Ground real subjects in real photos. Host images publicly and embed their URLs — a sandboxed iframe can't authenticate thread-scoped API file URLs (broken-image trap).
- **Type fidelity if you ever rasterize art** (cairosvg / headless screenshots / image-gen reference): a `Helvetica`/`Arial` CSS stack silently falls back to **Noto Sans** (reads like Calibri). Render in **Liberation Sans** or an embedded Helvetica/Arimo TTF before trusting it. (Same trap as the optical-measurement caveat: wrong font in → wrong result out.)
- **Spread model:** full-width sections, each its own per-spread `.grid` + `.guides`, consistent margins/folios.

---

## PART 5 — WORKFLOW
1. Pick the subject; gather real photos; host them publicly.
2. Generate the scaffold: `python3 grid_tokens.py` (or `--scaffold` for a full page; `--cols/--baseline/--gutter/--margin/--maxw/--accent` to taste; it warns if gutter/margin aren't baseline multiples).
3. Build spreads as **subgrid bands**; place everything by **column line**; lock spacing/line-heights/media heights to the **baseline**.
4. Add the overlay (same content box) + toggle + optical-alignment JS (already in the scaffold; point its selector list at your display elements).
5. Publish, then **verify**: `CHROME=… PUP=… node verify_grid.js <file-or-url> --widths=1440,1180,900`. Eyeball a top-left zoom crop. Fix, republish.

---

## SCRIPTS

### `grid_tokens.py`
Deterministic scaffold generator. Emits the `:root` tokens, `.grid`/`.band` (subgrid) scaffold, `.guides` overlay CSS, toggle JS, and the optical-alignment JS — all wired to one source of truth. `--scaffold` emits a full minimal HTML page. No network/credentials.

```python
#!/usr/bin/env python3
"""
grid_tokens.py — Müller-Brockmann editorial grid scaffold generator.

Emits a battle-tested, self-contained CSS + JS scaffold for building an
editorial/magazine webpage on a REAL, VISIBLE, VERIFIED modular grid:

  • ONE source of truth: all grid params live in :root CSS variables.
  • The grid-toggle OVERLAY reads the SAME variables and lives in the SAME
    content box as the content, so its columns ARE the content columns
    (this is the fix for the "grid is just slapped on top / misaligned" bug
    that happens when the overlay is a full-width sibling of a centered
    max-width container).
  • Subgrid "bands" so every element is placed by column LINE, not eyeballed.
  • Vertical rhythm locked to an 8px baseline (24px leading).
  • Runtime OPTICAL ALIGNMENT: display type is nudged so its INK (not its box)
    lands on the column line — large letterforms carry a left side-bearing, so
    a headline whose box is on the grid still looks misaligned vs body text.

No network, no credentials. Deterministic.

Usage:
  python3 grid_tokens.py                      # print CSS + JS block
  python3 grid_tokens.py --scaffold           # print a full minimal HTML page
  python3 grid_tokens.py --cols 12 --baseline 8 --gutter 24 --margin 72 \
                         --maxw 1296 --accent "#e4002b"
"""
import argparse, sys

def build(cfg):
    c = cfg
    lh = c.baseline * 3  # leading = 3 baselines
    css = f""":root{{
  --cols:{c.cols};
  --bl:{c.baseline}px;            /* baseline unit */
  --lh:{lh}px;                    /* leading = 3 x baseline */
  --gutter:{c.gutter}px;
  --margin:{c.margin}px;
  --pad:{c.baseline*12}px;        /* spread top/bottom pad (x baseline) */
  --maxw:{c.maxw}px;

  --paper:#ffffff;
  --ink:#111315;
  --ink-soft:#5b6066;
  --accent:{c.accent};

  --g-col:rgba(228,0,43,.075);     /* column field fill   (re-tint to taste) */
  --g-edge:rgba(228,0,43,.40);     /* column edge / margin line */
  --g-base:rgba(0,150,140,.34);    /* major baseline line ({lh}px) */
  --g-base-min:rgba(0,150,140,.12);/* minor baseline line ({c.baseline}px)  */
}}
*{{box-sizing:border-box;}}
body{{margin:0;background:var(--paper);color:var(--ink);
  font-family:"Inter",system-ui,sans-serif;font-size:16px;line-height:var(--lh);
  -webkit-font-smoothing:antialiased;}}
img{{display:block;width:100%;height:100%;object-fit:cover;}}

/* ---- spread + grid scaffold (ONE source of truth) ---- */
.spread{{position:relative;width:100%;}}
.wrap{{position:relative;max-width:var(--maxw);margin:0 auto;padding:var(--pad) var(--margin);}}
.grid{{display:grid;grid-template-columns:repeat(var(--cols),1fr);
  column-gap:var(--gutter);row-gap:var(--lh);}}
/* a band spans all columns and re-exposes them as a subgrid so children
   align to the SAME lines as everything else on the page */
.band{{grid-column:1 / -1;display:grid;grid-template-columns:subgrid;
  column-gap:var(--gutter);row-gap:var(--lh);align-items:start;}}
@supports not (grid-template-columns:subgrid){{
  .band{{grid-template-columns:repeat(var(--cols),1fr);}}
}}
/* place children with: style="grid-column: <startline> / <endline>" */

/* ---- the grid OVERLAY (same content box -> columns match exactly) ---- */
.guides{{position:absolute;inset:0;pointer-events:none;z-index:60;opacity:0;
  transition:opacity .26s ease;}}
body.grid-on .guides{{opacity:1;}}
.guides .cols{{position:absolute;top:0;bottom:0;left:var(--margin);right:var(--margin);
  display:grid;grid-template-columns:repeat(var(--cols),1fr);column-gap:var(--gutter);}}
.guides .col{{background:var(--g-col);
  box-shadow:inset 1px 0 0 var(--g-edge),inset -1px 0 0 var(--g-edge);position:relative;}}
.guides .col span{{position:absolute;top:{c.baseline*4}px;left:0;right:0;text-align:center;
  font-family:"Space Mono",monospace;font-size:10px;line-height:1;color:var(--accent);}}
.guides .rows{{position:absolute;left:var(--margin);right:var(--margin);top:var(--pad);bottom:0;
  background-image:
    repeating-linear-gradient(to bottom,var(--g-base) 0 1px,transparent 1px var(--lh)),
    repeating-linear-gradient(to bottom,var(--g-base-min) 0 1px,transparent 1px var(--bl));}}
.guides .mline{{position:absolute;top:0;bottom:0;width:1px;background:var(--g-edge);}}
.guides .mline.l{{left:var(--margin);}} .guides .mline.r{{right:var(--margin);}}

/* ---- vertical rhythm helpers (keep ALL spacing a multiple of --bl) ----
   line-heights for display type MUST be px multiples of --bl, never unitless,
   or the box height drifts off the baseline. Media heights = multiples of --lh
   so photo top AND bottom land on lines. */
.toggle{{position:fixed;top:18px;right:18px;z-index:200;display:flex;align-items:center;gap:10px;
  background:var(--ink);color:#fff;border:none;cursor:pointer;font-family:"Space Mono",monospace;
  font-size:12px;letter-spacing:.14em;text-transform:uppercase;padding:11px 14px;}}
.toggle .dot{{width:9px;height:9px;border-radius:50%;background:#555;}}
body.grid-on .toggle{{background:var(--accent);}} body.grid-on .toggle .dot{{background:#fff;}}"""

    js = """/* toggle: button + 'G' key */
var btn=document.getElementById('gridToggle');
function setGrid(on){document.body.classList.toggle('grid-on',on);
  if(btn){btn.setAttribute('aria-pressed',on?'true':'false');
    var l=btn.querySelector('.lbl'); if(l) l.textContent=on?'Hide grid':'Show grid';}}
if(btn) btn.addEventListener('click',function(){setGrid(!document.body.classList.contains('grid-on'));});
document.addEventListener('keydown',function(e){
  if((e.key==='g'||e.key==='G')&&!e.metaKey&&!e.ctrlKey&&!e.altKey){
    setGrid(!document.body.classList.contains('grid-on'));}});

/* populate every overlay's column guides (numbered) */
document.querySelectorAll('.guides .cols').forEach(function(h){
  var n=getComputedStyle(document.documentElement).getPropertyValue('--cols').trim()||'12';
  for(var i=1;i<=parseInt(n,10);i++){var c=document.createElement('div');c.className='col';
    var s=document.createElement('span');s.textContent=i;c.appendChild(s);h.appendChild(c);}});

/* ---- OPTICAL ALIGNMENT --------------------------------------------------
   Large display glyphs carry a left side-bearing: the ink sits inside the
   layout box, so a headline whose BOX is on the column line still LOOKS
   indented (or overhangs) vs body text. Measure each display glyph's actual
   ink offset and nudge the element so its visible ink lands on the line.
   Scales with fluid type; re-runs after the webfont loads and on resize.
   Add the selector list to match your display elements. */
(function(){
  var cvs=document.createElement('canvas'),ctx=cvs.getContext('2d');
  var sel='.masthead, .numeral, .shead h2, .h2b';   /* <-- your display selectors */
  function align(){
    document.querySelectorAll(sel).forEach(function(el){
      el.style.marginLeft='0px';
      var cs=getComputedStyle(el),ch=(el.textContent||'').trim().charAt(0); if(!ch) return;
      if(cs.textTransform==='uppercase') ch=ch.toUpperCase();
      ctx.font=cs.fontStyle+' '+cs.fontWeight+' '+cs.fontSize+' '+cs.fontFamily;
      ctx.textAlign='left';
      var abl=ctx.measureText(ch).actualBoundingBoxLeft; /* +ve = ink overhangs left */
      if(isFinite(abl)) el.style.marginLeft=abl.toFixed(2)+'px'; /* ink -> on the line */
    });
  }
  if(document.fonts&&document.fonts.ready){document.fonts.ready.then(align);}
  align();
  var t;window.addEventListener('resize',function(){clearTimeout(t);t=setTimeout(align,120);});
})();"""

    band = """      <!-- a band: children placed by column LINE -->
      <div class="band">
        <div style="grid-column:1 / 6;"><!-- text col --></div>
        <figure style="grid-column:6 / 13;"><!-- image col (height = x --lh) --></figure>
      </div>"""

    overlay = """    <div class="guides" aria-hidden="true">
      <div class="cols"></div><div class="rows"></div>
      <div class="mline l"></div><div class="mline r"></div>
    </div>"""

    if cfg.scaffold:
        return f"""<!DOCTYPE html>
<html lang="en"><head><meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Editorial — modular grid</title>
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
<style>
{css}
</style></head>
<body>
<button class="toggle" id="gridToggle" aria-pressed="false"><span class="dot"></span><span class="lbl">Show grid</span></button>

<section class="spread">
  <div class="wrap">
    <div class="grid">
{band}
    </div>
{overlay}
  </div>
</section>

<script>
{js}
</script>
</body></html>"""
    else:
        return ("/* ===== CSS (paste in <style>) ===== */\n" + css +
                "\n\n/* ===== JS (paste in <script>, after the DOM) ===== */\n" + js +
                "\n\n/* ===== band markup pattern ===== */\n" + band +
                "\n\n/* ===== per-spread overlay markup ===== */\n" + overlay + "\n")

def main():
    ap = argparse.ArgumentParser(description="Müller-Brockmann editorial grid scaffold generator")
    ap.add_argument("--cols", type=int, default=12)
    ap.add_argument("--baseline", type=int, default=8, help="baseline unit in px (leading = 3x)")
    ap.add_argument("--gutter", type=int, default=24)
    ap.add_argument("--margin", type=int, default=72)
    ap.add_argument("--maxw", type=int, default=1296)
    ap.add_argument("--accent", default="#e4002b")
    ap.add_argument("--scaffold", action="store_true", help="emit a full minimal HTML page")
    cfg = ap.parse_args()
    for name, v in (("gutter", cfg.gutter), ("margin", cfg.margin)):
        if v % cfg.baseline != 0:
            print(f"# WARNING: --{name} ({v}) is not a multiple of --baseline ({cfg.baseline}); "
                  f"vertical/spacing rhythm will drift off the grid.", file=sys.stderr)
    sys.stdout.write(build(cfg) + "\n")

if __name__ == "__main__":
    main()
```

### `verify_grid.js`
Puppeteer verification harness. Asserts column adherence (both column edges; excludes optically-aligned display elements), overlay-matches-content, baseline modulo, and optical ink-on-its-own-column-line, at multiple viewport widths (incl. > and < max-width). Prints PASS/FAIL per width. Env: `CHROME` (chrome binary path), `PUP` (puppeteer-core module path).

```javascript
#!/usr/bin/env node
/*
 * verify_grid.js — prove an editorial page actually sits on its grid.
 *
 * Renders the page with headless Chrome (Puppeteer) and asserts, at several
 * viewport widths (including > and < the content max-width, to catch the
 * centered-container drift):
 *
 *   1. COLUMN ADHERENCE  — every placed `.band > *` element's left edge snaps
 *      to a column START line and its right edge to a column END line (~0px).
 *      NB: build BOTH the start-set and end-set of x-coords. A grid item that
 *      spans "to line N" ends at the FAR side of the gutter, so naive single
 *      edge math falsely reports a one-gutter (gutter-px) error.
 *   2. OVERLAY MATCH     — each `.guides .col` rect equals the computed column
 *      rect (~0px), i.e. the overlay really is the content grid.
 *   3. BASELINE          — text element tops, modulo the baseline unit, ~0.
 *   4. OPTICAL INK       — display elements' visible INK-left equals the column
 *      line (measure canvas actualBoundingBoxLeft with the LOADED font).
 *      CAVEAT: side-bearing is font-specific. In a sandbox the webfont is often
 *      absent and canvas falls back to a different grotesque (we measured -16px
 *      fallback vs -7px real Inter). To verify optics offline, EMBED the real
 *      webfont via @font-face(local TTF). In production the page's runtime JS
 *      measures the actually-loaded font, so it is correct for the user.
 *
 * Env / args:
 *   CHROME = path to chrome binary (required)
 *   PUP    = path to puppeteer-core module (required)
 *   arg1   = file:// URL or http URL of the page (default: file://$PWD/index.html)
 *   --widths=1440,1180,900   --baseline=8
 *
 * Sandbox chrome flags that work here:
 *   --no-sandbox --disable-gpu --disable-dbus --use-gl=angle --use-angle=swiftshader
 */
const puppeteer = require(process.env.PUP || 'puppeteer-core');
const path = require('path');

const args = process.argv.slice(2);
const url = (args.find(a => !a.startsWith('--'))) ||
            ('file://' + path.join(process.cwd(), 'index.html'));
const opt = k => { const a = args.find(x => x.startsWith('--' + k + '=')); return a ? a.split('=')[1] : null; };
const widths = (opt('widths') || '1440,1180,900').split(',').map(Number);
const BL = Number(opt('baseline') || 8);

(async () => {
  const browser = await puppeteer.launch({
    executablePath: process.env.CHROME, headless: 'new',
    args: ['--no-sandbox','--disable-gpu','--disable-dbus','--use-gl=angle','--use-angle=swiftshader','--hide-scrollbars']
  });
  const page = await browser.newPage();
  let failed = false;

  for (const W of widths) {
    await page.setViewport({ width: W, height: 1000, deviceScaleFactor: 1 });
    await page.goto(url, { waitUntil: 'load', timeout: 25000 });
    try { await page.evaluate(() => document.fonts && document.fonts.ready); } catch (e) {}
    await new Promise(r => setTimeout(r, 500));

    const res = await page.evaluate((BL) => {
      const OPT = '.masthead, .numeral, .shead h2, .h2b'; // display elements: optically aligned by INK, not box
      const grid = document.querySelector('.grid');
      const cs = getComputedStyle(grid);
      const tracks = cs.gridTemplateColumns.split(' ').map(parseFloat);
      const gap = parseFloat(cs.columnGap);
      const gr = grid.getBoundingClientRect();
      // build column START (L) and END (R) coordinate sets
      const L = [], R = []; let x = gr.left;
      for (let i = 0; i < tracks.length; i++) { L.push(x); x += tracks[i]; R.push(x); if (i < tracks.length - 1) x += gap; }
      const nr = (v, arr) => arr.reduce((m, e) => Math.min(m, Math.abs(e - v)), 1e9);
      const nearest = (v, arr) => arr.reduce((b, e) => Math.abs(e - v) < Math.abs(b - v) ? e : b, arr[0]);

      // 1. column adherence — exclude optical display elements (their box is
      //    deliberately offset by the glyph side-bearing so the INK lands on
      //    the line; they are validated by check 4 instead).
      let colErr = 0, worst = null;
      document.querySelectorAll('.band > *').forEach(el => {
        if (el.matches(OPT)) return;
        const r = el.getBoundingClientRect(); if (r.width < 2) return;
        const e = Math.max(nr(r.left, L), nr(r.right, R));
        if (e > colErr) { colErr = e; worst = (el.className || el.tagName).toString().slice(0, 28); }
      });

      // 2. overlay match
      let ovErr = 0;
      document.querySelectorAll('.guides .cols .col').forEach((c, i) => {
        const r = c.getBoundingClientRect();
        if (L[i] != null) ovErr = Math.max(ovErr, Math.abs(r.left - L[i]));
        if (R[i] != null) ovErr = Math.max(ovErr, Math.abs(r.right - R[i]));
      });

      // 3. baseline (tops modulo BL, per spread relative to its rows-top)
      let baseErr = 0;
      document.querySelectorAll('.spread').forEach(sp => {
        const rowsEl = sp.querySelector('.guides .rows'); if (!rowsEl) return;
        const top = rowsEl.getBoundingClientRect().top;
        sp.querySelectorAll('.body,.lede,.cap,.toc li,.dishes li,.kicker').forEach(el => {
          const t = el.getBoundingClientRect().top - top; const m = ((t % BL) + BL) % BL;
          baseErr = Math.max(baseErr, Math.min(m, BL - m));
        });
      });

      // 4. optical ink offset — each display element's visible INK-left must
      //    sit on ITS OWN column line (the nearest column-start to its box),
      //    not always line 1 (headlines can start on any column).
      const cvs = document.createElement('canvas'), ctx = cvs.getContext('2d');
      let inkErr = 0, inkWorst = null;
      document.querySelectorAll(OPT).forEach(el => {
        const c = getComputedStyle(el); let ch = (el.textContent || '').trim().charAt(0); if (!ch) return;
        if (c.textTransform === 'uppercase') ch = ch.toUpperCase();
        ctx.font = c.fontStyle + ' ' + c.fontWeight + ' ' + c.fontSize + ' ' + c.fontFamily; ctx.textAlign = 'left';
        const abl = ctx.measureText(ch).actualBoundingBoxLeft;
        const box = el.getBoundingClientRect().left;
        const target = nearest(box, L);          // the column line this element sits on
        const ink = box - abl;                    // visible ink-left
        const e = Math.abs(ink - target);
        if (e > inkErr) { inkErr = e; inkWorst = (el.className || '').toString().slice(0, 20) + ' "' + ch + '"'; }
      });

      return {
        track: +tracks[0].toFixed(1),
        maxColErrPx: +colErr.toFixed(2), worstCol: worst,
        overlayErrPx: +ovErr.toFixed(2),
        maxBaselineOffPx: +baseErr.toFixed(2),
        maxInkOffPx: +inkErr.toFixed(2), worstInk: inkWorst,
        fontFamily: getComputedStyle(document.querySelector('.masthead') || document.body).fontFamily.split(',')[0]
      };
    }, BL);

    // baseline tolerance = half a baseline unit
    const pass = res.maxColErrPx <= 0.5 && res.overlayErrPx <= 0.5 && res.maxBaselineOffPx <= (BL / 2) && res.maxInkOffPx <= 1.0;
    if (!pass) failed = true;
    console.log(`[${pass ? 'PASS' : 'FAIL'}] vw=${W}  col=${res.maxColErrPx}px overlay=${res.overlayErrPx}px ` +
                `baseline=${res.maxBaselineOffPx}px ink=${res.maxInkOffPx}px  ` +
                `(worstCol=${res.worstCol}, worstInk=${res.worstInk}, font=${res.fontFamily})`);
  }
  await browser.close();
  if (failed) { console.error('GRID VERIFY: FAIL'); process.exit(1); }
  console.log('GRID VERIFY: PASS');
})().catch(e => { console.error('ERR', e.message); process.exit(2); });
```

## CREED
A grid you can't toggle on and measure is a mood board, not a system. Build it from one source of truth, prove it at 0px, and align the **ink**.
