# A full run — "Settle"

One pass through the whole pipeline on an invented product. The product is deliberately not a chatbot, because the interesting decisions only appear when the viewing conditions are real.

---

## Stage 0 — Brief

**The product.** Settle answers the phone for small restaurants. It takes takeaway orders and reservations over a real phone line, and shows what it is doing on a tablet mounted on the kitchen wall.

**1. What does it do?** It listens to a stranger on the phone and writes down what they want, while a kitchen runs.

**2. What is the character's job?** **Status** — and status read at three metres, by someone holding a pan, through a screen with grease on it, in a room too loud for sound to be an option. There is no hero pose, no marketing surface, no app icon. This one answer disqualifies most of the obvious directions, which is what a good brief does.

**3. What must it never imply?** That an order is handled when it isn't. A missed reservation on a Friday is real money, and the staff must be able to tell across the room that something needs a human. The character may never look pleased while it is failing.

## Stage 1 — Three directions

The first instinct was a kettle. **That instinct is the trap in lock 1** — restaurant product, therefore kitchen object. It is a pun, every competitor lands on it, and it stops being interesting the moment you get it. So all three directions take a **formal** source instead and let the connection be structural.

| | Direction | Formal source | What it buys |
| --- | --- | --- | --- |
| **A** | **A self-righting solid** — a weighted body that always returns upright | The tumbler / okiagari: a form whose low centre of mass gives it exactly one resting position | Its defining *behaviour* is the product's promise. And "held tipped and not springing back" is a free, alarming error state |
| **B** | **A plumb bob** — a dense teardrop that hangs true | The mason's tool for trueness | A crooked plumb bob is a contradiction, so "wrong" is legible. But it hangs, and a wall tablet has nothing to hang from |
| **C** | **A stacked cairn** — two or three forms resting on each other | Trail markers | Stacking reads as accumulating orders. Fragile at small sizes, and toppling is too violent for a kitchen |

**Picked: A.** Not because a rounded body is nice, but because the physics *is* the message — it always comes back to rest, and the one thing it must never do is stay tipped over.

### The hardness correction

The first build of A took the self-righting idea to its most literal formal source — a **gömböc**, the convex solid with one stable and one unstable equilibrium — and rendered it in polished serpentine: deep green-black, a sharp apex, a straight honed cut.

It read **raw**. Cold, mineral, a little forbidding. Nothing to do with the product, and wrong for a character people share a kitchen with for ten hours a day.

The fix was not a new material so much as the **hardness dial** in lock 2, and only two of the six levers did almost all of it:

- **Silhouette** — the apex and the straight cut went. The body is now fully convex with nothing you could cut yourself on. *One straight edge was keeping the whole thing hard.*
- **Light** — the pinpoint specular became a **broad** gradient across most of the form. Hard gloss reads wet and mineral; soft gloss reads plush.

The other four followed: brighter and more saturated, a deliberately asymmetric outline, squash restored now that the material deforms, and a wide diffuse contact shadow instead of a tight dark one.

The behavioural idea survived the change intact, which is the point — **the formal source was never the gömböc's geometry, it was its physics.** A soft weighted tumbler keeps every bit of that.

## Stage 1 — The locks

### 1. Form primitive

A **soft weighted bean**: a fully convex body, taller than wide, narrower at the top and heavier toward the base, with a low centre of mass. Held in one sentence: *a rounded body with no corners anywhere and all its weight in the bottom third, so it always comes back upright.*

**Limb budget: zero.**

**The one asymmetry: the outline is deliberately not symmetrical** — the left shoulder sits a little higher than the right, and the base is fractionally off-centre. It is small enough that nobody consciously registers it and large enough that the form reads as made rather than generated. A perfectly mirrored bean reads as a shape; a slightly lopsided one reads as a body.

### 2. Material

**Soft gloss** — the plush-vinyl end of the gloss family, not the wet-plastic end. Bright saturated body colour, a **broad** light gradient falling across the upper-left two-thirds with **no hotspot anywhere**, a soft shaded pool at the lower right, and a faint bounce of light along the bottom edge. No texture, no grain, no facets, no visible outline.

Chosen against the brief: a bright saturated body against a dark ground is the highest-contrast thing available on a backlit panel, which is what three metres through greasy glass actually needs — and unlike the deep stone, it gets that contrast from *brightness* rather than from weight, so it stays warm.

### 3. Face vocabulary

