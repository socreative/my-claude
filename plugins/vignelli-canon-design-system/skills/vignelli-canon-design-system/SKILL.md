---
name: vignelli-canon-design-system
description: Load when designing or critiquing any visual artifact that should read as disciplined, timeless, Swiss-modernist, or "Vignelli-style" — brand/identity systems, design systems and style guides, editorial/book/poster layouts, and especially transit/wayfinding signage and route diagrams. Also load for the corpus→skill→artifact "Tufte pattern" sizzle move, for any code→image→reality pipeline where rendered TYPE must stay true to a grotesque (avoid the Calibri/Noto fallback trap), or whenever a brief asks for grid-driven, Helvetica-led, primary-color, intellectually-elegant design.
---

# The Vignelli Canon — a working design discipline

Massimo Vignelli (1931–2014), Italian designer mentored by the Castiglioni brothers and Mies van der Rohe ("God is in the details"). With his wife and partner Lella, he designed "from the spoon to the city": the **1972 New York City Subway diagram and signage standards**, the **American Airlines** identity (used ~45 years), **Knoll**, **Bloomingdale's**, **Heller** dinnerware, the **National Park Service Unigrid** system, and the **Grandi Stazioni** Italian railway station signage. His book *The Vignelli Canon* is his own statement of method. This skill is that method, made applyable.

His thesis: **Design is one discipline, above any style. Creativity needs the support of knowledge.** The rules below are a bag of tools, not a cage.

---

## PART ONE — THE INTANGIBLES (decide these before you draw anything)

1. **Semantics — search for the meaning first.** Research the subject, its history, market, sender and receiver; distill the *essence*. "Design without semantics is shallow."
2. **Syntactics — control the relationships.** The grid, typefaces, headline/text/image relationships, page to page. "God is in the details."
3. **Pragmatics — it must be understood.** It should stand by itself with no explanation. Clarity of intent → clarity of result. "We love complexity but hate complications."
4. **Discipline.** No sloppiness. "Quality is there or it is not." Self-imposed rules give continuity of intent.
5. **Appropriateness.** "Listen to what a thing wants to be." The search for the *specific* of the problem — the right media, material, scale, color.
6. **Ambiguity (the good kind).** A plurality of meanings, used as a spice.
7. **Design Is One.** Master one discipline and you can design anything; style is downstream of discipline.
8. **Visual Power.** Strength through contrast of **scale** (huge head vs. small body) and **weight**, never through loudness.
9. **Intellectual Elegance.** Elegance of the *mind*, not of manners — the opposite of vulgarity.
10. **Timelessness.** Prefer **primary shapes and primary colors** and typography beyond trends. "Clear, simple and enduring."
11. **Responsibility.** Three levels: to self, to client (economy of means), to the public.
12. **Equity.** A long-lived mark is collective culture — **refine it; don't replace it for the sake of change.**

---

## PART TWO — THE TANGIBLES (the concrete rules)

### The Grid — "organization of information"
- The basic structure: organizes content, gives consistency, projects intellectual elegance.
- "Infinite grids, but just one — the most appropriate — for any problem." Too fine = an empty page; too coarse = restrictive.
- **Tight outside margins** create tension; **wider margins** bring serenity. **Tight gutters** (~one line of type) so type and images snap to the same grid.
- The five grids he specimens: **2×4, 5×4, 3×6, 6×6, 4×8** (columns × modules).
- Paper: prefer the **DIN A series** (golden-rectangle proportions).

### Typography
- **The six basic typefaces for a whole career:** Garamond (1532), Bodoni (1788), Century Expanded (1900), Futura (1930), Times (1931), **Helvetica (1957)**. Plus Optima, Univers, Caslon, Baskerville. *"It is not the type but what you do with it that counts."* Helvetica is the house face.
- **Type as objective organization, not self-expression.** "I don't believe that when you write *dog* the type should bark." Differentiate with **space, weight, alignment** — not novelty faces.
- **Alignment: flush left by default.** Centered only for lapidary text/invitations/addresses. **Justified is "fundamentally contrived" — avoid.**
- **Two type sizes per page, maximum;** heading ≈ **2× body** (e.g., 10/20). Bold/light/roman/italic sparingly and functionally.
- **Size/leading by column width:** 8/9, 9/10, 10/11 ≤70mm; 12/13, 14/16 ≤140mm; 16/18, 18/20 larger.
- **Rulers:** **2pt** major, **0.5–1pt** minor; **type hangs from the ruler.**

