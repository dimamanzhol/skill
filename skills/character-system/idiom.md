# Idiom

Material answers *what is this thing made of*. Idiom answers *what kind of picture is this*, and it is the decision underneath material rather than beside it — a felt character and a photograph of a felt character are not the same deliverable, and only one of them can be animated in CSS.

It is lock 0 because it is the only lock that constrains the others. Form, face, motion and code all have fewer legal answers once the idiom is chosen, which is exactly why choosing it first saves the work. Skip it and you have not avoided the decision, you have defaulted it: the default is a studio photograph of a small soft object, and that default is most of why competent character work comes out looking like every other piece of competent character work.

**The tell that it was never chosen:** every image prompt in the deliverable opens with the same four words. If all of them say *photograph of*, nobody picked an idiom.

## Choosing

Pick on production reality first and taste second. Four of the six idioms can be drawn directly in SVG and animated in code; two cannot, and no amount of wanting will make them cheap.

| Condition | Take | Avoid |
| --- | --- | --- |
| Must animate in code, no rendered assets | Flat graphic, drawn line, typographic, pixel grid | Rendered object, photographic |
| Must live at 16–24px | Pixel grid, flat graphic, typographic | Rendered object, photographic, fine-line |
| Must carry a sense of physical presence | Rendered object, photographic | Flat graphic, typographic |
| Must read as made by a person | Drawn line, photographic collage | Rendered object, pixel grid |
| Must sit inside dense UI without competing | Flat graphic, typographic, drawn line | Rendered object, photographic |
| Must survive being resized by someone else, forever | Flat graphic, drawn line, typographic | Pixel grid, photographic |
| Brand already has a strong typeface | Typographic | — |
| One-off marketing moment, no interface life | Photographic, rendered object | Pixel grid |

Then check what the idiom costs to keep alive. **The question is not whether you can make one picture, it is who makes the ninth one in eighteen months.** Flat graphic, drawn line, typographic and pixel grid are all reproducible from the bible by anyone with a vector tool. Rendered object and photographic require either the original generation setup or the original photographer, and the cast will drift the moment either is unavailable.

---

## The six idioms

### Rendered object

Light is simulated falling on a volume. The character is a thing that exists in space, and the picture is a record of it.

- **Says:** this is a real object, manufactured, holdable. Carries warmth and physical presence better than anything else here.
- **Produced by:** image generation, 3D render, or photography of a real maquette. Not by hand in a vector tool.
- **Size floor:** ~40px, and lower only when the material is gloss or enamel, whose read is one bright mark.
- **Face:** the minimal-mark vocabulary is at its strongest here. Marks sit *on* a surface, so cut-out eyes and appliqué eyes both work and both read as construction rather than anatomy.
- **Motion goes into:** squash and stretch of a baked asset, or a transform on a flat cut-out of the render. Per-state renders drift; generate state sheets as one grid image.
- **Prompt opener:** `Studio photograph of…`, `Macro photograph of…`, `Product photography of…`
- **Fails when:** the interface needs a dozen variants nobody budgeted to render, or a developer needs to recolour it at runtime.

### Flat graphic

Shapes with no light model. No highlight, no gradient, no ambient occlusion, no contact shading. Depth, where it exists at all, comes from overlap and from value, never from a simulated light source.

This is the idiom [material.md](material.md) is pointing at when it recommends **flat riso** for serious domains, and the one its opening line about flat vector is most likely to be misread as forbidding. The warning there is against *unchosen* flatness — a vector fill arrived at because nobody picked a material. Flat as a committed idiom is a different thing and a strong one: it is Olle Eksell, it is airline safety cards, it is most of the best print mascots of the last seventy years.

- **Says:** designed, printed, deliberate. Reads as graphic design rather than as illustration, which is often the more serious register.
- **Produced by:** vector tool, or written directly as SVG. The cheapest idiom to own.
- **Size floor:** ~16px, the lowest of the six apart from pixel grid.
- **Face:** solid marks only, and the ban on lids and pupils is at its most justified here — there is no surface for a highlight to sit on, so a highlight reads as a hole.
- **Motion goes into:** anything. Pure transforms, colour swaps, shape cuts. This is the only idiom with no motion tax at all.
- **Prompt opener:** `Flat two-colour screen print of…`, `Risograph print of…`, `Flat vector illustration, no gradients, no shading, of…`
- **Fails when:** the form was never good enough. Flat removes every rescue — no light to flatter a weak silhouette, no texture to add interest. **If a character looks dull flat, the problem is the form primitive, not the idiom.**

