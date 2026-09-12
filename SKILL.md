---
name: minirelax-console-design
description: MiniRelax 控制台（用户中心）视觉语言与实现规范。当需要按该品牌现有控制台视觉语言设计、移植、续写或重做用户中心 / 云控制台 / 管理后台 / 数据面板类页面，生成符合其顶栏、侧边栏产品树、概览卡、数据表格、浮层与费用组件规范的 HTML/CSS/React/Vue 代码，或评审现有后台页面是否符合该设计语言时使用。触发场景包括「MiniRelax 控制台」「用户中心」「管理后台」「控制台新页面」「按设计 token 出后台区块」等。
agent_created: true
---

# MiniRelax 控制台（用户中心）设计语言

## Overview

本技能封装 MiniRelax 控制台的完整设计语言：固定顶栏、侧边栏产品树、灰底白卡画布、概览卡、
数据表格、右栏组件、费用组件、浮层三件套，外加 12 条红线与红线自检脚本。

一句话定调：**专业、可信赖、信息密集、操作优先。**

## 与《minirelax-landing-design》（落地页技能）的关系

- **共用同一套基础 token**（主蓝 #2563eb、文字层级、圆角家族、阴影家族）。
- **密度策略相反**：落地页是营销页（大留白、大动效），控制台是信息密集型 UI（紧凑、克制）。
  两份技能可叠加使用，但**不要互相串密度**——悬浮幅度、留白、动效幅度都不同。
- **不要把落地页技能的 `components.css` 引进控制台**：其中 `.topbar` 是落地页悬浮胶囊，
  会与本技能 `console.css` 的控制台顶栏类名冲突。控制台只引 `tokens.css` + `console.css`。

## 何时使用

使用：新做 / 移植 / 续写用户中心、云控制台、管理后台、数据面板、设置页等**操作型界面**；
评审后台页面是否符合设计语言；需要控制台 chrome（顶栏/侧栏）、表格、浮层组件。

不使用：官网首页、营销落地页、活动页（用 `minirelax-landing-design`）；非视觉任务。

## 最关键的一条：品牌规范优先 + 控制台特有裁决

通用前端方法论与「反 AI 味」规则是**通用建议**；本技能是**品牌约束型规范**，冲突时以品牌规范为准。
此外控制台有三条落地页没有的裁决，最容易被做崩：

| 裁决 | 规定 |
|---|---|
| 画布模型 | **灰底白卡**（`body` 用 `--bg-alt`，内容浮在带边框白卡上）——与落地页白画布相反 |
| 固定 chrome 不做浮起 | 顶栏/侧栏贴边 + border + 极淡阴影；**只有浮层**（下拉/抽屉/弹窗）才用 `blur+border+shadow` 三件套。不要给侧边栏加毛玻璃，不要把顶栏做成悬浮胶囊 |
| 密度优先 | 卡片 padding ≤18px、区块间距 16px、悬浮 ≤2px；禁营销页的 88–96px 大留白、视差、跑马灯、打字机 |

## 页面全貌（用户中心一页流）

| 区块 | 要点 | 详见 |
|---|---|---|
| **B 顶栏** | 56px 固定；品牌区 + 主导航 + 全局搜索（Ctrl+K）+ 文档/工单/消息 + 余额胶囊 + 用户胶囊 | `references/chrome.md` |
| **C 侧边栏** | 240px 产品树：分组标题、计数徽标、激活态 3px 指示条、桌面可收起、移动端抽屉 | `references/chrome.md` |
| **D 主内容区** | 页头 → 概览 4 卡 → 资源用量条 → 数据表格（tabs+搜索+分页）→ 右栏四件套 → 费用概览 | `references/content.md` |
| **E 浮层与反馈** | 下拉浮卡、抽屉、Toast/确认弹窗、骨架屏（唯一允许毛玻璃的层级） | `references/overlays.md` |
| **F 页脚** | slim legal bar（禁止搬落地页四层页脚） | `references/overlays.md` |

## 数据驱动与白标

账户/品牌字段来自 `/api/v1/site-config`：`balance`、`username`、`icp` 等。
**空值时对应元素自动 `hidden`**，不渲染空壳；组件内禁止硬编码品牌信息。

## 工作流

### Step 1 — 定调与约束确认
确认页面类型（列表页 / 详情页 / 设置页 / 总览页）、信息密度、复用的既有区块。不做「重定义品牌色」类越界动作。

### Step 2 — 读取规范
- 全局系统（画布模型 / token / 密度 / 动效 / 断点）→ `references/design-tokens.md`
- 顶栏与侧边栏 → `references/chrome.md`
- 主内容区（页头 / 概览卡 / 表格 / 右栏 / 费用 / 空态）→ `references/content.md`
- 浮层与页脚 → `references/overlays.md`
- 红线与模板 → `references/redlines-and-templates.md`

