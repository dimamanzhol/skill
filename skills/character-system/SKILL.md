---
name: character-system
description: "Design a character system for an AI product — one locked form primitive, one committed material, a minimal face vocabulary, a state range, and a ground — then render it as a character bible, image-generation prompts, and animated SVG/React code. Use when someone wants a mascot, an assistant avatar, a brand character, a cast of characters, or a visual persona for an AI product; when a cast needs to stay consistent across renders; or when an existing mascot reads as generic, stocky, or AI-generated. Triggers on: mascot, mascot design, character design, character system, brand character, brand mascot, assistant avatar, AI persona, AI assistant persona, product character, cast of characters, creature, blob character, agent avatar, bot avatar, chatbot face, companion character, illustration system, expression sheet, turnaround, character bible, character style guide, idle animation, thinking state, loading character, our mascot looks generic, mascot feels off, design a mascot, draw a mascot, give the product a face, what should our AI look like, Grok Bot, ChatGPT dots, Claude characters, Duolingo owl, Clippy."
---

# Character Systems for AI Products

A character for an AI product is not a drawing. It is a small set of locked decisions that generate every drawing afterward — and the reason most AI mascots fail is that someone drew first and derived the rules later, so there were no rules, so the second drawing didn't match the first.

Look at the genre's good examples and the same structure appears every time: **one form primitive, one committed material, one minimal face vocabulary.** The cast comes from swapping color and accessory, never from redrawing the body. ChatGPT's "dots" are four identical plush blobs distinguished by a beret, googly eyes, glasses, and sunglasses. Grok Bot's cast is one rounded-square bean in teal, purple, and red with two capsule slits cut into each. Anthropic's print-grain blob is a hooded silhouette on a flat red field. A terracotta app icon does it with no face at all — an irregular clay circle and one continuous brush loop — which proves eyes were never the requirement.

So the deliverable is the decision set. The pictures are downstream.

## What to read, and when

This file is the whole method: the stages and the seven locks are below and you do not need another file to run them. Everything else is depth for one stage, so **open a file when you reach the stage that needs it, and not before.** Loading all five costs about a thousand lines to answer a question that lives in one of them.

| Open | When |
| --- | --- |
| [material.md](material.md) | Stage 1, at lock 2 — choosing the material, or a character reads too raw or too soft. The catalogue, what each one costs, and the hardness dial. |
| [prompts.md](prompts.md) | Stage 2 only, once the bible exists. Prompt structure, the single-image consistency trick, negatives. |
| [code.md](code.md) | Stage 3 only. SVG layering, the state machine, level-of-detail, the React shape. |
| [motion.md](motion.md) | Stage 3, once something moves — or Stage 1 lock 6 if motion is load-bearing. Blinking, loop exits, secondary motion, interruption, the always-on cost. |
| [example.md](example.md) | **Rarely.** One invented product taken brief-to-code, including what three renders corrected. Read it if you want to see a finished bible's shape, or if the user asks for a worked example — not as reference while working, it is long and it is one specific character. |

## How to run it

**Never open with a drawing, a render, or a palette.** Open with the brief, then the three directions, then the locks. Rendering is the expensive step and the irreversible one; do it last.

**Stage 0 — Brief.** Three questions, and resist asking more:

