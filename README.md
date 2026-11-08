# skill

Agent skills by [Dinmukhamed Amanzhol](https://www.dinmukhamed.me).

```sh
npx skills add dimamanzhol/skill
```

## character-system

Designs a character system for an AI product. The genre of Grok Bot, ChatGPT's
dots, and Anthropic's print-grain blob.

A character is not a drawing. It is a small set of locked decisions that
generate every drawing afterward, and most AI mascots fail because somebody
drew first and derived the rules later. This skill locks eight: idiom, form,
material, face, states, accessories, motion, ground. Then it writes the bible,
the image prompts, and the SVG.

Idiom comes first because it is the one decision that constrains the rest. A
photograph of a felt character and a flat two-colour print of the same character
are different deliverables with different costs, and only one of them can be
animated in CSS. Skip the decision and you have not avoided it, you have
defaulted it.

| File | What is in it |
| --- | --- |
| `SKILL.md` | The eight decisions, in locking order. |
| `idiom.md` | Six registers: rendered, flat, line, typographic, pixel, collage. Two cannot be generated, two cannot be coded. |
| `material.md` | Ten materials and what each one costs. Print grain dies below 64px. |
| `motion.md` | Blinking, idle loops that never repeat, and why `cancel()` destroys a CSS animation. |
| `prompts.md` | Prompt structure for image models, and the grid that stops a character drifting between renders. |
| `code.md` | SVG layering, the state machine, and what to drop at 24px. |
| `example.md` | One product taken brief to code, including what three renders corrected. |

It argues hardest about the form. Ask it for a kettle and it will talk you out
of one.

Three characters built with it are running at
[dinmukhamed.me/craft](https://www.dinmukhamed.me/craft).

## License

MIT
