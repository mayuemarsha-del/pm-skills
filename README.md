# pm-skills

> **面向 B 端产品经理**的 Claude Code skill suite。覆盖 **写需求 → 调研 → 原型 → 验收 → UI 走查** 全流程。
>
> 专为 SaaS / 企业服务 / 业务后台 PM 的工作流设计——长 PRD、复杂字段表、跨模块联动、配置化页面、验收清单。C 端 PM 用得到的部分有限。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Plugin-blueviolet)](https://docs.anthropic.com/claude-code)
![Skills](https://img.shields.io/badge/skills-7-brightgreen)
[![GitHub stars](https://img.shields.io/github/stars/mayuemarsha-del/pm-skills?style=social)](https://github.com/mayuemarsha-del/pm-skills/stargazers)

> 💡 觉得有用？点个 ⭐ Star 让更多 B 端 PM 在 GitHub 搜得到 → [给 Star](https://github.com/mayuemarsha-del/pm-skills/stargazers)

<!-- 截图位 1：装上后的效果图（demo.gif 或 demo.png 放 docs/ 下） -->
<!-- ![demo](docs/demo.gif) -->

每个 skill 是一段触发规则 + 一份工作方法论。装上后，在 Claude Code 里用自然语言（"写需求""跑个竞品调研""验收 XX 模块"）就能调起对应的工作流。

## ✨ 它能帮你做什么

| 你说 | Claude 会触发的 skill | 做什么 |
|---|---|---|
| "改一下这段需求的字段表" | `prd` | 按 PRD 写作规范重组字段、操作逻辑、指标口径 |
| "跑一份竞品调研" | `competitor-research` | 按帮助中心反推菜单，逐模块梳理产品全功能 |
| "做个 Agent 列表页的 HTML 原型" | `html-prototype` | 单文件可浏览器打开，套你公司 design tokens |
| "验收一下这个模块" | `feature-acceptance` | 自动打开浏览器，对照 PRD 逐项查实，出差异清单 |
| "UI 走查一下" | `ui-walkthrough` | 设计稿 vs 线上读 computed style 实锤，出还原度报告 |
| "今天有什么 AI 进展" | `daily-research` | 联网调研当日动态，产出三段式简报 |

<!-- 截图位 2：实际触发某个 skill 的截图，比如验收报告输出的样子 -->
<!-- ![acceptance-report](docs/screenshot-acceptance.png) -->


## 🚀 快速开始

**通过 Plugin Marketplace（推荐）**：

```
/plugin marketplace add mayuemarsha-del/pm-skills
/plugin install pm-skills
```

装完不知道怎么用？看 **[5 分钟上手指南 →](./QUICKSTART.md)**

**手动 clone**：

```bash
git clone https://github.com/mayuemarsha-del/pm-skills.git ~/.claude/skills-source/pm-skills
ln -s ~/.claude/skills-source/pm-skills/skills/* ~/.claude/skills/
```

## 📦 7 个 skill

| Skill | 触发场景 | 开箱即用 |
|---|---|---|
| `prd` | 写 / 改 / 重构 / 统一口径需求文档 | 建议替换 `example-domain.md` |
| `competitor-research` | 写竞品现状、AI 产品调研、模块汇总 | ✅ |
| `research-report` | 三维度（Prompt / Context / Harness）深度调研 | ✅ |
| `daily-research` | 每日 AI 行业动态简报 | 需替换 `prompt.md` 模板 |
| `html-prototype` | 单文件可浏览器打开的 HTML 原型 | 需替换 `tokens.css` |
| `feature-acceptance` | 研发提测后对照 PRD + 原型验收线上实现 | ✅（可选高速模式） |
| `ui-walkthrough` | 设计 HTML vs 线上页面 UI 还原度走查 | ✅（可选高速模式） |

## ⚙️ 装上后你需要做的配置

把下面的内容追加到你自己的 `~/.claude/CLAUDE.md` 或项目 `CLAUDE.md`，按实际填写：

```markdown
## pm-skills 配置

- RESEARCH_DIR: <每日调研简报 + 长期研究库根目录，例如 ~/work/research/>
- COMPETITION_DIR: <竞品现状文档根目录，例如 ~/work/competition/>
- ACCEPTANCE_DIR: <验收 / UI 走查产物根目录，例如 ~/work/acceptance/>
- BROWSER_CAP_DIR: <可选，装了 claude-browser-cap 才填，例如 ~/.claude/tools/browser-cap>
```

除了路径配置外，还有几件内容性的事要做：

### `html-prototype` 的 design tokens（必做）

默认 `skills/html-prototype/references/example-tokens.css` 是原作者的设计系统（橙紫双 brand + 24px 圆角），别人公司用不了。把这个文件**整体替换**成你公司的 CSS 变量（颜色 / 字体 / 圆角 / 间距 / 阴影）。SKILL.md 会把它内联到每份生成的原型 `<style>` 里。

保留例子里列出的必备变量名（`--neutral-*` / `--secondary-*` / `--radius-md` / `--shadow-sm` 等），值随便换 —— SKILL.md 中的组件代码片段依赖这些变量名。

### `prd` 的领域知识（建议做）

默认 `skills/prd/references/example-domain.md` 是 AI 客服 Agent 领域的示例（产品体系 / 模块定位 / 迁移原则）。把内容换成你产品的领域知识，PRD skill 在写涉及产品体系 / 模块定位的 PRD 时会按需读它。

### `daily-research` 的调研规格（必做）

`skills/daily-research/references/example-prompt.md` 是一个调研规格示例（每日扫 AI 应用层动态）。

1. 复制到 `<RESEARCH_DIR>/prompt.md`。
2. 改成你关心的方向（金融监管动态 / 跨境电商新政 / 某垂直行业一手新闻 等）。

SKILL.md 每次执行调研都先读 `<RESEARCH_DIR>/prompt.md` 作为规格。不复制就跑不起来。

## ⚡ 可选：高速验收

`feature-acceptance` 和 `ui-walkthrough` 默认用 chrome-devtools MCP 驱动浏览器，开箱即用。

如果验收量大，可以装配套的高速工具：

```bash
git clone https://github.com/mayuemarsha-del/claude-browser-cap.git ~/.claude/tools/browser-cap
```

然后在 CLAUDE.md 里设 `BROWSER_CAP_DIR`。两个验收 skill 会自动检测并切换到高速模式（一次执行多步动作，节省 token 和回合数）。

## 🧩 兼容性

- Claude Code（VS Code 扩展、CLI、Web、Desktop）
- 模型：Opus / Sonnet / Haiku 4.x

## 🤝 反馈与贡献

- 用着不顺手 / 触发不到 / description 写岔了 → 开 [Issue](https://github.com/mayuemarsha-del/pm-skills/issues/new/choose)（有模板，1 分钟填完）
- 想加新 skill / 改进现有 skill → 欢迎 PR，详见 [CONTRIBUTING.md](./CONTRIBUTING.md)
- 想分享你的 design tokens 或调研 prompt 模板给其他 PM 用 → 也可以 PR 到 `examples/` 目录
- 不知道往哪问 → 开 [Discussion](https://github.com/mayuemarsha-del/pm-skills/discussions)

## ⭐ Star History

[![Star History Chart](https://api.star-history.com/svg?repos=mayuemarsha-del/pm-skills&type=Date)](https://star-history.com/#mayuemarsha-del/pm-skills&Date)

## 📄 许可

MIT
