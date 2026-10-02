# Prompt Optimizer-CN（中文提示词）

**[简体中文](README.md) | English**

> Distill colloquial requirements into **token-efficient**, high-quality Chinese prompts — a ready-to-use Claude Code Skill

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet.svg)](SKILL.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

Chinese-speaking users often feed LLMs verbose, vague, colloquial requirements — wasting tokens and getting off-target answers. This skill runs a 3-step pipeline — **decompose → pick framework → Chinese-specific compression** — and outputs a copy-paste-ready prompt plus a token budget.

## Before / After

**Raw input (colloquial, ~120 tok)**:

> Um, help me write my weekly report — did a lot this week, mainly pushed the project forward, attended a bunch of meetings big and small, fixed tons of bugs, so annoying — my boss reads it every week, make it formal and professional but not too long, he has no patience — oh and don't make up data, write exactly what was done

**Distilled (~65 tok, ~46% saved)**:

```
写本周周报：读者=直属上级；正式；≤300字。
结构：进展｜数据｜风险｜下周计划。
素材：项目A联调完成；修bug×3（支付回调）；评审会×2。
禁区：不虚构数据，数字照实。
```

Clearer goal, harder constraints, half the tokens — and a *higher* first-pass success rate. More in [examples/](examples/).

## Features

- **3-step distillation**: six-element decomposition → optimal execution framework (single-turn / subagent / workflow / skill / script) → Chinese-specific compression
- **Works everywhere**: Claude Code (SKILL.md), Codex/Cursor/Aider (AGENTS.md), ChatGPT/Doubao/Qwen/DeepSeek and any agent (paste [prompts/system-prompt.md](prompts/system-prompt.md)) — see [platform guide](docs/各平台接入指南.md) (Chinese)
- **3 optimization levels**: light (clarify & fill gaps) / medium (quantify + compress, recommended) / aggressive (minimal viable prompt)
- **Iterative refinement**: minimal precise edits on previous drafts, `{{variables}}` preserved verbatim
- **Chinese-specific techniques**: telegraphic style, English term retention, four-character idioms, symbolic notation, classical-Chinese output constraints — each annotated with compression rate and risk
- **Honest evidence grading**: every claim marked [measured] / [official] / [community consensus] / [rumor] — see [docs/中文token压缩原理.md](docs/中文token压缩原理.md) (Chinese)

## Quick Start

| Platform | How |
|---|---|
| Claude Code | `mkdir -p ~/.claude/skills/prompt-optimizer-cn && curl -o ~/.claude/skills/prompt-optimizer-cn/SKILL.md https://raw.githubusercontent.com/xyzln/prompt-optimizer-cn/main/SKILL.md` |
| Codex / Cursor / Aider | Copy [AGENTS.md](AGENTS.md) into your project root |
| ChatGPT | Settings → Personalization → Custom Instructions → paste [prompts/system-prompt.md](prompts/system-prompt.md) |
| Claude web | Projects → Custom Instructions → paste same file |
| Doubao / Qwen | Create agent/bot → paste into persona/system prompt |
| DeepSeek | API `system` role; or paste as first message |
| Any other agent | Paste as first message + "follow these rules from now on" |

## Compression Techniques Cheat Sheet

| Technique | Savings | Risk | Best for |
|---|---|---|---|
| Telegraphic style (drop 的/了/请…) | 10~25% | Low | **Default choice** |
| Keep technical terms in English | Steady win | Low | Technical prompts |
| Four-character idioms | Medium | Ambiguity | Descriptions, never hard constraints |
| Symbolization (→ ∵ ｜) | Medium | Medium | Flows / logic |
| Classical Chinese | Output-side only (30%+ of output) | Medium | Long-output tasks |

⚠️ **Never write the input prompt in classical Chinese**: BPE merges tokens regardless of characters; ambiguity rises and fine-grained constraints get lost.

**Hard no-compress zone**: negations (不/别/禁止), numbers/units/versions, `path:line` and identifiers, structural markers. Better 10 extra tokens than one dropped constraint.

## Docs

- [SKILL.md](SKILL.md) — full skill definition (Claude Code core deliverable)
- [AGENTS.md](AGENTS.md) — Codex / Cursor / Aider carrier
- [prompts/system-prompt.md](prompts/system-prompt.md) — universal system prompt (paste into ChatGPT/Doubao/Qwen/DeepSeek…)
- [docs/各平台接入指南.md](docs/各平台接入指南.md) — per-platform setup guide (Chinese)
- [docs/中文token压缩原理.md](docs/中文token压缩原理.md) — token rates, evidence, sources (Chinese)
- [examples/](examples/) — full before/after walkthroughs (Chinese)

## Limitations

- No official Claude Chinese token rate (community estimate 1~1.5/char); token budgets are estimates
- Compression rates come from public sources and community consensus; A/B test critical prompts yourself
- This optimizes **input prompts**; it does not raise model capability

## Contributing

PRs welcome: new techniques (measured data required), more examples, tokenizer measurements. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Acknowledgements

Methodology distilled from: [linshenkx/prompt-optimizer](https://github.com/linshenkx/prompt-optimizer) · [LangGPT](https://github.com/langgptai/LangGPT) · [dair-ai/Prompt-Engineering-Guide](https://github.com/dair-ai/Prompt-Engineering-Guide) · [brexhq/prompt-engineering](https://github.com/brexhq/prompt-engineering) · [LLMLingua](https://github.com/microsoft/LLMLingua) (Microsoft, EMNLP 2023)

## License

[MIT](LICENSE) © 2026 Prompt Optimizer-CN contributors
