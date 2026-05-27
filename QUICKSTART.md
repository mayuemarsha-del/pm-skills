# 5 分钟上手 pm-skills

假设你已经装好了（`/plugin install pm-skills`）。这份文档带你 5 分钟内跑通一个 skill，确认整套能用。

---

## 第 1 步：确认装好了（30 秒）

在 Claude Code 里输入：

```
/plugin list
```

应该能看到 `pm-skills 0.1.0` 在列表里。看不到？回 [README](./README.md) 检查安装步骤。

---

## 第 2 步：配置 4 个路径（2 分钟）

打开你自己的 `~/.claude/CLAUDE.md`（没有就新建），追加：

```markdown
## pm-skills 配置

- RESEARCH_DIR: ~/work/research/
- COMPETITION_DIR: ~/work/competition/
- ACCEPTANCE_DIR: ~/work/acceptance/
- BROWSER_CAP_DIR: （可选，装了 claude-browser-cap 才填）
```

把 `~/work/` 换成你想放产物的实际目录。skill 会按这些路径创建 / 写入文件。

```bash
# 顺手把目录建好
mkdir -p ~/work/research ~/work/competition ~/work/acceptance
```

---

## 第 3 步：挑一个 skill 跑通（2 分钟）

最容易看到效果的是 **prd skill**——不依赖任何外部资源。

### 试触发 1：让 prd skill 改一段需求

随便准备一段你以前写过的需求（或者用下面这段假数据），在 Claude Code 里输入：

```
改一下这段需求，操作逻辑太散了：

字段：商品名称（必填，50 字以内）、商品分类（必填，从下拉选）、库存数（必填，整数 ≥ 0）。

当用户点击保存时：
如果商品名称为空，提示"请填写商品名称"
如果商品分类没选，提示"请选择分类"
如果库存数为空或小于 0 或非整数，分别提示对应错误
全部通过后，提交保存请求，成功跳列表页，失败提示"保存失败，请重试"
```

**期望看到**：Claude 显示 `⏺ Skill(pm-skills:prd)`，然后按 PRD 规范帮你重组——字段表收敛、操作逻辑按字段分组、校验子项用编号。

看到 `Skill(pm-skills:prd)` 这一行就说明触发成功了，不用真等它写完。

### 试触发 2（可选）：让它生成一个 HTML 原型

```
帮我做一个商品列表页的 HTML 原型，要列表 + 搜索 + 新建按钮
```

期望看到 `⏺ Skill(pm-skills:html-prototype)`。这个 skill 默认套原作者的 design tokens（橙紫双 brand）——如果不是你公司风格也没关系，先验证流程能跑。

---

## 第 4 步：知道每个 skill 怎么触发（1 分钟）

下面这张表抄到顺手的地方，遇到对应场景直接用：

| 你说什么 | 触发的 skill | 它会做什么 |
|---|---|---|
| "改一下这段需求 / 这段 PRD 写得乱" | `prd` | 按 PRD 规范重组 |
| "写 / 补一份 ⓘ 文案" | `prd` | 按指标口径表规范写 |
| "跑个竞品调研 / 把 X 产品功能梳理一下" | `competitor-research` | 真去帮助中心抓菜单，逐模块写 |
| "做一份正式调研报告 / 横向对比 X Y Z" | `research-report` | 三维度产出结构化报告 |
| "今天有什么 AI 进展 / 跑一份调研简报" | `daily-research` | 联网调研当日动态，落到 `<RESEARCH_DIR>/YYYY-MM-DD.md` |
| "做个 X 页面的 HTML 原型 / 给前端" | `html-prototype` | 单文件 HTML，浏览器直接打开 |
| "验收一下这个模块" + PRD 路径 + 线上 URL | `feature-acceptance` | 自动驱动浏览器，对照 PRD 逐项查实 |
| "UI 走查一下" + 设计 HTML + 线上 URL | `ui-walkthrough` | 同视口截图 + 读 computed style 实锤差异 |

---

## 进阶配置（按需）

跑通第 3 步后，再按需做这三件：

### A. 换 html-prototype 的 design tokens

默认 `skills/html-prototype/references/example-tokens.css` 是原作者的设计系统。换成你公司的：

```bash
# 找到 plugin 安装目录（一般是 ~/.claude/plugins/cache/.../html-prototype/references/）
# 把 example-tokens.css 整个内容替换成你自己的 CSS 变量定义
```

保留变量名（`--neutral-*` / `--secondary-*` / `--radius-md` 等），值随便换。

### B. 换 prd 的领域知识

`skills/prd/references/example-domain.md` 默认是 AI 客服领域的示例。改成你产品的领域知识（产品体系 / 模块定位 / 关键概念）。

### C. 写 daily-research 的 prompt

`skills/daily-research/references/example-prompt.md` 是示例。复制到 `<RESEARCH_DIR>/prompt.md` 并按你关心的方向改（金融 / 跨境电商 / 法律 / 医疗 / AI 都行）。

---

## 常见问题

**Q：粘了触发句但没显示 `⏺ Skill(...)`**

A：description 触发词没匹配上。试着把关键词换得更明确，比如把"改一下"换成"按 PRD 规范改"。如果某句话怎么都触发不到，欢迎 [开 issue](https://github.com/mayuemarsha-del/pm-skills/issues) 告诉我，我加触发词。

**Q：触发了但行为不对（用了我没想要的方式做）**

A：每个 skill 的方法论可以在 `SKILL.md` 里看到。如果方法论本身就不符合你的工作习惯，复制一份到你自己的项目 `.claude/skills/` 下改成你的版本，会覆盖 plugin 版本。

**Q：feature-acceptance / ui-walkthrough 跑起来很慢 / 很贵**

A：默认走 chrome-devtools MCP，每步一回合。装 [claude-browser-cap](https://github.com/mayuemarsha-del/claude-browser-cap) 配合使用，验收一个模块能省 50-80% 的 token。

**Q：自定义后怎么不影响 plugin 更新？**

A：把改动文件复制到你项目的 `.claude/skills/<skill-name>/` 下，覆盖优先级高于 plugin。这样 plugin 更新时你的本地改动不会丢。

---

跑通了？发个反馈到 [Issues](https://github.com/mayuemarsha-del/pm-skills/issues)，或者直接发 PR 加你自己的 skill。
