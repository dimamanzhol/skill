# Material

Material is the lock that does the most work and gets skipped the most often. It decides what the character is made of, which in turn decides how light hits it, how it deforms, what size it survives at, and what it costs to produce. Two characters with the same silhouette and different materials are different products.

**Flat vector fill and soft gradient are not materials.** They are what you get when nobody chose. The whole generic-mascot problem is downstream of that.

## Choosing

Choose for the viewing condition first and the mood second. A material that dies at your actual display size is the wrong material no matter how good it looks in the hero render.

| Condition | Take | Avoid |
| --- | --- | --- |
| Lives at 16–32px in the UI | Gloss, enamel, flat riso — anything whose read is *one bright mark* or *one hard edge* | Print grain, felt, fine texture: all mud |
| Seen at distance or in glare | Gloss, chrome | Matte anything |
| Large, static, marketing | Print grain, felt, collage, glass — texture rewards the pixels | Chrome (reads cheap at size) |
| Must animate in code, no rendered assets | Gloss, enamel, flat riso, ink | Felt, chrome: effectively require rendered frames |
| Must feel handmade / not-a-corporation | Print grain, ink, collage, clay, stone | Gloss, chrome |
| Must feel like a toy you could hold | Gloss, felt, ceramic | Ink, collage |
| Domain is serious (health, finance, security) | Enamel, flat riso, ink | Felt, googly gloss |

Then check the physics consequence, because it constrains motion before you write a line of it:

| Material | Deforms? | Motion goes into |
| --- | --- | --- |
| Felt, jelly, clay | Yes, freely | Squash and stretch, volume-preserving |
| Gloss / hard plastic | Barely, 2–4% | Squash, plus the highlight sliding across the surface |
| Ceramic, enamel, stone | No | Joints, gaps, tilts, hops — rigid-body motion only |
| Glass and liquid | No — the liquid moves, not the vessel | Over-damped drift; never a spring, never an overshoot |
| Ink | N/A — it's a mark | Redrawing the stroke; the line itself has weight |
| Print grain | N/A — it's a surface | The grain stays fixed while the form moves beneath it |

---

## The families

### Print grain / stipple

**Says:** handmade, analog, editorial, a little melancholy. Reads as something that was printed rather than rendered, which is a strong anti-corporate signal.

**Looks like:** heavy monochrome stipple or halftone across the whole form, no smooth areas anywhere. Value is carried entirely by dot density. Often a faint warm halo tracing the outline — ink spread, or a second plate slightly misregistered.

**Prompt language:** `risograph print`, `monochrome stipple shading`, `charcoal grain`, `halftone dot texture`, `visible paper tooth`, `single-plate print`, `slight misregistration`, `no smooth gradients`.

**In code:** an SVG `feTurbulence` + `feComponentTransfer` grain layer, or a tiled noise PNG at `mix-blend-mode: multiply` over flat fills. Keep the grain *fixed in screen space* while the form moves under it — grain that translates with the body looks like a texture map and breaks the print illusion.

```css
.grain::after {
  content: ""; position: absolute; inset: 0;
  background-image: url(noise.png); background-size: 180px;
  mix-blend-mode: multiply; opacity: .55; pointer-events: none;
}
```

**Size floor:** ~64px. Below that the dots merge into flat gray and you have lost the only thing you chose.

**Fails when:** the grain is applied as a soft overlay at 10% opacity. Half-committed grain reads as a dirty screen. Commit or drop it.

### Brush ink

**Says:** humanist, confident, fast, calligraphic. The only material where the *gesture* is the character.

**Looks like:** one continuous stroke with real pressure variation — thick in the middle of a movement, tapered at entry and exit, occasionally dry-brushed. Slight bleed where the stroke slows or turns.

**Prompt language:** `single continuous ink brush stroke`, `tapered brush ends`, `pressure variation`, `sumi-e`, `dry brush edge`, `confident one-stroke gesture`, `slight ink bleed`.

**In code:** do **not** use `stroke` with `stroke-width`. A uniform-width stroke is the dead giveaway. Draw the stroke as a **filled outline path** whose width varies along its length, and animate by swapping or morphing that filled path. `stroke-linecap: round` on a uniform stroke is not a taper.

**Size floor:** ~24px, if the stroke is thick enough.

**Fails when:** the stroke is symmetrical or geometrically perfect. The whole value of ink is evidence of a hand moving at speed. Draw it fast, then clean up the least.

### Glossy specular plastic / jelly

**Says:** toy-like, tactile, playful-corporate, wants to be squeezed. The most "product" of the materials and the safest at small sizes.

**Looks like:** saturated base color, one **hard** specular hotspot (small, high-contrast, upper-left or wherever your key light is — pick once and never move it), a broad soft sheen below it, a thin bright rim where light wraps the far edge, and a soft contact shadow directly under the body. A hint of subsurface glow at thin edges sells it as jelly rather than plastic.