1. What does the product actually do? (not the category — the verb)
2. What is the character's *job in the interface*: presence (it's always there), status (it tells you what's happening), wayfinding (it marks where you are), or marketing (it lives on the site and the app icon and nowhere else)? The job determines everything downstream — a status character needs a legible error state and can skip a hero pose; a marketing character is the reverse.
3. What must it never imply? Every character makes a claim about the system's competence, autonomy, and feelings. Name the claim you can't afford.

**Say it immediately if the request is made of the five words.** Most people open with some version of "I want a cute / friendly / modern / playful / approachable mascot", and those five words are the subject of the description test at the bottom of this file — they are what people say when nothing has been locked. Waiting until the final audit to mention that is useless, because by then the work is done. Say it at the moment it is said, in one line and without lecturing: *cute is an outcome of the form, not an input — tell me what the product does and I'll get you there.* Then ask question 1.

The same goes for a request that arrives as a species or an object — "a fox", "a robot", "an owl". That is a form, not a brief, and lock 1 will almost certainly argue with it. Do not open by arguing. Take the brief first; the form question has its own branch in Stage 1 below.

**Stage 1 — Three directions, then lock.** Write three one-sentence directions that differ *at the material level*, not the cute level. "A felted blob", "a glazed ceramic pot", and "a stipple-print silhouette" are three directions. "A friendly blob", "a cute blob", and "a modern blob" are one direction and no decisions. Give each a sentence on what it says about the product and what it costs to produce. Get a pick, then write the seven locks as a bible.

**Stage 1b — When the form is already decided.** Sometimes it is not open: the user arrives with a dragon, a brand character they already own, a mascot a founder is attached to, or something their illustrator drew. The three directions above assume a blank page, and running them anyway reads as refusing the request. Branch instead:

- **Say once what the locked form will cost**, concretely rather than as principle. A dragon has wings, legs and a tail, so it breaks lock 1's limb budget; its identity lives in a snout and a brow, which lock 3 does not allow; at 24px it is a lump. One sentence each, no sermon.
- **Then offer the trade honestly.** Either (a) keep the species and relax the locks — accept that it will not be a *system*, that the cast will have to be redrawn member by member, and that it will not survive being shrunk to an avatar; or (b) keep the locks and let the species become a reference rather than a depiction, which is how every good example in this genre handles its own origin.
- **If they keep the form, run the remaining locks on it anyway.** Material, face vocabulary, expression range, accessory zones, motion and ground are all still worth locking, and they are where most of the value was. Vary **material** across the three directions instead of form.
- **The decision is the user's, and they get to make it once.** Having said the cost, do not re-litigate it at every later step.

**When the product is not an AI product.** Lock 4's range — idle, listening, thinking, done, error — is a model of a system doing work, and a coffee shop has none of those. Do not force it. Replace the state sheet with the two or three moments the character actually has (a greeting, a wait, a thank-you), keep every other lock unchanged, and skip [motion.md](motion.md)'s loop and interruption machinery, which exists for status characters. A marketing character with no states is a legitimate output of this skill; an invented `thinking` state is not.

**Stage 2 — Prompts** (optional, [prompts.md](prompts.md)). Derived from the bible, not invented fresh.

**Stage 3 — Code** (optional, [code.md](code.md)). Also derived from the bible.

Stages 2 and 3 are siblings, not a sequence — both read from stage 1. If the bible changes, both regenerate.

**This skill does not draw.** Stage 2 produces prompt text to paste into an image model; Stage 3 produces SVG and CSS, which reaches the flat, glossy and ink materials and not the ones [material.md](material.md) marks as needing rendered frames. Say so early if the user is expecting a picture to come out of the conversation — it is a cheap sentence at the start and an expensive disappointment at the end.

**Put it on a screen before calling the bible done.** A character bible is a set of claims about how something looks, and some of them are wrong in ways that are invisible in prose — a lid that seats in the description and floats in the render, a tilt that reads at 11° and not at 5°, an asymmetry that disappears at 24px. Render the states, look at them side by side at full size and at icon size, and push the corrections back into stage 1 rather than patching them downstream.

## The seven locks

### 1. Form primitive

One silhouette the entire cast shares, written as a **constructive recipe** rather than an adjective. Not "a soft blob" — "a superellipse 1:1.15 tall, the top 40% a single unbroken dome, the bottom edge flat where it meets the ground." The test: hand the sentence to someone who has not seen your drawing and have them redraw it. If they can't, you have a drawing, not a primitive.

Decide the **limb budget once**: 0, 2, or 4 appendages, for the whole cast forever. Zero is cheapest to animate and hardest to make expressive — compensate with squash and with the one asymmetry below. Four buys you gesture and costs you a rig.

**Take the form from a formal source, not from the product's subject matter.** The reflex is to draw an icon of the domain — a kettle for a restaurant tool, a shield for a security product, a brain for anything with a model in it. That is a pun, and a pun is a joke you have to keep telling. None of the good examples in this genre do it: the dots are not chat bubbles, the beans are not rockets, the print-grain blob is not a book. Go to shape language instead — a sculpture, a mathematical solid, a tool whose *behaviour* rhymes with the product, a worn object, a typographic gesture — and let the connection be structural rather than illustrative. A form borrowed from something that *behaves* the way your product should behave will carry meaning for years; a form that merely depicts the domain stops being interesting the second the viewer gets it.

Give the primitive **exactly one asymmetry** — a spout, a cowlick, a single stubby arm, a tapered tail on a brush stroke. Symmetrical silhouettes are interchangeable at small sizes; the one odd feature is what makes it recognizable at 24px, and recognizability at 24px is the whole job for an in-product character.

> **Wrong:** each cast member gets its own silhouette so they feel like individuals. You now have five characters and no system, and the fifth one won't match when a different person draws it.
> **Right:** one body, five colors, five accessories. Individuality is carried by the cheapest axis available.

### 2. Material

**The single most differentiating decision, and the one people skip.** Generic mascots are generic almost entirely because they have no material — they are flat vector fills or a soft gradient, which is the absence of a choice rather than a choice. Print grain, brush ink, wet glaze, felted fiber, and enamel are materials. "A nice gradient" is not.

Pick **one** and commit. A character that is both glossy plastic and fuzzy felt is a render test, not a character. The material also sets the physics: ceramic doesn't deform, felt doesn't reflect, ink doesn't have a highlight. Half your motion decisions fall out of this lock for free.

**The hardness dial.** "It feels too raw" and "it feels too soft" are the most common notes a character gets, and they are almost never fixed by changing the material — they are fixed by six levers that cut across every material. Reach for them in this order:

| Lever | Reads hard / raw | Reads soft |
| --- | --- | --- |
| **Silhouette** | corners, straight edges, points | unbroken convex curves, nothing you could cut yourself on |
| **Light** | a small hard-edged specular | a *broad* low-contrast gradient across most of the form |
| **Value & chroma** | dark, desaturated, mineral | bright and saturated |
| **Outline** | perfectly regular, mechanical | slightly irregular and asymmetric — evidence of a hand |
| **Deformation** | rigid; motion only in joints | squash and stretch, volume-preserving |
| **Contact shadow** | tight, dark, close | wide, diffuse, light |

Silhouette and light do most of it. A single straight edge or one hard specular hotspot will keep a form reading raw no matter how friendly its colour is — which is why the same rounded body can read as a river stone or as a plush toy depending on nothing but those two.

Choose for the viewing condition, not for taste. Gloss survives distance and glare because a hard specular is a bright mark that holds at any size; fine print grain dies below about 64px and should not be the choice for a 20px status chip. The full catalogue — what each material says, what it costs, how to prompt it, how to fake it in CSS — is in [material.md](material.md).

### 3. Face vocabulary

The minimum marks that read as alive. Pick an eye shape and a mouth rule, write them down, and add nothing else:

- **Two solid dots and one curve.** The default, and still the strongest. Solid — no pupil, no iris, no specular dot.
- **Two vertical capsule slits, cut out rather than drawn.** The eye is a hole revealing a lighter layer underneath, which is why Grok Bot's beans read as objects rather than as faces painted on objects.
- **Googly eyes** — a physical appliqué sitting *on* the surface, with its own shadow. Reads as craft, not as anatomy.
- **Closed arcs (`^ ^`)** — content, unbothered, and immune to the dead-eyed problem.
- **No face at all.** A gesture, a stroke, a form. Legitimate and underused.

**Never add a nose, eyebrows, eyelids-with-lashes, or a pupil with a specular highlight.** The pupil highlight in particular is the fastest route to 2008 corporate vector art, and it does real harm: it promises a face capable of emoting, which yours is not going to deliver.

**When a face is the wrong call.** When the character must live at 16–24px in the interface, where a face becomes mud and the silhouette is all you get. When the product is in a domain where a face patronizes — a character that grins at you about your bank balance, your test results, or a security incident. When the character stands next to real human avatars, where it competes. And when the character represents the *system* rather than an *agent*: a face implies a mind, and that is a claim about your product, not a style choice.

> The face is an honesty surface. If it smiles while the model is failing, you have built something that lies in the interface. Either the expression range covers error and uncertainty properly, or there should be no face.

### 4. Expression range

AI product states are not emotions. Build the sheet from what the system is actually doing:

| State | What it must do |
| --- | --- |
| **idle** | Present, not working. Must survive being looked at for an hour. |
| **listening** | The user is typing or speaking. Attention, without eagerness. |
| **thinking** | The one people see most. Must loop indefinitely with **no climax** — any beat implying "almost done" becomes a lie on long runs. |
| **done** | Fires once, then returns to idle. Resolution, not celebration. |
| **error / can't** | Legible, and distinct from thinking at a glance. |

Five is enough. Add **blocked / needs-permission** only if your product really stops and waits for a human — it is a different message from error and users act on it differently.

The rule that keeps the system a system: **every state must be reachable from the locked primitive by changing at most two things.** Eye shape plus a transform. Lid angle plus a color. If a state needs new path data for the body, it isn't a state, it's a second character.

Make **error a silhouette change, not a color change.** Color fails at distance, in bright light, for colorblind users, and in grayscale screenshots. A tilt, a slump, a persistent gap in the form reads across a room and survives all four.

### 5. Accessory and prop system

The cheap axis, and the reason a cast of eight doesn't cost eight designs.

- Define **attachment zones** once — crown, face-plane, held-at-front — and let nothing attach anywhere else.
- **One accessory per cast member.** Two is a costume.
- Accessories should say the member's **job**, not its personality. The one that reads documents wears the glasses. "The sassy one" is not a job.
- **Worn and held are different classes.** Worn objects state identity and persist. Held objects state activity and come and go — the blob cradling a glass sphere is *doing* something, and putting that sphere down is a state change you get for free.

### 6. Motion personality

The character's mass was already decided when you locked the material — ceramic, felt, jelly, and ink do not accelerate alike. Motion makes that mass visible. The locks below are the ones that belong in the bible; **[motion.md](motion.md) carries the craft** — blinking, breaking the loop's period, leaving a running loop without a jolt, secondary motion, interruption, and the performance cost of a character that animates all session.

- **An idle that shows it is alive is mandatory, and it must be nearly invisible.** Which channel carries it comes from the material, and getting this wrong is how a character ends up fighting its own substance:
  - *Soft* (felt, jelly, clay) → **breath.** Scale 1 → 1.012–1.02 over 3–4s, `ease-in-out`, infinite.
  - *Rigid* (ceramic, enamel, stone, metal) → **weight shift.** A sub-degree sway, or a drift in a joint. A rock that breathes is a beach ball.
  - *Polished and rigid* → **let the light move instead of the body.** Drift the specular slowly across the surface while the form stays perfectly still. This is the most restrained idle available and the most convincing, because stillness is what the material actually does.
  - *Mark-based* (ink, flat riso) → **almost nothing.** A barely-there drift in the stroke weight, or no idle at all; a flat mark that wobbles looks like a bug.

  If you can perceive the idle as motion rather than as life, it is too big.
- **Squash preserves volume.** Wider means shorter, by the same ratio. Uniform scaling is a balloon inflating, not a body moving.
- **Deform where the material allows.** Felt squashes; glazed ceramic does not. If the material is rigid, put the motion in a joint, a gap, or a tilt instead — the constraint produces better motion than freedom does.
- **Enter once.** The character arrives when the surface mounts and then stays. Re-animating the entrance on every re-render is the most common implementation bug in this genre.
- **The thinking loop has no peak.** Constant amplitude, constant period, no accent beat.
- **Decide whether it blinks.** A blink is the cheapest aliveness signal there is and it doubles as cover for the face cutting between states — but it commits you to a randomised timer, and it must be suppressed in `error`, where a stuck character should stare. See [motion.md](motion.md).
- **Reduced motion keeps the character and kills the loop.** Swap to a static pose that still distinguishes the state. Never remove the character — it carries status.

### 7. Ground

The backdrop is part of the character, and dropping the character onto whatever background the page happens to have is what turns a system into a sticker.

- **Flat saturated field** — makes the character a subject, poster-like, editorial. Carries weight and a little melancholy.
- **Black void** — makes it glow, and therefore requires it to be emissive or dramatically lit. Commits you to a dark surface everywhere it appears.
- **Light grid** — makes it a UI object sitting inside a system, among chips and bubbles. The most "product" of the four.
- **Paper or cream** — makes it an artifact, something printed or made.

Lock one. Then write the **one rule for arbitrary surfaces**: the contact shadow spec, the minimum clear space, and the single approved alternate ground. A character with no ground rule will be pasted onto a gradient hero within the week.

## Anti-slop guardrails

The tells that mark an AI mascot as generated rather than designed:

- **The default gradient orb** — a purple-to-indigo radial blob with a soft glow, floating, no material, no silhouette. This is the single most-shipped AI mascot in existence and it is the same one every time.
- **Pixar eyes** — large irises, white specular dot, eyelid, eyebrow. Stock vector at best, uncanny at worst, because it promises emotional range the character cannot deliver.
- **An emoji face.** Borrowed geometry is not a face vocabulary.
- **The sparkle as the thinking state.** ✨ means "AI" to nobody anymore; it means "we didn't design a loading state."
- **Headphones, antennae, or a visor to signal "robot".** A costume standing in for a form decision.
- **The three-quarter studio render, drop shadow, white seamless.** The stock-3D look, and it has no ground rule.
- **Chromatic glow on everything.** Where a rainbow bloom works, it is applied to exactly one element while the characters stay matte. Applied to all of it, it's a filter.
- **A cast that differs by shape.** Diagnostic of no system at all.
- **The domain pun.** A kettle for a kitchen product, a shield for security, an owl for anything educational, a brain, a robot. Depiction standing in for a form decision — see lock 1.
- **Purple.** Not banned — *unchosen* purple is. Derive the hue from the product's domain or its existing brand, the way every good example in this genre does.

Two tests before anything ships:

1. **The 24px test.** Render the character at 24px, in grayscale, next to the other candidates. If you can't tell them apart, the design is living in detail it will never be given room to show. Note the trade it exposes: **protrusions survive shrinking and smooth convex forms do not**, so a sculptural, abstract primitive has a higher size floor than a chunkier one. Measure it rather than guessing — render both and look. If the form you want fails here and you still want it, the system owes itself a simplified icon-weight variant with the asymmetry exaggerated — and that variant is part of the system, not an afterthought.
2. **The description test.** State the form and the material in one sentence each, without using *cute*, *friendly*, *modern*, *playful*, or *approachable*. Those five words are what people say when nothing has been locked. If you can't write the sentences, go back to the locks.
