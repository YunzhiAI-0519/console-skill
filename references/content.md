# D. 主内容区 — 页头 · 概览卡 · 用量条 · 数据表格 · 右栏 · 费用

## D0. 画布与栅格

```css
.cmain { flex:1; min-width:0; padding:20px 24px 28px; }        /* 灰画布上的内容区 */
.cgrid  { display:grid; grid-template-columns:minmax(0,1fr) 320px; gap:16px; align-items:start; }
```

- 面板统一：白卡 + `1px --border-light` + `--radius-lg` + 18px 内边距 + `min-width:0`。
- `panel__head`：左标题 `15.5px/700`；右侧 `meta`（英文小标注）/ `tools`（按钮组）/ `more`（蓝链接）。
- 右栏 320px 在 ≤1280px 并入下方（先 2 列再 1 列）。

---

## D1. 面包屑 + 页头

- 面包屑：12.5px，上级蓝可点、末级 `--text-4`，14px chevron 分隔。
- 页头：H1 问候 `24px/800/-0.02em` + 次信息行 13px `--text-3`（上次登录时间 / 地域 / **IP 打码** `116.24.*.*`）。
- 右侧操作组：[副 CTA 白描边 34px] [主 CTA 渐变 34px]；`align-items:flex-end`；≤640px 改纵向。

---

## D2. 概览卡（4 列）

- 栅格：`repeat(4, minmax(0,1fr))`，gap 16px；≤900px 2 列；≤640px 1 列。
- 白卡 16px 圆角、18px 内边距、`min-width:0`、`position:relative`。
- 结构四层：
  1. **头部**：图标芯片 38px / 圆角 11px / `--primary-soft` 底 / 蓝图标 18px + 右上灰色 more（14px 三点）
  2. **数字行**：`24px / 800 / tabular-nums`；单位用 13px `--text-3` 小字（`台` / `条`）
  3. **标签行**：13px `--text-3` + hint（12px `--text-4`，如「冻结 ¥0.00」「共 52 台」）或 delta（12px/700：正向橙 `--warn`、紧急 `--dot-danger`）
  4. **底部链接**：12.5px 蓝 600 + 14px 箭头；**hover 箭头 `translateX(4px)`**，禁改 gap
- hover：`translateY(-2px)` + `--primary-border` 边 + `--shadow-card-hover`。

---

## D3. 资源用量条

- 行：`grid-template-columns:130px minmax(0,1fr) 110px`，`gap 12px`；≤640px 标签独占一行。
- 进度条：高 8px 胶囊，槽底 `--bg-alt`；填充默认 `--grad-cta`，**≥75% 换 `--grad-warn`**。
- 数值：12.5px `tabular-nums` 右对齐（`62%` 或 `328 / 800 Mbps`）。
- **禁过渡 width**：静态直接设宽；需要动画用 `transform: scaleX()`。

---

## D4. 数据表格（核心组件）

- 容器：`1px --border-light` + `--radius-md` + `overflow:hidden` + `min-width:0`。
- 表头：`12px/700/--text-3`、底 `--bg-alt`、下边框、`white-space:nowrap`。
- 行：上下 `12px`、`border-bottom`（末行去线）、hover `--bg-alt`。
- 状态列：状态点（带外环）+ 12.5px/600 文本；语义固定（红线 9）。
- ID / IP：mono 12.5px + `--text-1`；名称列普通字体，**两行堆叠**（ID 上、名称下）。
- 操作列：**右对齐**、`nowrap`；蓝 600 链接 + 浅色 `|` 分隔；首操作 = 最高频动作（登录/续费）。
- 筛选：tabs 胶囊槽（每项带计数）+ 搜索框（36px，占位含可搜字段提示），双条件实时联动分页。
- 分页：左信息 `共 N 台 · 第 x / y 页`（tabular），右 30px 方钮（8px 圆角，当前页蓝边蓝字加粗）。
- 空态：居中 32px、`--text-4` 文案，**禁白板**。
- 刷新按钮：点击图标旋转 360°反馈（`prefers-reduced-motion` 时跳过）。

---

## D5. 右栏（320px）

| 组件 | 规范 |
|---|---|
| 待办事项 | 行：图标（时钟/钱包=橙 `--warn`，盾=蓝）+ 标题 13px/700 + 说明 12.5px `--text-3`；行 hover `--bg-alt`；头部带 `3 TODO` 标注 |
| 快捷入口 | 3 列格子：图标 20px 蓝 + 12.5px 文字；边框小白卡，hover `-2px` + 蓝边 + `--shadow-panel` |
| 产品公告 | 项：`NEW` 徽章或 `09-08` 日期 + 13px 链接（`overflow-wrap:break-word`） |
| 帮助面板 | **全站唯一彩色面板**：`--primary-soft` 底 + `--primary-border` 边；链接行 38px hover 白底 |

---

## D6. 费用概览

- 指标 4 格：`--bg-alt` 小卡 + 12px 圆角；标签 12.5px → 数字 `20px/800 tabular` → 说明 12px `--text-4`。
- 消费构成分段条：高 12px 圆角、四段按占比分宽，颜色必须 `--chart-1..4`；
  legend 用 8px 圆点 + 百分比，允许换行（`flex-wrap`）。
- 新图表色先在 tokens 扩展块注册（红线 10）。

---

## D7. 空态 / 加载态

- 空态：居中文案（`--text-4`）+ 可选操作按钮；禁白板。
- 骨架屏：`--bg-alt` 色块按真实尺寸占位（表格按行高 44px 铺 6 行），**禁满屏 spinner**。
- 数据加载中禁用提交类按钮，避免重复请求。