**Prompt language:** `glossy vinyl toy`, `hard specular highlight`, `soft studio key light upper left`, `jelly translucency at the edges`, `soft contact shadow`, `saturated matte-free surface`, `product photography of a toy`.

**In code:** entirely doable in CSS/SVG and the main reason to pick this family for an in-product character.

```css
.gloss {
  background:
    radial-gradient(circle at 32% 24%, #fff 0 4%, transparent 26%),      /* hard hotspot */
    radial-gradient(circle at 38% 34%, rgb(255 255 255 / .38) 0, transparent 52%), /* sheen */
    radial-gradient(circle at 70% 82%, rgb(255 255 255 / .22) 0, transparent 34%), /* rim bounce */
    var(--char-body);
  box-shadow: inset -6% -8% 14% rgb(0 0 0 / .22), 0 6% 10% -2% rgb(0 0 0 / .3);
}
```

**Make the eyes cut-outs, not paint.** Two capsules punched through to a lighter layer beneath read as an object; the same two capsules painted on read as a sticker. In SVG, use a `<mask>`.

**Size floor:** ~16px. The hotspot alone carries it.

**Fails when:** the highlight is a soft blurry blob *and* nothing else changes. Gloss is defined by the hard edge of the specular; blur it on its own and you are back to the generic gradient orb.

**Hard gloss vs soft gloss.** These are two different materials that people reach for with the same word, and the difference is the whole raw-versus-soft axis:

| | Hard gloss (wet plastic, glazed) | Soft gloss (jelly, plush vinyl) |
| --- | --- | --- |
| Highlight | small, hard-edged hotspot | broad gradient over 50–70% of the form, no hotspot |
| Silhouette | can take a corner | must stay fully convex |
| Shadow | tight and dark | wide and diffuse |
| Deforms | 2–4% | 6–10%, squash and stretch |

Soft gloss is what the current crop of AI-product characters use — bright saturated bodies, a broad soft falloff, cut-out capsule eyes, nothing sharp anywhere. It is also the most forgiving at small sizes, because the read is *one bright shape plus two dark marks* rather than any detail.

### Felted fiber / plush

**Says:** warm, soft, handmade, harmless. The most affectionate material available, and the hardest to produce.

**Looks like:** the silhouette edge is **not crisp** — individual fibers break the outline. Fully matte, no highlight anywhere. Soft ambient occlusion where forms meet or overlap. Slightly irregular body, because wool never comes out symmetrical.

**Prompt language:** `needle-felted wool`, `fuzzy fiber edge`, `matte, no specular`, `soft ambient occlusion`, `handmade felt craft`, `irregular hand-shaped form`, `macro photograph of a felt object`.

**In code:** effectively requires rendered assets. A reasonable approximation is an `feTurbulence` displacement on the silhouette edge plus a fiber-noise overlay, but it will not be convincing in motion. **If the character must animate in code and you want felt, render sprite frames instead** — that is the honest answer, and it is the one most teams shipping felted characters actually take.

**Size floor:** ~48px. The fuzzy edge is the whole material and it needs pixels.

**Fails when:** it has a highlight. One specular dot and it is plastic pretending to be wool.

### Glazed ceramic

**Says:** domestic, durable, quietly premium, an object with a function. Underused and it reads as deliberate precisely because nobody picks it.

**Looks like:** deep saturated glaze with a wet look, one hard specular, a *thicker darker pool of glaze near the bottom edge* where it ran before firing, a thin lighter rim at the very bottom, and an unglazed matte foot. Optional micro-crazing at very low opacity.

**Prompt language:** `glazed stoneware`, `wet glossy glaze`, `glaze pooling at the base`, `unglazed matte foot`, `fine crazing`, `studio pottery`, `kiln-fired ceramic`.

**In code:** the gloss recipe above, plus a bottom-edge inner shadow in a darker, *more saturated* version of the body color (glaze pooling is thicker color, not black) and a 1px lighter rim.

**Size floor:** ~24px.

**Physics gift:** ceramic is rigid, so squash is off the table and you are forced to put motion into joints, tilts, and hops. This produces better and more distinctive motion than free deformation does.

### Polished stone

**Says:** dense, old, unhurried, made by erosion rather than by a factory. Reads as an object with mass, and it is the rare material that feels neither corporate nor cute.

**Looks like:** a *broad* soft sheen rather than a pinpoint specular — polish on stone scatters where polish on glaze does not. Faint mineral veining running on one consistent diagonal. One honed matte plane where the stone was worked flat, only a little lighter than the polished surface. Deep value overall, so the sheen has somewhere to be bright.

**Prompt language:** `polished serpentine`, `waxy polish, broad soft sheen, no sharp specular`, `mineral veining on one diagonal`, `honed matte worked plane`, `river-worn`, `macro photograph of a stone object`.

**In code:** a blurred white ellipse clipped to the body, plus two or three very low-opacity veining ellipses on a shared rotation. A second darker plane bounded by the form's ridge is what makes it read as solid rather than as a flat shape — without it you get a pebble.