### Scale, Texture, Color
- **Scale:** the most appropriate size in context — can be pushed deliberately for power. "It doesn't allow mistakes."
- **Texture:** "Light is the master of form and texture." Shiny reflects, matte absorbs.
- **Color as Signifier / Identifier ("Chromotype"), not pictorial.** Default to the **primary palette: Red, Blue, Yellow.** In identity, color IS part of the identity.

### White Space — the protagonist
- *"It is really the white that makes the black sing."* *"In a world where everybody screams, silence is noticeable."* Don't fill the page.

### Layout, Sequence, Identity/Diversity, Economy
- A publication is a **cinematic object** — design the *sequence*. **"If you see the layout, it is probably a bad layout."**
- Balance **identity vs. diversity** — strong system, room to play. Simple modular ratios (single→double→triple). Standardization is an ethic; "good design doesn't cost more than bad design."

---

## TRANSIT & WAYFINDING (Subway + Grandi Stazioni)

- **Route / line diagram:** **45° and 90° only**; **evenly spaced station dots** regardless of true geography; each line a **single color**; **solid dot = always stops, hollow ring = sometimes/passes**; trunk lines share a color; land/water as flat neutral fields; Helvetica throughout.
- **Station signage (Grandi Stazioni):** **white Helvetica on a signal-blue panel**, flush left; a hierarchy by cap height — station identification (largest, internally lit), directional (overhead, arrow sharing the cap box), information/regulatory (smaller, hanging from a 2pt rule); **platform numbers in a square flag**, double-sided. Arrows and pictograms share the type's cap box.
- **Pictograms:** geometric, primary-shape based, consistent stroke and cap box; meaning over illustration.

---

## HOW TO APPLY (workflow)
1. **Intangibles pass:** one line each for semantics, appropriateness, and the timelessness/equity stance.
2. **Pick the system:** one grid, **Helvetica**, **two sizes** (body + ~2× heading), one **primary identifier color**.
3. **Generate tokens** with `vignelli_system.py`.
4. **Compose** flush-left on the grid; let **white space** carry hierarchy; **rulers** + weight for distinction; **scale contrast** for power.
5. **Self-critique:** more than two sizes? justified text? decorative color? visible/cluttered layout? novelty face? Cut it.

### Helper script `vignelli_system.py`
Deterministic, no network/credentials. `python3 vignelli_system.py` → CSS tokens (palette, two-size scale, the five grids, ruler weights). `--primary "#RRGGBB"`, `--base 16`, `--format css|scss|json`, `--grid 4x8`, `--signage` (railway panel/cap-height table). The CSS `--v-face` lists **"Liberation Sans"** before the generic fallback so headless renders keep a real grotesque (see Production notes).

### Canon palette (color as identifier)
Vignelli vermilion `#F04E23` · Signal blue `#0039A6` · Signal yellow `#FFCC00` · Ink `#0A0A0A` · Warm paper `#F4F1EA` · White `#FFFFFF`.

---

## PRODUCTION — bringing the system into images & the real world
Hard-won from shipping the VECTOR Northeast-rail sizzle.

### Type fidelity when rasterizing (the #1 trap)
Helvetica is **not installed** in most headless environments. Rasterizing SVG/HTML (cairosvg, headless-Chromium screenshots) with a `Helvetica`/`Arial`/`sans-serif` stack silently falls back to **Noto Sans** — a rounded humanist face that reads like **Calibri** and breaks the grotesque. The failure is invisible until someone asks "why does this look like Calibri?"
- **Fix:** render in a true Helvetica/Arial-metric grotesque — **Liberation Sans** (usually installed; verify `fc-match "Liberation Sans:bold"`) or an embedded **Helvetica/Arimo** TTF in `~/.fonts` + `fc-cache`.
- `fc-match Helvetica` reveals the fallback. **Always eyeball one render before trusting it.**