**Two capsule slits, cut out of the body, tilted about 15° off vertical.** No mouth, no nose, no pupils, no highlights.

The tilt is not decoration — it is most of the personality. Two perfectly vertical slits read as a machine readout; the same two slits rotated 15° read as a face with an attitude, and it costs one number.

No mouth, for the brief's hard constraint: a character with no mouth **cannot smile while it is failing**. Solved by subtraction rather than by discipline.

The slits are the right call at this size range for a second reason — being dark marks on a bright body, they are the highest-contrast thing in the character, so they survive being shrunk far better than any surface detail.

### 4. Expression range

Each state is a body transform plus an eye shape. Two things, never more.

| State | Body | Eyes | Motion |
| --- | --- | --- | --- |
| **idle** | upright | two tilted slits, full length | breathes 1 → 1.016, volume-preserving, 3.6s |
| **listening** | leans 5° toward you, stretches up 3% | slits longer and wider | none; the lean holds |
| **thinking** | rocks ±7° through its basin, squashing at each extreme | slits shortened to stubs | 2.6s, constant amplitude, no peak |
| **done** | squash, overshoot, settle | **closed upward arcs** | 560ms, once |
| **error** | **tipped 19° and squashed, and it stays there** | short stubs splayed in opposite directions | none; it holds |

Two notes on the sheet.

**`done` is the one state that leaves the capsule vocabulary**, and it earns the exception: a closed arc is the only mark that reads unambiguously as *finished and content*, and every within-vocabulary alternative tested identical to `thinking` at a glance. A cast may hold one deliberate exception; it may not hold five.

**The error state is the form paying for itself.** A weighted tumbler that stays tipped is a physical contradiction — the viewer's eye expects it to spring back, it doesn't, and that unresolved wrongness is exactly the feeling *needs a human* should produce. It is a silhouette change, not a colour, so it survives grayscale, glare, colourblindness and 26px, and it holds until somebody touches the tablet.

### 5. Accessory and prop system

**One attachment zone: the crown.** Nothing attaches anywhere else. In practice this cast doesn't use it — **colour alone is carrying three members**, and an accessory would be the next axis if it grew past five or six.

| Member | Colour | Job |
| --- | --- | --- |
| Settle | persimmon | phone orders |
| Settle | jade | reservations |
| Settle | amber | the kitchen queue |

### 6. Motion personality

Soft and springy, with a real low centre of mass. Ease `cubic-bezier(.3, 1.25, .4, 1)` — a gentle overshoot, which a rigid material could not have had and this one needs.

**Squash is back, and volume-preserving.** Wider means shorter by the same ratio. Every state squashes a little; `done` squashes a lot, because a soft body landing is the clearest "finished" signal available without a sound.

### 7. Ground

**Deep warm charcoal**, flat, no gradient and no grid. Dark rather than the pale tile of the earlier draft, for a reason from the brief: on a backlit wall panel a bright saturated body on a dark field is the most legible combination at distance, and it is the one that looks least grubby under kitchen light.

Contact shadow: wide and diffuse — 23 × 3.4 units at 2.6 blur, 50% — and it **widens to 17 units at 62% in the error state**, because less contact and more spread is half of why that pose looks wrong. Clear space 25% of body width.

```
--body (orders)       oklch(.66 .18 40)    persimmon
--body (reservations) oklch(.72 .14 166)   jade
--body (queue)        oklch(.70 .15 85)    amber
--ink                 oklch(.20 .03 40)    the cut-out slits
--ground              oklch(.22 .012 60)   warm charcoal
```

---

## Stage 2 — Prompts

**PREFIX — never change:**

> *Studio photograph of a small soft vinyl toy character. Plush matte-glossy surface in a bright saturated persimmon, lit by a single large softbox so the highlight is a broad soft gradient across the upper left with no sharp hotspot anywhere, a gently shaded pool at the lower right, and a faint bounce of light along the bottom edge. The form is a single rounded bean, taller than wide, narrower at the top and heavier toward the base, fully convex with no corners, no edges and no points, and very slightly asymmetrical as if hand-shaped. No limbs, no arms, no legs. The face is two dark capsule slits cut into the surface, tilted about fifteen degrees off vertical — no mouth, no nose, no pupils, no highlights in the eyes. Flat dark warm charcoal background, no gradient, no horizon. Straight-on eye-level, wide soft contact shadow beneath, generous clear space.*

**PER IMAGE — change only this clause:**