**Size floor:** ~40px for the material; the *form* may have a higher floor still, since stone shapes tend to be smooth and smooth shapes shrink badly.

**Physics gift:** rigid, so no squash — all motion is rigid-body rotation, and the idle belongs in the light rather than in the body (see lock 6).

**Fails when:** it gets a hard pinpoint highlight, which turns it into plastic, or when the veining is high-contrast, which turns it into marble novelty.

### Matte clay / flat riso

**Says:** graphic, printed, confident, slightly retro. The flattest honest option — a deliberate absence of lighting rather than a missing one.

**Looks like:** flat saturated fills with **no** gradient, no highlight, no shadow. Depth comes from overlap and from a single offset shape in a second colour, the way a two-plate print works. Edges are hard.

**Prompt language:** `flat two-color riso print`, `no shading`, `no gradient`, `offset second plate`, `screen print`, `hard-edged flat shapes`, `limited palette`.

**In code:** trivially cheap — flat `fill` values, one offset duplicate path. Scales to any size, any format, any theme.

**Size floor:** none. This is the material for a 16px status chip.

**Fails when:** someone adds a drop shadow "for depth". The flatness is the decision.

### Paper cut-out / collage

**Says:** crafted, slightly unruly, editorial, made by a person with scissors.

**Looks like:** layered shapes with visible torn or cut edges, a fine paper-fiber fringe on tears, and a **tight, short** drop shadow proving the layers are physically stacked. Slight rotation on each layer.

**Prompt language:** `cut paper collage`, `torn paper edge with fiber`, `layered construction paper`, `short hard drop shadow between layers`, `visible paper texture`, `handmade collage`.

**In code:** achievable — each layer its own `<path>` with a short `filter: drop-shadow(0 2px 1px rgb(0 0 0/.18))`, torn edges baked into the path data. Animate by moving layers independently at slightly different rates; the parallax is what sells it.

**Size floor:** ~40px.

### Glass and liquid

**Says:** instrument, calibration, something measured rather than asserted. The only material in this list that is mostly *not there*, which makes it the one to reach for when the character must avoid claiming certainty.

**Looks like:** a bright caustic where light focuses through the curve (along the floor of a horizontal form, along one side of a vertical one), a soft broad highlight opposite it, a visible meniscus where liquid meets glass, and a thin rim of near-white at every edge. Colour lives in the *liquid*, not the glass.

**Prompt language:** `borosilicate glass vial`, `oil-filled`, `caustic light through the curve`, `meniscus`, `refraction`, `the background bending through it`, `thin bright rim`, `macro product photograph on a light ground`.

**In code:** the honest answer is that CSS has no refraction, so you fake the *consequence* rather than the cause — draw whatever sits behind the form a second time, clipped to it and distorted. Get the distortion right and nobody asks how: a **horizontal cylinder magnifies across its axis and not along it**, so horizontal lines behind it bow apart while vertical lines stay straight. That asymmetry is the whole tell. A uniform scale reads as a second, different background rather than as glass.

**Size floor:** ~32px, and it is a real floor — caustics, meniscus and rim all collapse below it, leaving a coloured capsule. Design the icon-weight variant deliberately; cropping the hero does not work.

**Physics gift:** liquid is *over-damped*. Nothing in glass springs or overshoots — it drifts into place slowly and takes a long time to fully stop, which is a motion personality most materials cannot give you.

**Fails when:** there is nothing behind it. Glass on a flat field is a coloured pill. **Lock 7 is not free for this material** — the ground has to carry structure, and a light grid is the natural answer.

### Chrome / liquid metal

**Says:** slick, synthetic, a bit cold, nineties-revival. High risk — it is currently fashionable, which means it will date.

**Looks like:** a mirrored surface reflecting an environment that must be *invented and kept consistent*: a horizon line, a warm light above, a cool bounce below. Extreme value range, from blown-out white to near-black, within millimetres.

**Prompt language:** `liquid chrome`, `mirror-polished metal`, `studio HDRI reflection`, `warm key from above, cool bounce below`, `sharp horizon line in the reflection`, `high dynamic range`.

**In code:** a layered `conic-gradient` plus `linear-gradient` can approximate it, but the reflection must stay fixed relative to the *world*, not the body, or the illusion dies the moment it moves.

**Size floor:** ~48px. Below that the value range collapses to gray.

**Fails when:** the reflection is a generic rainbow sweep. Chrome reflects a specific room; invent the room.

---

## Mixing

Don't. One material per character system.

The two legitimate exceptions, both of which require the base material to stay dominant:

1. **A held prop in a second material.** The blob is print-grain and the sphere it cradles is clear glass. The contrast is the point and it is confined to one object.
2. **An accessory in a second material.** Felt body, real wire-frame glasses. The accessory is an appliqué; the body is untouched.

A body that is partly glossy and partly fuzzy is not a style, it is an unfinished decision.