### The code → image → (optional) reality pipeline
1. **Draw the type/diagram in code first**, in the correct grotesque — the source of truth.
2. **Place it into the world with an image model** (best legible-text rendering). Pass the crisp artwork as a reference image AND in the prompt **name the face and forbid the drift**: *"Helvetica Bold, Swiss neo-grotesque (Arial Bold / Neue Haas Grotesk); reproduce the artwork letter-for-letter as an applied/printed graphic; NOT Calibri, NOT Noto Sans, NOT a rounded humanist sans."*
3. If the reference font is wrong (step 1 trap), the model faithfully reproduces the wrong font — **fix the reference before blaming the prompt.**

### Wayfinding-in-context shot vocabulary
Type on the train flank (wordmark + destination blind); the route **diagram on the wall** (backlit, traveler in silhouette, golden hour); the **paper map in hand** by a window; **overhead platform directional** (white Helvetica on signal blue, drawn arrows); **station-ID + square platform flag**; **concourse pictogram totem**. Pair each crisp coded design beside its render — "design in code → reality" is the proof.

### Avoid video gen for type-critical heroes
Most video models **drift letterforms frame-to-frame** — fatal for a sign you must read. When type is the hero, deliver **stills** or **screen-capture an interactive webpage**, not generated video.

### Packaging
- Build **visual-first**: cinematic in-situ heroes lead; the system reference (principles, palette, type scale, coded diagram) provides depth below.
- **Embedding gotcha:** a sandboxed iframe artifact can't authenticate thread-scoped API file URLs. Host images publicly and use their share URLs in the HTML.

---

## SCRIPT

### `vignelli_system.py`
Deterministic Vignelli-Canon token generator: CSS/SCSS/JSON tokens (primary palette, two-size type scale with 2× heading, the five grids, ruler weights), plus `--grid` and `--signage` modes. CSS `--v-face` lists `Liberation Sans` before the generic fallback so headless renders don't drift to Noto/Calibri. No network or credentials.