```
base       → "sitting upright at rest, slits at full length"
listening  → "leaning slightly toward the viewer and stretched a little taller, slits longer and wider"
thinking   → "tipped a little off upright and squashed slightly wider, slits shortened to short stubs"
done       → "settling, eyes closed into two upward arcs"
error      → "tipped over to one side and squashed, not righting itself, slits short and splayed apart"
```

**Expression sheet** — one generation, not five:

> *PREFIX, then: the image is a 5-cell expression sheet in one row, even spacing, the identical character in every cell at the same scale and lighting. Cell 1 upright at rest. Cell 2 leaning toward the viewer, slits longer. Cell 3 tipped and squashed wider, slits short. Cell 4 upright, eyes closed into upward arcs. Cell 5 tipped far over and squashed, slits splayed apart.*

**Negatives:**

```
sharp specular highlight, glossy wet plastic, chrome, metal,
gradient orb, purple, indigo, glow, bloom, sparkles, floating particles,
pupils, iris, eyebrows, nose, mouth, teeth, emoji face, anime eyes,
Pixar style, 3D cartoon character render, arms, legs, hands,
corners, edges, faceted, low poly, crystalline,
white seamless studio background, drop shadow on white, depth of field,
text, watermark, logo
```

(`sharp specular highlight` leads the list because it is the single thing that will drag this back to reading hard.)

---

## Stage 3 — Code

Rendered and corrected in a browser at 120px, 44px and 26px; this is the version that survived that.

```jsx
const BODY = "M51 18 C65 18 76 29 79 45 C82 62 79 79 69 87 C61 93 39 93 31 87 C21 79 18 62 21 45 C24 29 37 18 51 18 Z";

// one capsule generator — every eye in the system is this function with different numbers
const slit = (cx, cy, len, w, tilt) => (
  <rect x={cx - w / 2} y={cy - len / 2} width={w} height={len} rx={w / 2}
        transform={`rotate(${tilt} ${cx} ${cy})`} />
);

const EYES = {
  idle:      <>{slit(40, 52, 19, 8.2, 15)}{slit(61, 52, 21, 8.2, 15)}</>,
  listening: <>{slit(40, 51, 22, 9,   13)}{slit(61, 51, 24, 9,   13)}</>,
  thinking:  <>{slit(40, 54, 10, 8.2, 15)}{slit(61, 54, 11, 8.2, 15)}</>,
  done:      <g fill="none" stroke="var(--ink)" strokeWidth="6.8" strokeLinecap="round">
               <path d="M34.5 56 q5.5 -8 11 0"/><path d="M55.5 56 q5.5 -8 11 0"/></g>,
  error:     <>{slit(40, 53, 10.5, 8.2, -24)}{slit(61, 53, 10.5, 8.2, 24)}</>,
};

const SAYS = {
  idle: "Settle is at rest",
  listening: "Settle is on a call",
  thinking: "Settle is writing the order down",
  done: "Order sent to the kitchen",
  error: "Settle could not take this one — needs you",
};

export function Settle({ state = "idle", size = 120, who = "orders" }) {
  const id = `${state}-${who}`;
  return (
    <span className="bean" role="status" aria-label={SAYS[state]}
          data-state={state} data-who={who}>
      <svg viewBox="0 0 100 100" width={size} height={size} aria-hidden="true">
        <defs>
          {/* broad and soft — no hotspot. This is the whole difference from hard gloss. */}
          <radialGradient id={`l-${id}`} cx="30%" cy="20%" r="68%">
            <stop offset="0"   stopColor="#fff" stopOpacity=".40"/>
            <stop offset=".55" stopColor="#fff" stopOpacity=".06"/>
            <stop offset="1"   stopColor="#fff" stopOpacity="0"/>
          </radialGradient>
          <radialGradient id={`d-${id}`} cx="76%" cy="92%" r="56%">
            <stop offset="0" stopColor="#000" stopOpacity=".22"/>
            <stop offset="1" stopColor="#000" stopOpacity="0"/>
          </radialGradient>
          <radialGradient id={`r-${id}`} cx="40%" cy="98%" r="42%">
            <stop offset="0" stopColor="#fff" stopOpacity=".22"/>
            <stop offset="1" stopColor="#fff" stopOpacity="0"/>
          </radialGradient>
          <filter id={`s-${id}`} x="-70%" y="-70%" width="240%" height="240%">
            <feGaussianBlur stdDeviation="2.6"/>
          </filter>
        </defs>

        <ellipse className="bean__shadow" cx="50" cy="92" rx="23" ry="3.4"
                 filter={`url(#s-${id})`}/>

        <g className="bean__body">
          <path d={BODY} fill="var(--body)"/>
          {/* soft lighting is low-contrast, so it is the first thing to go when small */}
          {size >= 28 && <>
            <path d={BODY} fill={`url(#d-${id})`}/>
            <path d={BODY} fill={`url(#r-${id})`}/>
            <path d={BODY} fill={`url(#l-${id})`}/>
          </>}
          {/* the slits are high contrast and stay down to ~20px */}
          <g className="bean__eyes" fill="var(--ink)">{EYES[state]}</g>
        </g>
      </svg>
    </span>
  );
}
```

```css
.bean {
  --body: oklch(.66 .18 40);
  --ink:  oklch(.20 .03 40);
  display: inline-block;
}
.bean[data-who="reservations"] { --body: oklch(.72 .14 166); }
.bean[data-who="queue"]        { --body: oklch(.70 .15 85);  }
.bean__shadow { fill: oklch(.05 0 0 / .5); transition: rx 300ms ease, opacity 300ms ease; }