### Drawn line

The character is a stroke. Identity lives in the line's weight, its pressure variation, and where it opens or closes, rather than in a filled shape.

- **Says:** made by a hand, in one pass, unrepeatable. The only idiom that reads as a gesture rather than an object.
- **Produced by:** drawn and vectorised, or generated as a variable-width outline from a path. Animatable in SVG either way.
- **Size floor:** ~32px, and it depends entirely on stroke weight. A 1px monoline dies by 24px; a heavy brush stroke survives to 20px.
- **Face:** the one idiom where the minimal-mark rule can relax. A brow is one short stroke here, in the same language as the body, which is not true anywhere else — it costs nothing and it does not promise anatomy. Lids and pupils stay out.
- **Motion goes into:** redrawing the stroke. `stroke-dasharray` for draw-on, width modulation along the path for pressure, and the line's own tremble for error. Never squash — a stroke that scales non-uniformly reveals itself as a shape in a costume.
- **Prompt opener:** `Single continuous brush-ink drawing of…`, `Monoline pen drawing of…`, `Sumi-e ink study of…`
- **Fails when:** the cast needs more than about four members. Lines do not differentiate by colour the way filled shapes do, and a stroke-only cast reads as one character in five moods.

### Typographic

The character is built from letterforms, punctuation, or the vocabulary of type — a counter, a terminal, a bowl, a bracket. Not a character *wearing* a letter, and not a logotype with eyes bolted on: a character whose construction is typographic.

**Do not confuse this with lock 1's "typographic gesture".** There, type is a *source* for a shape — you borrow the swell of a brush terminal and then render it in glazed ceramic, and the result is a rendered object. Here, type is the *construction*, and the character stays made of type all the way down. A letterform rendered as a physical object belongs to the rendered idiom and should be prompted as one.

- **Says:** this product is about language. Also the most unexpected idiom in the genre, and the easiest one to make feel like nobody else's.
- **Produced by:** type, in a vector tool. If the brand already has a face with character in it, half the design is done and already consistent with everything else you ship.
- **Size floor:** ~20px, which is the type's own floor.
- **Face:** often no face at all, and this is the idiom where that lands best — a counter already reads as an eye. When there is a face, the marks must be drawn in the type's own logic: the eye is a punched counter, not a circle placed on top.
- **Motion goes into:** variable font axes, if the face has them, which is the cleanest motion in this whole document — one `font-variation-settings` interpolation and the whole form responds coherently. Otherwise glyph swaps, which must cut rather than morph.
- **Prompt opener:** mostly not a prompt job. This idiom is drawn, not generated; image models handle letterforms badly and will hand you something misspelled and almost right. Build it in a vector tool.
- **Fails when:** the product is not about language at all, in which case the connection is decorative and the viewer will feel it.

### Pixel grid

The form is quantised to discrete cells and the aliasing is the point. Not a smooth character displayed small — a character whose construction unit is the cell.

- **Says:** computational, honest about being software, a little nostalgic. The one idiom that openly admits the character lives on a screen.
- **Produced by:** drawn cell by cell. Trivially animatable as CSS grid, canvas, or a sprite sheet.
- **Size floor:** none, in the usual sense. It has a size *ceiling* instead: scale it past a few hundred pixels and it becomes a chart of squares. Scale only by whole multiples, and never with smoothing.
- **Face:** two cells and a gap. The most constrained face vocabulary available, and the constraint does all the work — there is no room to add a nose even if someone wants one.
- **Motion goes into:** cell state changes, on a slow discrete clock. **Never sub-pixel transforms** — a pixel character that slides smoothly or rotates by 7° stops being a pixel character and becomes a blurred photograph of one. Snap to the grid, every frame.
- **Prompt opener:** generated badly by image models at the time of writing; they produce soft approximations of pixel art rather than pixel art. Draw it.
- **Fails when:** the brand is premium or the domain is grave. The idiom carries a strong register and it will not put it down.