直接引用现成 CSS（顺序固定）：
```html
<link rel="stylesheet" href="assets/tokens.css" />
<link rel="stylesheet" href="assets/console.css" />
```
`tokens.css` 含基础变量 + 控制台扩展变量 + `prefers-reduced-motion` 降级块；
`console.css` 只含组件与布局，所有颜色走变量。

### Step 3 — 结构先行
顶栏 → 侧边栏 → 面包屑页头 → 按需拼面板（概览卡 / 表格 / 右栏 / 费用）→ 页脚条。
先出语义化 DOM 与 class 钩子，再填视觉。

### Step 4 — 视觉实现
颜色只走变量；圆角 / 阴影 / 动效只用规范值；交互元素三态齐全；
**含长文本的 flex/grid 子项必须 `min-width:0`**；数字一律 `tabular-nums`，表格 ID/IP 用 mono。

### Step 5 — 动效与浮层
只过渡 `opacity / transform / border-color / color / background-color`；禁 `all` 与布局属性。
浮层遵循「点外关闭 + Esc + aria-expanded」，危险操作二次确认。必须做 `prefers-reduced-motion` 降级。

### Step 6 — 自检与交付
跑 `scripts/check_redlines.py` + 红线清单（见 redlines-and-templates.md），交付代码 + token 引用方式 + 响应式说明。

## 关键数值速查

| 项目 | 值 |
|---|---|
| 顶栏 | 高 `56px`，固定，白底 + border-bottom + `--shadow-chrome`（不悬浮） |
| 侧边栏 | `240px`（收起 `64px`），sticky，白底 `border-right` |
| 画布 | `--bg-alt` 灰底，白卡 `1px --border-light` 边 + `--radius-lg` |
| 面板内边距 | `18px`（上限 20px）；栅格间距 `16px` |
| 概览卡数字 | `24px / 800 / tabular-nums`；图标芯片 38px 圆角 11px |
| 表格行 | 上下 `12px`（总高约 44px）；容器 12px 圆角 `overflow:hidden` |
| 状态点 | 8px 圆 + 同色 3px 外环；绿=运行 橙=预警 灰=停止 红=紧急 |
| 悬浮幅度 | `-2px` + `--shadow-panel`（不用营销阴影） |
| 右栏宽度 | `320px`（≤1280px 并入下方） |
| 动效 | 微交互 `.2s ease`；浮层 `.18–.26s`；只过渡白名单属性 |

## 十二条红线（详见 redlines-and-templates.md）

1. 灰画布白卡必须带边框。 2. 固定 chrome 不做浮起，浮层才用三件套。 3. 禁黑色阴影。
4. 禁 `transition:all` 与布局属性过渡。 5. 数字 tabular-nums、ID/IP 用 mono。
6. 操作列右对齐、行 hover 反馈、空态必给文案。 7. 长文本子项 `min-width:0`。
8. 密度红线：禁大留白与营销动效。 9. 状态语义固定（绿/橙/灰/红）。 10. 颜色仅来自变量表。
11. 浮层交互契约 + 危险操作二次确认。 12. 三态齐全 + reduced-motion + 安全区。

## 资源索引

- `references/design-tokens.md` — A 全局系统：画布模型、token 全表、密度、动效、断点
- `references/chrome.md` — B 顶栏 + C 侧边栏（含收起态与移动抽屉）
- `references/content.md` — D 主内容区：页头 / 概览卡 / 用量条 / 表格 / 右栏 / 费用 / 空态骨架
- `references/overlays.md` — E 浮层与反馈 + F 页脚
- `references/redlines-and-templates.md` — 12 条红线、使用模板、交付自检清单
- `assets/tokens.css` — 基础变量 + 控制台扩展变量（`--chart-*` / `--dot-*` / `--ctop-h` 等）+ 降级块
- `assets/console.css` — 顶栏 / 侧边栏 / 面板 / 表格 / 浮层组件样式
- `scripts/check_redlines.py` — 红线静态自检（与落地页技能共用同一套检查逻辑）

## 常见错误

- 引入落地页 `components.css` → `.topbar` 类名冲突，顶栏变悬浮胶囊 → **跑偏**。
- 白画布上放无边框白卡 → 卡片消失。
- 给侧边栏/顶栏加毛玻璃 → 层级混乱，违反红线 2。
- 过渡 `width / gap`（进度条、下划线展开）→ 违反红线 4，用 `transform`。
- 表格数字没用 tabular、ID 没用 mono → 数据页观感立刻垮。
- 空态白板、行无 hover、操作列左对齐 → 不像正经控制台。
- 营销动效（视差/跑马灯/大悬浮）搬进控制台 → 违反红线 8。
