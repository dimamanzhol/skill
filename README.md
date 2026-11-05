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
drew first and derived the rules later. This skill locks seven: form, material,
face, states, accessories, motion, ground. Then it writes the bible, the image
prompts, and the SVG.

| File | What is in it |
| --- | --- |
| `SKILL.md` | The seven decisions, in locking order. |
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