/* soft and weighted: it deforms, and it always comes back upright */
.bean__body {
  transform-origin: 50px 90px;          /* the low centre of mass */
  transition: rotate 380ms cubic-bezier(.3,1.25,.4,1),
               scale 380ms cubic-bezier(.3,1.25,.4,1);
  animation: breathe 3.6s ease-in-out infinite;
}
/* volume-preserving: narrower as it rises */
@keyframes breathe { 0%,100% { scale: 1 1; } 50% { scale: .986 1.016; } }

[data-state="listening"] .bean__body { animation: none; rotate: -5deg; scale: .98 1.03; }

[data-state="thinking"] .bean__body { animation: rock 2.6s ease-in-out infinite; }
@keyframes rock {                      /* constant amplitude — no peak */
  0%,100% { rotate: -7deg; scale: 1.02 .984; }
  25%,75% { rotate:  0deg; scale: .99 1.012; }
  50%     { rotate:  7deg; scale: 1.02 .984; }
}

[data-state="done"] .bean__body { animation: pop 560ms cubic-bezier(.3,1.5,.4,1) 1; }
@keyframes pop { 0% { scale: 1.1 .9; } 45% { scale: .94 1.07; } 100% { scale: 1 1; } }

/* a weighted tumbler that stays tipped is a contradiction, and that is the alarm */
[data-state="error"] .bean__body   { animation: none; rotate: -19deg; scale: 1.06 .93; }
[data-state="error"] .bean__shadow { rx: 17; opacity: .62; }

@media (prefers-reduced-motion: reduce) {
  .bean__body { animation: none !important; transition: none; }
  /* the static pose and the eye shape still distinguish all five states */
}
```

### What the render corrected

1. **The stone draft read raw.** Covered above — silhouette and light were the two levers that mattered, and changing the material alone would not have done it.
2. **`done` was identical to `thinking` at a glance.** Keeping it inside the capsule vocabulary produced two short stubs either way. Closed arcs fixed it, and that exception is now documented in the sheet rather than left as an inconsistency.
3. **Gating the face at 32px was the wrong gate.** It was copied from the stone draft, where the face *was* the fragile part. Here the opposite is true: the low-contrast soft lighting turns to mush first, while the dark cut-out slits still read at 20px. The gate now drops the lighting at 28px and keeps the eyes all the way down — which is exactly why this genre's characters work as chat avatars.

---

## The audit

**Anti-slop pass.** No domain pun — the form came from a tumbler's physics, not from a kitchen. No gradient orb: the body has a *broad* gradient, but it also has a committed silhouette, a face vocabulary and a ground, which is the difference between a material and a default. No glow, no sparkle, no emoji, no pupils-with-highlights, no robot signifiers. No purple. The cast differs by colour alone, never by shape.

**24px test.** Rendered at 26px with the lighting dropped: a bright bean with two dark tilted slits, and the error state clearly tipped with its eyes splayed. It passes, and this direction passes it *better* than the stone did — high-contrast marks shrink well, low-contrast surfaces don't.

**Description test.** *Form:* a fully convex bean, taller than wide, weight in the bottom third, slightly lopsided. *Material:* soft gloss — bright saturated body, one broad highlight with no hotspot, a soft shaded pool, a faint bounce along the base. Neither sentence needed *cute*, *friendly*, *modern*, *playful*, or *approachable*.