### Photographic / collage

Real matter — photographed, scanned, cut, assembled. Torn paper, pressed fabric, found objects arranged on a surface.

- **Says:** made, physically, by someone, out of real things. The least reproducible register and the most distinctive because of it.
- **Produced by:** a camera and a scanner. The most expensive idiom to run and the one most likely to be quietly abandoned a year in.
- **Size floor:** ~64px. Texture is the entire proposition and it needs pixels.
- **Face:** appliqué — a face made of the same matter as the body, physically placed. Drawn marks on photographed material look pasted, every time.
- **Motion goes into:** stop-motion steps, or rotation at cut-out joints. Hold frames on a slow clock; smooth interpolation fights the material's evidence of being handled.
- **Prompt opener:** `Overhead photograph of a torn paper collage character on…`, `Scanned construction-paper cut-out of…`
- **Fails when:** the interface needs states nobody will re-shoot. Lock the smallest state set the product can live with, or pick another idiom.

---

## What the idiom changes downstream

Each row is a rule stated elsewhere in this skill as though it were universal. It is not — it is true of the rendered-object default.

| Rule, as stated elsewhere | Still true in | Changes in |
| --- | --- | --- |
| No eyebrows, ever (lock 3) | Rendered, flat, pixel, photographic | **Drawn line** — a brow is one stroke in the body's own language |
| No face at all is legitimate (lock 3) | All | **Typographic** — it is the *preferred* answer, not a fallback |
| Squash and stretch, volume-preserving (lock 6) | Rendered, flat, photographic | **Drawn line** — redraw the stroke. **Pixel** — snap, never squash |
| Error is a silhouette change, not colour (lock 4) | All, and it is the one rule that survives every idiom intact | — |
| The 24px test | All | **Pixel** — it is already solved, and a size *ceiling* applies instead |
| Generate state sheets as one grid image (stage 2) | Rendered, flat, photographic | **Typographic, pixel** — not generated at all; drawn |
| Faces cut, never morph (code.md) | All | — |

## Which materials are reachable from which idiom

[material.md](material.md)'s ten families are not all available in every idiom. Reading this table the other way is also useful: if the material you want appears in only one column, the idiom was already decided for you.

| Material | Rendered | Flat | Line | Typographic | Pixel | Photographic |
| --- | --- | --- | --- | --- | --- | --- |
| Print grain / stipple | ✓ | ✓ | — | ✓ | — | ✓ |
| Brush ink | ✓ | — | ✓ | ✓ | — | ✓ |
| Glossy plastic / jelly | ✓ | — | — | — | — | ✓ |
| Felted fiber | ✓ | — | — | — | — | ✓ |
| Glazed ceramic | ✓ | — | — | — | — | ✓ |
| Polished stone | ✓ | — | — | — | — | ✓ |
| Matte clay / flat riso | ✓ | ✓ | — | ✓ | — | ✓ |
| Paper cut-out / collage | ✓ | ✓ | — | ✓ | — | ✓ |
| Glass and liquid | ✓ | — | — | — | — | ✓ |
| Chrome / liquid metal | ✓ | — | — | — | — | ✓ |

The shape of that table is the finding: **most materials only exist in the rendered and photographic columns**, which is why a skill that never asks the idiom question lands there every time. The flat, line, typographic and pixel idioms are not materially poor — they substitute a different axis for material entirely. Flat trades material for colour relationships and shape economy. Line trades it for stroke quality. Typographic trades it for the typeface. Pixel trades it for the grid and the palette.

So for those four, **lock 2 asks a different question**: not *what is it made of*, but *what is the one formal system carrying the identity*. Name it as concretely as a material, and the same test applies — write it in a sentence without using *clean*, *simple*, *modern*, or *bold*.

## Mixing

One idiom, as with material. A flat-graphic cast with one photographic hero is two systems.

The single legal exception is the one the genre already uses: **a rendered or photographic character for marketing, with a flat or pixel reduction for the interface** — and only when the reduction is specified in the bible as its own locked drawing, not left to whoever is implementing it to approximate. Lock 1's 24px note already asks for this when the form fails at icon size; this is the same provision, arrived at from the other direction. Two drawings, one system, both specified. Anything beyond that is a moodboard.