```python
#!/usr/bin/env python3
"""
vignelli_system.py — Generate a Vignelli-Canon-compliant design-token sheet.

Massimo Vignelli's discipline reduced to machine-emittable tokens: a primary
palette used as *identifier* (not decoration), a two-size type scale where the
heading is ~2x the body, the five canonical grids, ruler weights, and the
signage panel module logic from the Grandi Stazioni railway program.

This is a deterministic generator — no network, no credentials. It exists so the
"discipline" of the Canon can be applied consistently across artifacts (web
pages, signage specs, design-system docs) instead of being re-derived by hand
each time.

Usage:
  python3 vignelli_system.py                      # default: Helvetica, Vignelli vermilion
  python3 vignelli_system.py --primary "#F04E23"  # set the identifier color
  python3 vignelli_system.py --base 16 --format css
  python3 vignelli_system.py --format json
  python3 vignelli_system.py --grid 4x8           # print one grid's column/module map
  python3 vignelli_system.py --signage            # print railway signage panel module table

Formats: css (default) | json | scss

RENDERING NOTE (hard-won): Helvetica is the house face, but it is NOT installed in
most headless environments. If you rasterize SVG/HTML (cairosvg, headless Chromium)
with a `Helvetica`/`Arial`/`sans-serif` stack, the renderer silently falls back to
Noto Sans — a rounded humanist face that reads like Calibri and breaks the Vignelli
grotesque. For any artwork you will rasterize (or feed to an image model), render in
a true Helvetica/Arial-metric grotesque: `Liberation Sans` (usually installed) or an
embedded Helvetica/Arimo TTF. The CSS emitted here lists "Liberation Sans" before the
generic fallback for exactly this reason. Always eyeball one render before trusting it.
"""

# Rasterization-safe Helvetica substitute (Arial/Helvetica-metric grotesque).
RASTER_FONT = "Liberation Sans"
import argparse, json, sys

# --- The Canon's fixed constants --------------------------------------------

# The six basic typefaces Vignelli said you can live a whole career on.
BASIC_TYPEFACES = [
    ("Garamond", 1532, "serif"),
    ("Bodoni", 1788, "serif"),
    ("Century Expanded", 1900, "serif"),
    ("Futura", 1930, "sans-serif"),
    ("Times", 1931, "serif"),
    ("Helvetica", 1957, "sans-serif"),
]
# plus, by his own admission: Optima, Univers, Caslon, Baskerville.

# Primary palette — color as Signifier / Identifier ("Chromotype"), not pictorial.
# Vignelli vermilion is the Canon cover; signal blue is the NYC subway / station blue.
PALETTE = {
    "vermilion": "#F04E23",   # the Canon cover red — primary identifier
    "blue":      "#0039A6",   # NYC subway / Grandi Stazioni signage blue
    "yellow":    "#FFCC00",   # signal yellow
    "ink":       "#0A0A0A",   # near-black text
    "paper":     "#F4F1EA",   # warm paper
    "white":     "#FFFFFF",
    "rule":      "#0A0A0A",
}

# The five grids Vignelli specimens in the Canon: (columns, modules).
GRIDS = {
    "2x4": (2, 4),
    "5x4": (5, 4),
    "3x6": (3, 6),
    "6x6": (6, 6),
    "4x8": (4, 8),
}

# Ruler weights — type ALWAYS hangs from the ruler.
RULERS = {"major_pt": 2.0, "minor_pt": 1.0, "hair_pt": 0.5}

# Column-width -> size/leading pairs (pt on pt), straight from the Canon.
TYPE_BY_COLUMN = [
    ("<=70mm",  [(8, 9), (9, 10), (10, 11)]),
    ("<=140mm", [(12, 13), (14, 16)]),
    (">140mm",  [(16, 18), (18, 20)]),
]


def type_scale(base_px: float) -> dict:
    """Two living sizes per page, heading ~2x body. Everything else is a weight."""
    body = base_px
    return {
        "caption": round(body * 0.75, 2),   # the small print / specs
        "body":    round(body, 2),           # one of the two sizes
        "lead":    round(body * 1.5, 2),     # standfirst / intro (use sparingly)
        "heading": round(body * 2.0, 2),     # the OTHER size — 2x the body
        "display": round(body * 4.0, 2),     # scale-as-visual-power (covers, hero)
        "mega":    round(body * 8.0, 2),     # "Books"-spread scale contrast
        "leading_body": round(body * 1.2, 2),
        "leading_tight": round(body * 1.05, 2),
    }


def emit_css(primary: str, base: float) -> str:
    ts = type_scale(base)
    lines = [":root {",
             "  /* Vignelli Canon tokens — color as identifier, two sizes, hard grid */"]
    pal = dict(PALETTE); pal["vermilion"] = primary
    for k, v in pal.items():
        lines.append(f"  --v-{k}: {v};")
    lines.append(f"  --v-primary: {primary};")
    lines.append("")
    for k, v in ts.items():
        unit = "px"
        lines.append(f"  --v-{k.replace('_','-')}: {v}{unit};")
    lines.append("")
    for name, pt in RULERS.items():
        lines.append(f"  --v-rule-{name.split('_')[0]}: {pt}px;")
    lines.append("  --v-gutter: 1rem;            /* gutters tight — ~one line of type */")
    lines.append("  --v-margin: clamp(16px, 4vw, 64px);")
    # 'Liberation Sans' before the generic fallback so headless renderers don't drift to Noto/Calibri
    lines.append("  --v-face: 'Helvetica Neue', Helvetica, Arial, 'Liberation Sans', sans-serif;")
    lines.append("}")
    lines.append("")
    lines.append("/* Type hangs FROM the ruler */")
    lines.append(".v-rule { border-top: var(--v-rule-major) solid var(--v-ink); }")
    lines.append(".v-rule--minor { border-top: var(--v-rule-minor) solid var(--v-ink); }")
    lines.append(".v-flush-left { text-align: left; } /* the default; never justify */")
    lines.append("")
    for name, (cols, mods) in GRIDS.items():
        lines.append(f".v-grid-{name} {{ display:grid; "
                     f"grid-template-columns: repeat({cols}, 1fr); "
                     f"grid-template-rows: repeat({mods}, 1fr); gap: var(--v-gutter); }}")
    return "\n".join(lines)


def emit_scss(primary: str, base: float) -> str:
    css = emit_css(primary, base)
    return "// Vignelli Canon tokens (SCSS map)\n" + css.replace("--v-", "$v-").replace(":root {", "")


def emit_json(primary: str, base: float) -> str:
    pal = dict(PALETTE); pal["vermilion"] = primary; pal["primary"] = primary
    payload = {
        "palette": pal,
        "typefaces_basic": [{"name": n, "year": y, "kind": k} for n, y, k in BASIC_TYPEFACES],
        "house_face": "Helvetica",
        "type_scale_px": type_scale(base),
        "type_by_column": {k: v for k, v in TYPE_BY_COLUMN},
        "grids": {k: {"columns": c, "modules": m} for k, (c, m) in GRIDS.items()},
        "rulers_pt": RULERS,
        "rules": [
            "Search the meaning before the form (semantics first).",
            "Max two type sizes per page; heading is ~2x the body.",
            "Flush left, never justified.",
            "Color is an identifier, not decoration — primary palette.",
            "White space makes the black sing — protect the silence.",
            "If you can see the layout, it is a bad layout.",
            "Refine equity, don't replace it.",
            "When rasterizing or feeding art to image models, render Helvetica via Liberation Sans / an embedded TTF — never the bare sans-serif fallback (it becomes Noto/Calibri).",
        ],
    }
    return json.dumps(payload, indent=2)


def print_grid(name: str):
    if name not in GRIDS:
        print(f"Unknown grid '{name}'. Options: {', '.join(GRIDS)}"); return
    cols, mods = GRIDS[name]
    print(f"{name} grid — {cols} columns x {mods} modules")
    for r in range(mods):
        print("  " + "  ".join("[]" for _ in range(cols)))


def print_signage():
    """Railway station signage panel module table (Grandi Stazioni logic).
    Cap-heights scale by a fixed module; arrows and pictograms share the cap box."""
    print("Railway signage — panel hierarchy (white Helvetica on signal blue)")
    print(f"  Identifier color: {PALETTE['blue']}   Text/Pictograms: {PALETTE['white']}")
    rows = [
        ("Station identification (building)", 600, 100, "Cap height 100mm, internally lit"),
        ("Directional (overhead)",            300, 75,  "Arrow shares the cap box; flush left"),
        ("Information / regulatory",          150, 25,  "Hangs from a 2pt rule"),
        ("Platform number (flag)",            250, 250, "Numeral in a square, double-sided"),
    ]
    print(f"  {'Panel':36} {'Panel mm':>9} {'Cap mm':>7}  Note")
    for name, panel, cap, note in rows:
        print(f"  {name:36} {panel:>9} {cap:>7}  {note}")


def main():
    ap = argparse.ArgumentParser(description="Generate Vignelli-Canon design tokens.")
    ap.add_argument("--primary", default=PALETTE["vermilion"], help="identifier color hex")
    ap.add_argument("--base", type=float, default=16.0, help="base body size in px")
    ap.add_argument("--format", choices=["css", "scss", "json"], default="css")
    ap.add_argument("--grid", help="print one grid map, e.g. 4x8")
    ap.add_argument("--signage", action="store_true", help="print railway signage module table")
    a = ap.parse_args()

    if a.grid:
        print_grid(a.grid); return
    if a.signage:
        print_signage(); return
    if a.format == "css":
        print(emit_css(a.primary, a.base))
    elif a.format == "scss":
        print(emit_scss(a.primary, a.base))
    else:
        print(emit_json(a.primary, a.base))


if __name__ == "__main__":
    main()
```

## CREED
*"I love systems and despise happenstance."*
