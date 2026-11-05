# Motion

General easing and duration craft — which curve for an entering element, how long a modal takes — is the same here as anywhere, and a dedicated animation skill covers it if one is installed. This file is only the part that is specific to characters, because a character breaks most of the assumptions UI motion is built on: it is **always on screen**, it **loops forever**, its states arrive **out of order**, and it is supposed to look **alive** rather than responsive.

| | |
| --- | --- |
| The two clocks | the single most useful idea here |
| Blinking | the cheapest aliveness signal there is |
| Breaking the period | why a correct loop still reads as a machine |
| Leaving a loop cleanly | the hard problem, with the three-line answer |
| Secondary motion | lag, and why the hat should arrive late |
| Weight, anticipation, gaze | the character-animation vocabulary, trimmed |
| Interruption | states do not arrive in a tidy order |
| The always-on cost | a permanent animation is a permanent bill |
| Reduced motion | what to keep, not just what to kill |
| Timing table | numbers to start from |

## The two clocks

A character runs on two clocks at once, and most implementation bugs in this genre come from treating them as one.

- **The continuous clock** — the idle breath, the thinking rock, the blink. Free-running, loops forever, nobody triggered it, and it should keep its phase across state changes where it can.
- **The event clock** — `done` fires, `error` arrives, the user clicks. One-shot, triggered, has a beginning and an end.

Keep them on separate elements. Put the continuous clock on an inner group and the event clock on an outer one, so a `done` pop can play without stopping the breath underneath it and without the two fighting over the same `transform`:

```html
<g class="char__event">     <!-- one-shots: pop, tilt, enter -->
  <g class="char__loop">    <!-- free-running: breathe, rock -->
    <g class="char__face"/>
  </g>
</g>
```

Do this first. It means a one-shot never has to stop a loop to play, which removes a whole class of jolt on its own — but note what it does *not* solve: when the `thinking` rock itself has to stop, you still need the freeze/switch/release below. The split buys you clean one-shots, not clean loop exits.

## Blinking

A blink is the highest ratio of aliveness to effort in the whole system, and it is the thing most product characters are missing. It also solves a problem you already have: the rule that face marks **cut rather than morph** between states leaves a visible pop, and **a blink hides the cut**.

- **Duration 80–140ms**, and asymmetric: the close is faster than the open, roughly 40ms close / 20ms hold / 70ms open. A symmetric blink looks like a shutter.
- **Interval 2–6 seconds, randomised.** This matters more than the duration. A blink on a fixed timer is the single most robotic thing a character can do, and viewers detect the periodicity long before they can name it.
- **Double-blink about one time in six.** Real eyes do it, and its irregularity is most of what sells the rest.
- **Blink on state entry**, under the eye-shape swap. Free, natural, and it hides the cut.
- **Never blink in `error`.** A character that is stuck should stare. Suppressing a reflex is a strong signal precisely because the reflex was established.
- **Never blink when the eyes are already closed** (`done`, if your done-state is closed arcs).
- A blink is a **`scaleY` on the eye group about its own centre**, not a second set of artwork.

```css
.char__eyes { transform-box: fill-box; transform-origin: 50% 50%; }
@keyframes blink {
  0%, 92%, 100% { scale: 1 1; }
  95%           { scale: 1 .06; }   /* not 0 — a zero-height mark can disappear entirely */
  97%           { scale: 1 1; }
}
```

Driving the interval from JS beats a long keyframe, because that is where the randomness lives:

```js
function scheduleBlink(el) {
  const gap = 2000 + Math.random() * 4000;
  setTimeout(() => {
    el.animate(
      [{ scale: "1 1" }, { scale: "1 .06", offset: .35 }, { scale: "1 1" }],
      { duration: Math.random() < .17 ? 320 : 130, easing: "ease-in-out" }
    );
    scheduleBlink(el);
  }, gap);
}
```

## Breaking the period

A loop that is technically correct still reads as a machine if its period is visible, and at 3–4 seconds it always is. Two fixes, both nearly free:

1. **Jitter the period** by ±10–15% each cycle.
2. **Run two loops at coprime periods** and let them drift against each other. A breath at 3.6s and a sway at 5.3s produce a combined pattern that does not visibly repeat for about 19 seconds — long enough that nobody clocks it.

The second is better because it costs nothing at runtime. Give the breath and the sway to two different groups and pick periods that do not share a factor.

## Leaving a loop cleanly

**The hard problem.** `thinking` is mid-rock at −5° when `done` fires. Swap the animation and the element snaps to the new animation's 0% frame — a visible jolt, and the most common polish failure in a state-driven character.

