# Stage 3 — Code

Also derived from the bible. The job here is a component whose props are the **states you already locked**, built so that the locked decisions are structurally impossible to violate.

## Layer discipline

This is the whole architecture, and it is what keeps a coded character from drifting the way a drawn one does:

```
<svg>
  <defs>  masks, filters, gradients — the material lives here
  <g id="ground">     ground + contact shadow
  <g id="body">       ← path data NEVER changes. Only transforms.
  <g id="face">       ← swapped wholesale per state
  <g id="accessory">  ← swapped per cast member
```

**The body path is written once and never edited again.** Every state is a transform on `#body` plus a different `#face`. If a state tempts you to touch the body's `d` attribute, it is not a state — it is a second character, and you should go back to lock 4 in SKILL.md.

Keep each face variant as a `<symbol>` or a small component and select by state. Do not animate between face shapes by morphing paths; **cut**. Faces in this genre are graphic marks, and marks blink from one to the next. Morphing eyes is uncanny and expensive.

## The component

```jsx
const FACES = {
  idle:      <g><ellipse cx="38" cy="44" rx="5"   ry="5"/><ellipse cx="62" cy="44" rx="5"   ry="5"/></g>,
  listening: <g><ellipse cx="38" cy="44" rx="4.5" ry="6"/><ellipse cx="62" cy="44" rx="4.5" ry="6"/></g>,
  thinking:  <g><rect x="33" y="42" width="10" height="4" rx="2"/><rect x="57" y="42" width="10" height="4" rx="2"/></g>,
  done:      <g fill="none" strokeWidth="4" strokeLinecap="round" stroke="currentColor">
               <path d="M33 46q5-6 10 0"/><path d="M57 46q5-6 10 0"/></g>,
  error:     <g><rect x="36" y="40" width="4" height="9" rx="2"/><rect x="60" y="40" width="4" height="9" rx="2"/></g>,
};

export function Mascot({ state = "idle", size = 96, label }) {
  return (
    <span
      role="status"
      aria-label={label ?? STATE_TEXT[state]}
      data-state={state}
      className="mascot"
      style={{ "--size": `${size}px` }}
    >
      <svg viewBox="0 0 100 100" width={size} height={size} aria-hidden="true">
        <defs>{/* material: masks, gradients, grain */}</defs>
        <g className="mascot__body">
          <path d="…" fill="var(--char-body)" />      {/* never changes */}
          <g className="mascot__face" fill="var(--char-ink)">
            {size >= 32 ? FACES[state] : null}
          </g>
        </g>
      </svg>
    </span>
  );
}
```

Three things in there that matter more than they look:

- **`role="status"` and a real label.** The character is carrying system state. If it is the only indication that the thing failed, a screen-reader user currently has no indication at all. Give each state a sentence, not "mascot".
- **`aria-hidden` on the SVG itself**, so the label is announced once rather than fighting with the graphic.
- **The level-of-detail gate.** Below a threshold, drop what turns to mud. *Which* parts those are depends on contrast, so measure rather than assume: low-contrast surface detail (grain, veining, a soft sheen, a subtle facet) dies first, often by 32px, while **high-contrast cut-out eyes survive much smaller** — dark capsule slits on a bright body still read at 20px, which is why the current crop of AI-product characters work as chat avatars. Gate the lighting early and the face late, and if the face has to go, the silhouette's one asymmetry is what's left carrying it.

Drive everything from `data-state` in CSS rather than from JS-computed styles. CSS transitions retarget smoothly when the state changes mid-animation; a JS keyframe restart does not, and states in this genre change fast and out of order.

## Motion

The essentials are below; **[motion.md](motion.md)** has the rest — blinking, randomising the loop period, exiting a running loop cleanly, secondary motion, interruption, and keeping an always-on character off the main thread.

```css
.mascot__body {
  transform-origin: 50% 100%;   /* it sits on the ground; it does not float */
  transition: scale 260ms cubic-bezier(.34,.9,.2,1), rotate 260ms cubic-bezier(.34,.9,.2,1);
  animation: breathe 3.6s ease-in-out infinite;
}

/* nearly invisible, and volume-preserving: rises as it narrows */
@keyframes breathe {
  0%, 100% { scale: 1 1; }
  50%      { scale: .994 1.012; }
}

[data-state="thinking"] .mascot__body { animation: ponder 2.4s ease-in-out infinite; }
@keyframes ponder {            /* constant amplitude — no climax, loops forever */
  0%, 100% { translate: 0 0;      scale: 1 1; }
  50%      { translate: 0 -2.5%;  scale: .99 1.01; }
}

[data-state="done"]  .mascot__body { animation: settle 420ms cubic-bezier(.34,.9,.2,1) 1; }
[data-state="error"] .mascot__body { animation: none; rotate: -4deg; }  /* persists until handled */

@media (prefers-reduced-motion: reduce) {
  .mascot__body { animation: none !important; transition: none; }
  /* the character stays, and the face still distinguishes the state */
}
```

