# Stage 2 — Image prompts

Prompts are **derived from the bible, never invented fresh.** If you find yourself making up a detail at prompt-writing time, that detail belongs in stage 1 — go back and lock it, or you will get a different answer on the next render and have no idea which one is canon.

## Clause order

Image models weight early tokens heavily and treat late ones as suggestions. Order the clauses by how much the decision matters and give material the most words, because material is the thing that will otherwise default to "soft 3D gradient".

```
[MEDIUM + MATERIAL]  ← first and longest. 2–4 clauses.
[FORM PRIMITIVE]     ← the constructive recipe, not an adjective.
[FACE]               ← exact marks. Name what is absent.
[STATE / POSE]
[GROUND]
[FRAMING + LIGHT]
--negatives
```

Worked skeleton:

> *Macro photograph of a needle-felted wool character, fuzzy fiber edges breaking the silhouette, completely matte with no specular highlights, soft ambient occlusion where the forms meet. The body is a single soft irregular blob, wider than tall, no limbs, slightly lopsided the way hand-felted wool is. Face is two solid black oval eyes and nothing else — no nose, no eyebrows, no mouth. Sitting upright, at rest. Pure black background, no horizon. Straight-on eye-level view, soft key light from upper left, subject centred with generous clear space.*

Things that routinely go wrong and the fix:

- **"Cute character" anywhere in the prompt** → you get the median mascot of the entire internet. Delete it. Cuteness is an outcome of the form, not an input.
- **Material stated once, late** → it gets ignored. State it first and restate it in different words at the end.
- **Adjectives instead of construction** → "a soft blob" gives you a different blob every time. "A superellipse 1:1.15 tall, the top 40% one unbroken dome" gives you the same one.
- **Naming what's absent is as important as naming what's present.** "No mouth, no nose, no eyebrows" does more work than any positive clause about the face.

## The four jobs

Generate these in order. Each depends on the one before it.

**1. Base portrait.** One character, idle state, canonical ground, straight on. This is the canon image — everything downstream references it. Iterate here until it is right; do not move on with a base you are only 80% happy with, because every later image inherits the 20%.

**2. Turnaround.** The same character at front / three-quarter / side / back, **in one image, on one row.** This is what makes the form primitive real — it is the first moment you find out whether your silhouette actually works in three dimensions or whether it was a flat drawing all along.

**3. Expression sheet.** All five states in one image, as a labelled grid.

**4. Cast sheet.** All members in one image, in a row, on one ground.

## The consistency technique

**Generate each sheet as a single image containing a grid, not as N separate images.**

This is the single most useful technique in this stage and most people learn it the slow way. Five separately-generated images of "the same character thinking / idle / done" will give you five subtly different characters — the body gets rounder, the colour drifts, the eyes change size — and the drift is invisible one image at a time and glaring when you lay them side by side. One generation containing a 5-cell grid forces the model to keep the character identical across cells, because it is literally drawing them in the same picture. Then crop the cells.

> *... The image is a 5-cell expression sheet, one row, even spacing, identical character in every cell, same scale and same lighting throughout. Cell 1: eyes round, at rest. Cell 2: eyes widened into tall ovals, leaning slightly forward. Cell 3: eyes narrowed to short horizontal bars, head tipped down. Cell 4: eyes closed into upward arcs. Cell 5: eyes as short vertical bars, body tilted off-axis.*

Supporting moves, in descending order of reliability:

1. **Reference-image chaining.** Feed the approved base portrait back in as an image reference for every subsequent generation. Most capable models support this and it beats every text-only trick.
2. **Fixed seed** across a batch, varying only the state clause. Helps; does not survive large prompt edits.
3. **Verbatim prefix.** Keep the medium/material/form clauses byte-identical between prompts and change only the state clause. Rewording a clause you meant to hold constant is the most common cause of drift.

## Negatives for this genre

Most of these exist to suppress the defaults that make a mascot look generated. Adapt to whatever negative-prompt syntax your model uses; if it has none, state them as "no X" clauses at the end.

```
glossy gradient orb, purple to indigo gradient, soft glow, bloom, lens flare,
pupils with specular highlights, iris, eyelashes, eyebrows, nose, teeth,
emoji face, anime eyes, Pixar style, 3D cartoon character render,
sparkles, stars, magic dust, floating particles,
headphones, antenna, visor, robot panel lines, screws, seams,
white seamless studio background, drop shadow on white, product render turntable,
text, watermark, logo, signature,
multiple characters (for single-subject jobs), busy background, depth of field
```

Two to add conditionally:

- If your material is **matte** (felt, clay, riso): add `specular highlight, glossy, wet, reflective`.
- If your material is **flat** (riso, cut-paper): add `gradient, soft shading, ambient occlusion, volumetric light`.

## Iterating

Change **one clause at a time** and keep the rest byte-identical. Multi-clause edits make it impossible to know which change did what, and you will burn a lot of generations rediscovering that.

When a render is wrong, diagnose before rewriting:

| Symptom | Cause | Fix |
| --- | --- | --- |
| Looks like every other mascot | Material clause too weak or too late | Move it first, triple its length, add the absence negatives |
| Character drifts between images | Separate generations | Grid in one image; chain the base as a reference |
| Face reads dead or uncanny | Model added pupils/eyelids you didn't ask for | Negatives, plus explicitly "solid black shapes, no interior detail" |
| Form is mushy, unrecognizable at small size | Primitive described with adjectives | Replace with the constructive recipe and the one asymmetry |
| Correct but boring | Ground is a default | Lock the ground clause; "pure black, no horizon" or "flat saturated field, no gradient" |

## Handing it over

Deliver the prompts as a block the user can paste, with the **verbatim-constant prefix separated from the per-image clause**, so they can regenerate any single image months later without re-deriving anything:

```
PREFIX (never change):
  <medium + material + form + face + ground + framing>

PER IMAGE (change only this):
  base        → "sitting upright, at rest"
  thinking    → "eyes narrowed to short horizontal bars, head tipped down"
  ...
```