Three answers, worst to best:

1. **Make every loop start and end at neutral** and accept a catch-up transition. Cheapest, and it quietly constrains every loop you will ever write.
2. **Design the loop to pass through neutral and only switch on the pass.** Most correct, needs a scheduler, rarely worth it.
3. **Commit the current computed value, then release it.** The right answer, and the one with a trap in it:

```js
const PARTS = ".char__event, .char__loop, .char__sway";
const COMMITTED = ["rotate", "scale", "translate", "transform"];

function setState(root, next) {
  const parts = root.querySelectorAll(PARTS);

  // 1. freeze every running loop exactly where it is
  parts.forEach(el => el.getAnimations().forEach(a => {
    try { a.commitStyles(); } catch {}   // throws if the element isn't being rendered
    a.cancel();
  }));

  // 2. switch state — the transition target is now defined in CSS
  root.dataset.state = next;

  // 3. RELEASE the committed values, or they win. This step is the one everyone misses.
  requestAnimationFrame(() => parts.forEach(el =>
    COMMITTED.forEach(p => el.style.removeProperty(p))
  ));
}
```

**If the loop is a CSS animation, `cancel()` destroys it.** This is the trap behind the trap, and it is worse than the one above because nothing looks broken. A cancelled CSS-declared animation never comes back on its own: `getComputedStyle(el).animationName` still reports `cel-lean`, the rule is still matching, and `el.getAnimations()` returns **nothing at all**. So every continuous loop dies silently on the *first* state change and the character goes quietly rigid for the rest of the session — which reads as weak amplitude, and sends you off tuning numbers that were never running. Re-assert the declaration as part of the release:

```js
p.style.animation = "none";
void p.getBoundingClientRect();   // force a reflow so the change is observed
p.style.removeProperty("animation");
```

Drive the loops from WAAPI instead and the problem disappears, because then you own the animation objects and can simply restart them. Either is fine; mixing CSS loops with WAAPI cancellation and no re-assert is not.

`commitStyles()` writes the animation's current computed value onto the element's **inline style**, and `cancel()` then removes the animation without snapping back. That gets you the freeze — but inline styles beat a selector rule, so the frozen pose will now silently override whatever `[data-state="error"]` says, and your error tilt simply will not happen. Removing those properties on the next frame hands control back to CSS, the computed value changes from the committed pose to the state's pose, and the `transition` animates between them. That is the whole trick: **freeze, switch, release.**

Note that `getAnimations()` on the root does not reach descendants — pass `{ subtree: true }`, or query the specific groups as above, which is clearer about what it owns.

If you are not using WAAPI: read `getComputedStyle(el).transform`, set it inline, swap the animation class, then remove the inline value on the next frame. Same three beats.

## Secondary motion

Parts of a character that are not rigidly attached should **arrive late**. This is the cheapest way to make a form feel like it has mass and softness, and it costs one number.

- Same keyframes on the child, **60–120ms longer duration** or a small negative `animation-delay`. The hat follows the head and settles after it.
- **Lag in proportion to looseness.** A held prop lags most, a worn-loose accessory (a beret, a scarf) next, a tight one (glasses) barely at all, a cut-out marking not at all — it is painted on the surface.
- **Overshoot scales with lag.** Something that arrives late should go slightly too far and come back. Something rigid should not.
- Do not give secondary motion to the eyes. Eyes that lag behind the head read as unwell.

```css
.char__body      { animation: rock 2.6s ease-in-out infinite; }
.char__accessory { animation: rock 2.6s ease-in-out infinite; animation-delay: 90ms; }
```

## Weight, anticipation, gaze

**Weight lives in the asymmetry of the curve**, not in the duration. Heavy things take a long time to get going and stop hard; light things start instantly and take a long time to settle.

| | Start | Stop | Overshoot |
| --- | --- | --- | --- |
| Heavy (stone, ceramic, metal) | slow, long ease-in | abrupt | none |
| Mid (vinyl, clay) | moderate | moderate | 2–4% |
| Light (felt, jelly, paper) | instant | long settle | 5–10% |

**Anticipation** — a small counter-move before a big one. A 60ms dip before a `done` pop reads as a gathering of effort and makes the pop land harder. Use it on `done` and nothing else. Anticipation on a *status* character reads as hesitation, and hesitation is a claim about the system — never make that claim by accident.

