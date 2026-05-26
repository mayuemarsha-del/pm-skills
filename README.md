1. # pm-skills

   一套面向产品经理的 Claude Code skill suite。覆盖 **写需求 → 调研 → 原型 → 验收 → UI 走查** 全流程。

   每个 skill 是一段触发规则 + 一份工作方法论。装上后，在 Claude Code 里用自然语言（"写需求""跑个竞品调研""验收 XX 模块"）就能调起对应的工作流。

2. ## 安装

   2.1. **通过 Plugin Marketplace（推荐）**

   ```
   /plugin marketplace add mayuemarsha-del/pm-skills
   /plugin install pm-skills
   ```

   2.2. **手动 clone**

   ```bash
   git clone https://github.com/mayuemarsha-del/pm-skills.git ~/.claude/skills-source/pm-skills
   ln -s ~/.claude/skills-source/pm-skills/skills/* ~/.claude/skills/
   ```

3. ## 7 个 skill

   | Skill | 触发场景 | 开箱即用 |
   |---|---|---|
   | `prd` | 写 / 改 / 重构 / 统一口径需求文档 | 建议替换 `example-domain.md` |
   | `competitor-research` | 写竞品现状、AI 产品调研、模块汇总 | ✅ |
   | `research-report` | 三维度（Prompt / Context / Harness）深度调研 | ✅ |
   | `daily-research` | 每日 AI 行业动态简报 | 需替换 `prompt.md` 模板 |
   | `html-prototype` | 单文件可浏览器打开的 HTML 原型 | 需替换 `tokens.css` |
   | `feature-acceptance` | 研发提测后对照 PRD + 原型验收线上实现 | ✅（可选高速模式） |
   | `ui-walkthrough` | 设计 HTML vs 线上页面 UI 还原度走查 | ✅（可选高速模式） |

4. ## 装上后你需要做的配置

   把下面的内容追加到你自己的 `~/.claude/CLAUDE.md` 或项目 `CLAUDE.md`，按实际填写：

   ```markdown
   ## pm-skills 配置

   - RESEARCH_DIR: <每日调研简报 + 长期研究库根目录，例如 ~/work/research/>
   - COMPETITION_DIR: <竞品现状文档根目录，例如 ~/work/competition/>
   - ACCEPTANCE_DIR: <验收 / UI 走查产物根目录，例如 ~/work/acceptance/>
   - BROWSER_CAP_DIR: <可选，装了 claude-browser-cap 才填，例如 ~/.claude/tools/browser-cap>
   ```

   除了路径配置外，还有两件内容性的事要做：

   4.1. **`html-prototype` 的 design tokens（必做）**

   默认 `skills/html-prototype/references/example-tokens.css` 是原作者的设计系统（橙紫双 brand + 24px 圆角），别人公司用不了。把这个文件**整体替换**成你公司的 CSS 变量（颜色 / 字体 / 圆角 / 间距 / 阴影）。SKILL.md 会把它内联到每份生成的原型 `<style>` 里。

   保留例子里列出的必备变量名（`--neutral-*` / `--secondary-*` / `--radius-md` / `--shadow-sm` 等），值随便换 —— SKILL.md 中的组件代码片段依赖这些变量名。

   4.2. **`prd` 的领域知识（建议做）**

   默认 `skills/prd/references/example-domain.md` 是 AI 客服 Agent 领域的示例（产品体系 / 模块定位 / 迁移原则）。把内容换成你产品的领域知识，PRD skill 在写涉及产品体系 / 模块定位的 PRD 时会按需读它。

   4.3. **`daily-research` 的调研规格（必做）**

   `skills/daily-research/references/example-prompt.md` 是一个调研规格示例（每日扫 AI 应用层动态）。

   1. 复制到 `<RESEARCH_DIR>/prompt.md`。
   2. 改成你关心的方向（金融监管动态 / 跨境电商新政 / 某垂直行业一手新闻 等）。

   SKILL.md 每次执行调研都先读 `<RESEARCH_DIR>/prompt.md` 作为规格。不复制就跑不起来。

5. ## 可选：高速验收

   `feature-acceptance` 和 `ui-walkthrough` 默认用 chrome-devtools MCP 驱动浏览器，开箱即用。

   如果验收量大，可以装配套的高速工具：

   ```
   git clone https://github.com/mayuemarsha-del/claude-browser-cap.git ~/.claude/tools/browser-cap
   ```

   然后在 CLAUDE.md 里设 `BROWSER_CAP_DIR`。两个验收 skill 会自动检测并切换到高速模式（一次执行多步动作，节省 token 和回合数）。

6. ## 兼容性

   - Claude Code（VS Code 扩展、CLI、Web、Desktop）
   - 模型：Opus / Sonnet / Haiku 4.x

7. ## 许可

   MIT
