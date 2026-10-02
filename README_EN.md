# Prompt Optimizer-CN（中文提示词）

**[简体中文](README.md) | English**

> Distill colloquial requirements into **token-efficient**, high-quality Chinese prompts — a ready-to-use Claude Code Skill

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet.svg)](SKILL.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

Chinese-speaking users often feed LLMs verbose, vague, colloquial requirements — wasting tokens and getting off-target answers. This skill runs a 3-step pipeline — **decompose → pick framework → Chinese-specific compression** — and outputs a copy-paste-ready prompt plus a token budget.

## Before / After

**Raw input (colloquial, ~150 tok)**:

> Help me look at this project, my code runs really slow and I don't know why, please optimize it, faster would be great, oh right it's Python, maybe a few hundred thousand rows, the pandas script, runs the daily report every morning, takes several minutes now, boss is pushing, can you get it to like ten seconds, thanks

**Distilled (~75 tok, ~50% saved)**:

```
优化 ~/report.py（pandas，~50万行，每日报表）：运行 3min+ → 目标 <20s。
先 cProfile 定位瓶颈，再给优化 diff。
禁区：不改输出格式；pandas 2.x。
```

Clearer goal, harder constraints, half the tokens — and a *higher* first-pass success rate. More in [examples/](examples/).

## Features

- **3-step distillation**: six-element decomposition → optimal execution framework (single-turn / subagent / workflow / skill / script) → Chinese-specific compression
- **3 optimization levels**: light (clarify & fill gaps) / medium (quantify + compress, recommended) / aggressive (minimal viable prompt)
- **Iterative refinement**: minimal precise edits on previous drafts, `{{variables}}` preserved verbatim
- **Chinese-specific techniques**: telegraphic style, English term retention, four-character idioms, symbolic notation, classical-Chinese output constraints — each annotated with compression rate and risk
- **Honest evidence grading**: every claim marked [measured] / [official] / [community consensus] / [rumor] — see [docs/中文token压缩原理.md](docs/中文token压缩原理.md) (Chinese)

## Quick Start

**Claude Code**:

```bash
mkdir -p ~/.claude/skills/prompt-optimizer-cn
curl -o ~/.claude/skills/prompt-optimizer-cn/SKILL.md \
  https://raw.githubusercontent.com/xyzln/prompt-optimizer-cn/main/SKILL.md
```

Then say "优化提示词：……" / "拆解需求：……" in conversation.

**Other agents / manual use**: SKILL.md is the complete methodology — paste it into any LLM as a system prompt.

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

- [SKILL.md](SKILL.md) — full skill definition (core deliverable, bilingual-ready)
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