**Gaze** — if the character has eyes and something is happening elsewhere on screen, look at it. Offset the eye group a few user units toward the active element and let it ease back when nothing is happening. It is the cheapest attention signal after blinking. Two rules: **clamp the offset hard** (3–5 units; more and it looks possessed), and **never track the cursor continuously** — it is a per-frame cost, and a character whose eyes follow your mouse around a work tool is unsettling rather than charming.

## Interruption

States do not arrive in a tidy order. `thinking` → `error` halfway through a rock. `done` → `thinking` 200ms later because another job landed. Four rules:

- **Transitions, not keyframes, for anything interruptible.** A CSS transition retargets smoothly from wherever it is; a keyframe animation restarts from 0%.
- **The latest state wins. Never queue.** A character that plays a backlog of state animations is reporting the past.
- **`done` must be cancellable mid-play.** It is the only one-shot likely to be interrupted, because work often arrives in bursts.
- **`error` outranks everything.** It is a pose, not an animation, and nothing should be able to animate over it.

## The always-on cost

A character is on screen the whole session, so its animation is a permanent bill — and this is the one thing in this file that will get flagged in a performance review rather than a design review.

- **Loop only `transform` and `opacity`.** They stay on the compositor thread. Anything else re-runs layout or paint every frame, forever.
- **Never loop a `filter`.** A blurred sheen or a soft shadow is re-rasterised on every frame and is by far the most expensive idle you can write. If the idle *is* a moving blur, bake it to a sprite or move a pre-blurred element instead of animating the blur itself.
- **Pause when off-screen** with an `IntersectionObserver`, and **pause when the tab is hidden** via `visibilitychange`. A character quietly burning a laptop battery in a background tab is a bug, not a flourish.
- **Below 32px, drop the loop entirely.** The motion is not legible at that size and you pay for it at full price anyway. Render the state pose static and let the face carry it.

```js
new IntersectionObserver(
  ([e]) => el.style.animationPlayState = e.isIntersecting ? "running" : "paused"
).observe(el);
document.addEventListener("visibilitychange", () => {
  el.style.animationPlayState = document.hidden ? "paused" : "running";
});
```

## Reduced motion

The usual advice — turn it off — is wrong here, because the character is carrying status. What to do instead:

- **Keep the character.** Removing it removes information.
- **Kill every loop**, including the blink.
- **Keep the pose that distinguishes the state.** The `error` tilt is not decoration; it is the message. Hold it statically.
- **Cross-fade between states** rather than cutting, at 120ms or so. A cross-fade is not vestibular motion and it keeps the state change from being jarring.
- Gate it once, high up, rather than per-animation — then it cannot be forgotten on the next state you add.

## Verifying it

Character motion is loops and state changes over time, so a screenshot proves almost nothing. Two things to know before you automate the check:

- **Capture in a real browser.** Headless Chrome's `--virtual-time-budget` is the obvious way to get deterministic animation frames, and for character work it is a trap. Two things break under it:
  - **CSS keyframe animations advance; CSS transitions do not.** Transitions read as stuck at their start value, so every state pose renders *one state behind*. If your state poses are transition-driven — and they should be, because transitions are what makes them interruptible — virtual time cannot render them at all.
  - **`requestAnimationFrame` callbacks do not fire reliably**, so anything gated on a rAF, the release step above most of all, looks broken in the capture while being perfectly fine in the browser.

  Both failures look exactly like real bugs. If a captured frame disagrees with what you expect, check the real thing before you "fix" the code.
- **Assert on computed style, not on pixels.** `getComputedStyle(el).rotate` and `.scale`, plus `el.getAttribute("style")` to confirm the committed inline values were actually released, will tell you in one line what a screenshot only hints at.

The states worth checking explicitly: every loop → every other state (the jolt lives there), `done` interrupted mid-play, and `error` arriving from each of the others.

## Timing table

Starting numbers, to be adjusted by the material's weight:

| What | Duration | Shape |
| --- | --- | --- |
| Blink | 80–140ms | asymmetric, close faster than open |
| Blink interval | 2–6s | randomised, double ~1 in 6 |
| Breathing idle | 3–4s | `ease-in-out`, infinite, 1–2% |
| Sway idle | 5–6s | coprime with the breath |
| Thinking loop | 2–3s | constant amplitude, no peak |
| State transition | 250–400ms | from the committed pose |
| `done` one-shot | 400–600ms | optional 60ms anticipation dip |
| `error` | — | a pose, held, no animation |
| Enter on mount | 300–400ms | from `scale(.9)`, never from 0 |
| Exit | ~60% of enter | only on unmount, never on a state change |
