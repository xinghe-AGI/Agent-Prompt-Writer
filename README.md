# AI Prompt Writer

---
参考资料：
- [nidhinjs/prompt-master](https://github.com/nidhinjs/prompt-master)
- [yzfly/structured-prompt-skill](https://github.com/yzfly/structured-prompt-skill)
- [ckelsoe/prompt-architect](https://github.com/ckelsoe/prompt-architect)
- [Principled Instructions Are All You Need for Questioning LLaMA-1/2, GPT-3.5/4](https://arxiv.org/pdf/2312.16171)
- [[26条有效提示词技巧]]
- [[结构化 Prompt 编写指南和最佳实践]]
---

## 这个 skill 解决什么问题？

AI Prompt Writer 用来解决“我知道自己想让 AI 做什么，但不知道 prompt 应该怎么写”的问题。

这次重构后，它不再追求覆盖所有 prompt 框架，而是以结构化 Prompt 写法为核心：把输入、任务、约束、输出格式、异常处理和样例写清楚，让 prompt 可以稳定复用。

它主要适合：

- 结构化输出 prompt。
- JSON 信息抽取 prompt。
- Agent 执行 prompt。
- 自动化工作流 prompt。
- 长期复用的角色 prompt。
- 把粗略需求整理成可复制的一次性 prompt。

## 核心用法

使用时，直接给出你的需求。如果 prompt 要贴到某个具体工具里，也一起说明。

例如：

```text
帮我写一个结构化 prompt。
输入是账单 OCR 文本，输出必须是可解析 JSON。
字段包括金额、日期、商户、类别、备注。
无法识别的字段返回 null。
```

```text
帮我把下面需求写成给 Claude Code 用的 prompt：
只允许修改 src/components/UserCard.tsx，修复头像加载失败时的兜底显示。
```

```text
优化下面这段 prompt，让它输出更稳定：

[已有 prompt]
```

## 最小工作流

| 步骤 | 要解决的问题 |
|---|---|
| 判断用途 | 一次性任务、结构化输出、Agent 执行、工作流、长期角色 |
| 补齐输入输出 | 输入来自哪里，最终要输出什么 |
| 写清约束 | 字段、类型、枚举、缺失值、禁止项、异常输入 |
| 组织工作流 | 复杂任务拆成可执行步骤 |
| 给出样例 | 至少覆盖正常情况，生产级任务补异常样例 |
| 自检 | 检查输出形态、字段契约、缺失值和来源约束 |

## 文件结构

```text
AI Prompt Writer/
  SKILL.md
  README.md
  agents/
    openai.yaml
  references/
    prompt-method.md
```

## 方法基准

这个 skill 以 NanGePlus/PromptsCollection 的结构化 Prompt 方法为基准，核心是统一骨架和 9 条硬标准。

统一骨架：

| 部分 | 作用 |
|---|---|
| 角色 | 定义模型是谁，负责什么，不负责什么 |
| 背景 | 说明输入来源、使用场景和易错点 |
| 简介 | 记录作者、版本、语言和 description |
| 任务 | 写清模型要完成的动作 |
| 约束条件 | 写死输出形态、字段限制、禁用项和异常处理 |
| 技能 | 说明识别、判断、归一化、分类等能力 |
| 工作流 | 把复杂任务拆成步骤 |
| 输出格式 | 定义字段、类型、枚举、顺序 |
| 输入模板 | 告诉用户应该提供什么输入 |
| 输出样例 | 对齐正常和异常输出 |
| 初始化 | 简短告诉用户如何开始 |

9 条硬标准：

1. 简介必须可追踪版本变化。
2. 输出形态只能有一个。
3. 字段契约要全量闭环。
4. 缺失值策略要统一且可执行。
5. 枚举值要可列举、可校验。
6. 字段来源约束是防幻觉关键。
7. 规则写法用“条件 + 结果”。
8. 样例要覆盖主流程和异常流程。
9. 初始化要短，但必须能直接开工。

论文 26 条原则只作为辅助检查，不再作为复杂模块。默认只吸收这些稳定原则：指令具体、信息充分、分隔输入、明确受众、使用示例、正向表达、必要时先澄清、复杂任务拆步骤。

## 这个项目副本和全局 skill 的关系

当前目录是项目内副本，用来学习、查阅、修改和沉淀 Prompt Engineering 相关 skill 思路。

全局可运行版本位于：

```text
C:\Users\admin\.codex\skills\ai-prompt-writer
```

修改本项目副本不会自动影响全局版本。只有当你明确要求“同步回全局 skill”时，才会把这里的改动复制到全局目录，并按治理规则更新 skill inventory 和 ledger。