**Units.** Inside an SVG `viewBox`, CSS `px` in a transform *are* user units, so `translate: 0 -3px` means three viewBox units and stays character-relative at every rendered size. A **percentage resolves against the element's own bounding box** instead — so `-4%` on a small lid group is a completely different distance from `-4%` on the body. Use px (user units) for anything you also wrote down in the bible, or the two will silently disagree.

The rules behind those numbers:

- **Volume-preserving squash.** Wider means shorter by the same ratio. Uniform scale is a balloon inflating.
- **Respect the material's rigidity.** The 1–2% figures above suit something firm. Felt or jelly can take 6–10%; glazed ceramic and enamel should take **zero** — put the motion into a tilt, a hop, or a gap between parts instead.
- **`transform-origin` at the base.** Characters that scale from their centre float; characters that scale from where they touch the ground have weight.
- **The thinking loop has constant amplitude.** Any accent beat implies progress, and on a sixty-second run that beat becomes a lie.
- **Error persists.** It is not an animation that plays and resolves; it is a pose the character holds until a human deals with it.
- **Enter once.** Mount the character and leave it mounted. If it is inside a list or a conditionally-rendered panel, make sure a re-render is not remounting it — replaying the entrance on every state change is the most common bug in this genre.
- **Reduced motion keeps the character.** Kill the loops, never the character; it is load-bearing — and keep the pose that distinguishes the state.
- **Split the two clocks.** Put free-running loops (breath, rock) on an inner group and one-shots (`done`, enter) on an outer one, so they never fight over the same `transform`. This is the first thing to do and it removes most of the need for the loop-exit handling in [motion.md](motion.md).

## Idiom in code

Before the material question comes the lock 0 question: **is this idiom a component at all, or an asset pipeline?**

| Idiom | Stage 3 is |
| --- | --- |
| Flat graphic | Native. SVG shapes and CSS transforms; everything in this file applies unchanged. |
| Drawn line | Native, as filled variable-width outline paths. Motion is stroke redraw, not transform. |
| Typographic | Native, and the cheapest here — `font-variation-settings` where the face is variable, glyph swaps where it is not. |
| Pixel grid | Native as a CSS grid or a canvas. Snap every frame, disable smoothing, scale only by whole multiples. |
| Rendered object | Sprite frames, or a baked cut-out. The layer discipline above still applies — to the *cut-out*, not to a live form. |
| Photographic | Sprite frames. There is no component here, only an asset pipeline and a naming convention. |

The first four put the character inside the codebase, where anyone can add a state in an afternoon. The last two put it in a folder of images that someone has to regenerate. That is the whole of why lock 0 is decided first.

## Material in code

For the rendered and flat idioms, the practical ranking, since this decides whether stage 3 is even possible:

| Material | In CSS/SVG |
| --- | --- |
| Flat riso, enamel | Trivial — flat fills, one offset shape |
| Gloss, ceramic | Good — layered radial gradients + inset shadow (recipe in [material.md](material.md)) |
| Ink | Good — but as a **filled variable-width outline path**, never `stroke-width` |
| Print grain | Workable — `feTurbulence` or a tiled noise overlay, fixed in screen space |
| Cut paper | Workable — per-layer `drop-shadow`, torn edges baked into path data |
| Felt, chrome | Don't. Render sprite frames instead. |

Cut-out eyes beat painted eyes for any glossy or layered material: punch the face through the body with an SVG `<mask>` so the darker layer beneath shows through. It is the difference between an object and a sticker.

## The swap axis

Put the cast's two variable axes — and only those two — on custom properties, so a new cast member is a few lines and never a new file:

```css
.mascot            { --char-body: oklch(.52 .16 32); --char-ink: oklch(.18 .02 40); }
.mascot[data-who="reservations"] { --char-body: oklch(.58 .13 150); }
```

If adding a cast member ever requires touching the SVG path data, the system has broken its own first lock. Fix the system, not the member.
