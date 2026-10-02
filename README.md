# 中文提示词（Prompt Optimizer-CN）

**简体中文 | [English](README_EN.md)**

> 把口语化需求蒸馏成**最省 tokens** 的高质量中文提示词 —— 开箱即用的 Claude Code Skill

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet.svg)](SKILL.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

中文用户的痛点：需求靠口语描述，又冗长又模糊，直接丢给 AI 要么浪费 tokens，要么答非所问。本 Skill 把需求**拆解 → 选框架 → 中文专项压缩**，输出可直接复制的高质量提示词，并给出 token 账。

## 效果对比

**原文（口语，~120 tok）**：

> 那个，帮我写个周报呗，这周的话干了挺多活的，主要就是项目推进了不少，然后大大小小的会开了好几个，bug 也改了特别多，烦死了，我们老板每周都要看这个，你帮我写得正式一点、专业一点，但是也别写太长了，他没耐心看，哦对了数据别瞎编啊，改了多少写多少

**压缩稿（~65 tok，省 ~46%）**：

```
写本周周报：读者=直属上级；正式；≤300字。
结构：进展｜数据｜风险｜下周计划。
素材：项目A联调完成；修bug×3（支付回调）；评审会×2。
禁区：不虚构数据，数字照实。
```

目标更清晰、约束更硬、token 减半 —— 一次通过率反而更高。更多示例见 [examples/](examples/)。

## 功能特性

- **三步蒸馏**：拆解六要素 → 推荐最优执行框架（单轮/subagent/workflow/skill/脚本）→ 中文专项压缩
- **全平台覆盖**：Claude Code（SKILL.md）、Codex/Cursor（AGENTS.md）、ChatGPT/豆包/通义千问/DeepSeek 等（通用系统提示词粘贴）——见[各平台接入指南](docs/各平台接入指南.md)
- **三档优化**：轻（澄清补全）/ 中（量化+压缩，推荐）/ 狠（极限精简）
- **迭代微调**：在旧稿上最小精准修改，不重写全文，`{{变量}}` 原样保留
- **中文专项手法**：电报体、术语英文混用、四字格、符号化、文言输出约束 —— 每条标注压缩率与风险
- **诚实标注**：压缩率区分「实测 / 社区共识 / 传言」，不夸大（见 [docs/中文token压缩原理.md](docs/中文token压缩原理.md)）

## 快速开始

| 平台 | 用法 |
|---|---|
| Claude Code | `mkdir -p ~/.claude/skills/prompt-optimizer-cn && curl -o ~/.claude/skills/prompt-optimizer-cn/SKILL.md https://raw.githubusercontent.com/xyzln/prompt-optimizer-cn/main/SKILL.md`，说「优化提示词：…」触发 |
| Codex / Cursor / Aider | 复制 [AGENTS.md](AGENTS.md) 到项目根目录 |
| ChatGPT | 设置 → 个性化 → 自定义指令，粘贴 [prompts/system-prompt.md](prompts/system-prompt.md) |
| Claude 网页版 | Projects → 自定义指令，粘贴同上 |
| 豆包 / 通义千问 | 创建智能体 → 设定/提示词粘贴同上 |
| DeepSeek | API 作 system prompt；或新对话首条粘贴同上 |
| WorkBuddy 等 | 首条消息粘贴同上 + 「此后按以上规则处理我的需求」 |

分平台详细步骤：[docs/各平台接入指南.md](docs/各平台接入指南.md)

## 中文压缩手法速查

| 手法 | 压缩率 | 风险 | 场景 |
|---|---|---|---|
| 电报体：删虚词（的/了/请/我想） | 10~25% | 低 | **通用首选** |
| 技术术语保留英文 | 省且稳 | 低 | 技术 prompt |
| 四字格/成语缩略 | 中 | 歧义 | 通用描述，硬约束禁用 |
| 符号化（→ ∵ ｜） | 中 | 中 | 流程/逻辑 |
| 文言文 | 仅输出端约束（省输出 30%+） | 中 | 长输出场景 |

⚠️ **输入端不要文言化**：BPE 按词元合并不按字省，歧义高、约束易丢——省不了多少还降可靠性。

**压缩禁区**：否定词「不/别/禁止」· 数字/单位/版本号 · `path:line` 与标识符原文 · 结构标记。宁多 10 token，不丢一条硬约束。

## 文档

- [SKILL.md](SKILL.md) — 完整 Skill 定义（Claude Code 核心交付物）
- [AGENTS.md](AGENTS.md) — Codex / Cursor / Aider 等 AGENTS.md 生态载体
- [prompts/system-prompt.md](prompts/system-prompt.md) — 通用系统提示词（ChatGPT/豆包/千问/DeepSeek 等粘贴用）
- [docs/各平台接入指南.md](docs/各平台接入指南.md) — 分平台安装步骤
- [docs/中文token压缩原理.md](docs/中文token压缩原理.md) — 汇率实测、手法依据、来源与局限
- [examples/](examples/) — 完整前后对照示例

## 局限

- Claude 中文 token 汇率无官方实测数据（社区估算 1~1.5/字），token 账为估算值
- 压缩率数据来自公开资料与社区共识，个体差异存在，建议对关键 prompt 自行 A/B
- 本工具优化**输入提示词**，不改变模型能力上限

## 贡献

欢迎 PR：新的中文压缩手法（请附实测数据）、更多示例、各模型 tokenizer 实测补充。见 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 参考与致谢

方法论汲取自：[linshenkx/prompt-optimizer](https://github.com/linshenkx/prompt-optimizer) · [LangGPT](https://github.com/langgptai/LangGPT) · [dair-ai/Prompt-Engineering-Guide](https://github.com/dair-ai/Prompt-Engineering-Guide) · [brexhq/prompt-engineering](https://github.com/brexhq/prompt-engineering) · [LLMLingua](https://github.com/microsoft/LLMLingua)（Microsoft, EMNLP 2023）

## License

[MIT](LICENSE) © 2026 Prompt Optimizer-CN contributors
