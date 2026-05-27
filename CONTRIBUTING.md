# 贡献指南

欢迎所有形式的贡献——不一定要写代码，**反馈一句 "这个 skill 触发不到我说的某句话" 就是非常有价值的贡献**。

## 三种最受欢迎的贡献

### 1. 反馈 skill 触发不到 / 触发错

每个 skill 的 `description` 决定了它什么时候被 Claude 触发。原作者的工作语境覆盖不全，你的真实使用场景反馈最珍贵。

[开 Issue](https://github.com/mayuemarsha-del/pm-skills/issues/new) 告诉我们：

1. **你说的那句话**（一字不改贴上来）。
2. **你期望触发哪个 skill**。
3. **实际触发了什么 skill 或没触发**。

我会评估是该 skill description 不够全，还是跟另一个 skill 撞了，更新一版。

### 2. 分享你的 design tokens / 调研 prompt / 领域知识

`html-prototype/references/example-tokens.css`、`daily-research/references/example-prompt.md`、`prd/references/example-domain.md` 这些都是"示例"——欢迎 PR 你公司 / 行业的实际版本进 `examples/` 目录，让其他 PM 不用从零写起。

PR 时请去掉商业敏感信息（具体客户名、内部 URL、定价等）。

### 3. 加新 skill

如果你有一段重复的工作流，觉得能抽成 skill，欢迎提议或直接 PR。提议形式：

[开 Issue](https://github.com/mayuemarsha-del/pm-skills/issues/new) 描述：

1. **什么场景下会触发**（一句话）。
2. **触发后 skill 应该做什么**（按用户操作流程）。
3. **跟现有 7 个 skill 边界怎么划**。

讨论清楚后我们一起决定加进核心 plugin 还是建议你独立维护。

## 怎么提 PR

1. Fork 这个 repo。
2. 建一个 feature 分支（如 `add-pricing-research-skill`）。
3. 改 / 加文件。**SKILL.md 必须有 `name` + `description` frontmatter**。
4. 用 `claude --plugin-dir <你 fork 的本地路径> plugin details pm-skills` 校验通过。
5. 提 PR，描述里说明动机 + 测过的触发场景。

## 排版风格（仅适用于 SKILL.md / 文档类）

为了保持一致：

1. 标题里不塞括号备注（反例 `### 4.5 Monitors（Pro add-on）`；正例 `### 4.5 Monitors`，备注下沉到正文）。
2. 短句 ≤ 30 字；复合长句拆有序序号。
3. 分项内容用有序数字 `1./2./3.`，子项也用数字，不用 `-` bullet。

完整规范见 `skills/prd/SKILL.md` 的「二、通用排版规则」。

## 行为准则

朴素 GitHub 礼节：对人友好、对事直接。Issue / PR 里不攻击别人的工作流偏好。
