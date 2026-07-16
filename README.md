# AI Prompt Writer

AI Prompt Writer 是一套自适应 Prompt 编写架构。它把粗略需求或已有 Prompt，转换成符合统一规范、可直接复制使用的最终 Prompt，并附带可追踪的质量报告。

它只负责 Prompt 本身，不负责替用户执行 Prompt 中描述的普通任务。

## 解决的问题

- 不知道怎样把需求组织成清楚、稳定的 Prompt。
- 已有 Prompt 存在歧义、边界不明、输出不稳定或难以复用的问题。
- 需要为指定模型、编程工具、Agent 或自动化平台调整 Prompt。
- 需要为 JSON、表格、分类、抽取等任务建立严格输出契约。
- 需要给 Agent 写清允许范围、禁止动作、检查点和停止条件。
- 希望每次生成 Prompt 后都能看到具体的质量检查结果。

## 使用边界

适合：

- 从零编写 Prompt。
- 优化或规范化已有 Prompt。
- 适配目标工具。
- 诊断并重写 Prompt。

不适合：

- 直接写文章、改代码、总结资料或执行普通任务。
- 单独讲解 Prompt 理论、论文和框架。
- 编写项目研究或学习笔记。
- 编写图像、视频和音频生成 Prompt。

## 自适应架构

Skill 固定执行一套完整流程：

`范围判断 → 编写模式 → 需求契约 → 澄清门 → 类型路由 → 结构装配 → 质量门禁 → 标准交付`

“完整”指编写流程和质量闭环完整，不代表所有 Prompt 都使用同一个大模板。简单任务保留最小结构，生产级任务才启用字段契约、异常处理、版本或初始化等模块。

## 支持的 Prompt 类型

| 类型 | 主要用途 |
|---|---|
| 通用文本 | 内容生成、总结、分析、改写、问答和比较 |
| 代码任务 | 代码生成、解释、审查和不含自主工具循环的修改任务 |
| 结构化输出 | JSON、表格、分类、抽取和数据转换 |
| Agent | 使用文件、命令、浏览器或其他工具完成多步状态变更 |
| 自动化工作流 | 定义触发、节点、字段映射、失败处理和最终产物 |
| 长期角色 | 建立可长期复用的角色、系统提示词和固定交互规则 |

## 安装

将仓库克隆到 Codex 的个人 Skill 目录：

```powershell
git clone https://github.com/xinghe-AGI/Agent-Prompt-Writer.git "$HOME\.codex\skills\ai-prompt-writer"
```

安装后，在新的 Codex 任务中使用 `$ai-prompt-writer` 调用。

## 快速使用

### 从零编写

```text
使用 $ai-prompt-writer，帮我编写一个账单信息抽取 Prompt。
输入是 OCR 文本，输出必须是可解析 JSON。
字段包括金额、日期、商户、类别和备注，无法识别的字段返回 null。
```

### 适配编程 Agent

```text
使用 $ai-prompt-writer，把下面需求编写成给 Claude Code 使用的 Agent Prompt：
只允许修改 src/components/UserCard.tsx，修复头像加载失败时的兜底显示。
修改后运行相关测试，不要改动其他文件。
```

### 诊断并重写

```text
使用 $ai-prompt-writer，诊断并重写下面这段 Prompt。
保留原始业务目标，重点修正任务边界、输出格式和完成标准：

<<<SOURCE_PROMPT
[粘贴已有 Prompt]
SOURCE_PROMPT
```

如果缺失信息会实质改变结果，Skill 最多提出 3 个问题。非关键内容可以合理补全，并在质量报告中明确标记。

## 默认输出

每次正常完成都固定交付最终 Prompt 和质量报告：

````markdown
## 最终 Prompt

```text
[可直接复制使用的 Prompt]
```

## 质量报告

| 检查项 | 状态 | 修改依据 |
|---|---|---|
| 任务目标 | 通过 / 已补全 / 需确认 | 具体处理 |
| 输入与上下文 | 通过 / 已补全 / 需确认 | 具体处理 |
| 约束条件 | 通过 / 已补全 / 需确认 | 具体处理 |
| 输出契约 | 通过 / 已补全 / 需确认 | 具体处理 |
| 完成标准 | 通过 / 已补全 / 需确认 | 具体处理 |
| 类型专项规则 | 通过 / 已补全 / 需确认 | 具体处理 |
| 歧义与假设 | 通过 / 已补全 / 需确认 | 具体处理 |
| 精简与安全 | 通过 / 已补全 / 需确认 | 具体处理 |
````

质量报告不使用数字评分。它说明哪些要求已经满足、哪些由 Skill 补全、哪些仍需要确认。

最终 Prompt 使用安全长度的 Markdown 围栏；如果 Prompt 内部包含代码块，外层围栏会自动增加反引号数量，避免截断内容。

## 项目结构

```text
AI Prompt Writer/
├── SKILL.md
├── README.md
├── agents/
│   └── openai.yaml
└── references/
    ├── prompt-contract.md
    └── prompt-patterns.md
```

| 文件 | 职责 |
|---|---|
| `SKILL.md` | 定义职责边界、八阶段总控流程、引用路由和交付接口 |
| `references/prompt-contract.md` | 定义九项需求契约、通用模块、条件硬标准和质量报告 |
| `references/prompt-patterns.md` | 定义六类 Prompt 的路由优先级、模板和专项门禁 |
| `agents/openai.yaml` | 定义 Codex 中展示的名称、简介和默认调用语句 |

当前项目只有一个主 Skill，不包含需要分别调用的子 Skill。

## 方法来源

- [NanGePlus/PromptsCollection](https://github.com/NanGePlus/PromptsCollection)：结构化 Prompt 与生产级约束的主要方法基准。
- [nidhinjs/prompt-master](https://github.com/nidhinjs/prompt-master)：触发边界、关键澄清和坏 Prompt 诊断。
- [ckelsoe/prompt-architect](https://github.com/ckelsoe/prompt-architect)：意图判断和质量检查维度。
- [Principled Instructions Are All You Need for Questioning LLaMA-1/2, GPT-3.5/4](https://arxiv.org/pdf/2312.16171)：通用提示原则的辅助参考。

这些资料只提供方法依据。运行中的 Skill 不展示框架名称，也不会把所有技巧机械叠加到同一段 Prompt 中。
