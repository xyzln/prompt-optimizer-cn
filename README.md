# 中文提示词（Prompt Optimizer-CN）

**简体中文 | [English](README_EN.md)**

> 把口语化需求蒸馏成**最省 tokens** 的高质量中文提示词 —— 开箱即用的 Claude Code Skill

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet.svg)](SKILL.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

中文用户的痛点：需求靠口语描述，又冗长又模糊，直接丢给 AI 要么浪费 tokens，要么答非所问。本 Skill 把需求**拆解 → 选框架 → 中文专项压缩**，输出可直接复制的高质量提示词，并给出 token 账。

## 效果对比

**原文（口语，~150 tok）**：

> 帮我看看这个项目吧，就是我现在这个代码跑起来特别慢，我也不知道为啥，你帮我优化一下，最好是快点，哦对了我用的是 Python，数据大概有几十万条，就是 pandas 处理的那个脚本，每天早上跑报表用，现在要等好几分钟，老板催了，能不能搞到十几秒这种水平啊，谢谢啦

**压缩稿（~75 tok，省 ~50%）**：

```
优化 ~/report.py（pandas，~50万行，每日报表）：运行 3min+ → 目标 <20s。
先 cProfile 定位瓶颈，再给优化 diff。
禁区：不改输出格式；pandas 2.x。
```

目标更清晰、约束更硬、token 减半 —— 一次通过率反而更高。更多示例见 [examples/](examples/)。

## 功能特性

- **三步蒸馏**：拆解六要素 → 推荐最优执行框架（单轮/subagent/workflow/skill/脚本）→ 中文专项压缩
- **三档优化**：轻（澄清补全）/ 中（量化+压缩，推荐）/ 狠（极限精简）
- **迭代微调**：在旧稿上最小精准修改，不重写全文，`{{变量}}` 原样保留
- **中文专项手法**：电报体、术语英文混用、四字格、符号化、文言输出约束 —— 每条标注压缩率与风险
- **诚实标注**：压缩率区分「实测 / 社区共识 / 传言」，不夸大（见 [docs/中文token压缩原理.md](docs/中文token压缩原理.md)）

## 快速开始

**Claude Code**：

```bash
mkdir -p ~/.claude/skills/prompt-optimizer-cn
curl -o ~/.claude/skills/prompt-optimizer-cn/SKILL.md \
  https://raw.githubusercontent.com/xyzln/prompt-optimizer-cn/main/SKILL.md
```

然后在对话中说「优化提示词：……」「拆解需求：……」即可触发。

**其他 Agent / 手动使用**：SKILL.md 本身就是完整方法论，直接把内容贴给任意 LLM 作为系统提示词同样有效。

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

- [SKILL.md](SKILL.md) — 完整 Skill 定义（核心交付物）
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
