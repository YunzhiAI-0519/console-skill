# A. 全局设计系统 — 画布模型 · Token · 密度 · 动效 · 断点

> 落地文件：`assets/tokens.css`（基础变量 + 控制台扩展变量 + 降级块）、`assets/console.css`（组件）。

## A1. 画布模型（第一原则，与落地页相反）

```
落地页：白画布，区块用 section-alt 浅灰交替，大留白
控制台：灰画布（--bg-alt），内容浮在白色卡片上，紧凑
```

- `body { background: var(--bg-alt) }`；顶栏、侧边栏、一切面板为白底。
- **白卡必须带边框** `1px solid var(--border-light)`，否则在灰画布上消失（红线 1）。
- 唯一的彩色面板：帮助面板（`--primary-soft` 底 + `--primary-border` 边）。

## A2. 色彩变量

### 基础（与落地页共用）

```css
--primary:#2563eb; --primary-hover:#1d4ed8; --primary-active:#1e40af;
--primary-soft:#eff6ff; --primary-border:#bfdbfe;
--ink:#0f172a; --bg-code:#0f172a;
--text-1:#0f172a; --text-2:#334155; --text-3:#64748b; --text-4:#94a3b8;
--border:#e2e8f0; --border-light:#eef2f7;
--bg:#ffffff; --bg-alt:#f8fafc;
--success:#059669; --success-soft:#ecfdf5;
--warn:#d97706; --danger:#dc2626;
--radius-lg:16px; --radius-md:12px; --radius-pill:999px;
--grad-cta:linear-gradient(90deg,#4f46e5,#2563eb,#0891b2);
--grad-brand:linear-gradient(135deg,#2563eb,#7c3aed);
--shadow-card-hover:0 12px 32px rgba(37,99,235,.10);
--shadow-panel:0 8px 26px rgba(37,99,235,.08);
--shadow-float:0 8px 30px rgba(15,23,42,.08), 0 2px 8px rgba(15,23,42,.04);
```

### 控制台扩展（`assets/tokens.css` 末尾的扩展块）

```css
--ctop-h:56px;  --side-w:240px;  --side-w-min:64px;
--chart-1:#2563eb; --chart-2:#7c3aed; --chart-3:#0891b2; --chart-4:#94a3b8;
--dot-ok:#10b981;    --dot-ok-ring:#ecfdf5;     /* 运行/正常 */
--dot-warn:#f59e0b;  --dot-warn-ring:#fffbeb;   /* 即将到期/预警 */
--dot-muted:#cbd5e1; --dot-muted-ring:#f1f5f9;  /* 已停止/无 */
--dot-danger:#ef4444;                           /* 紧急/危险/未读 */
--grad-warn:linear-gradient(90deg,#f59e0b,#d97706);
--danger-soft:#fef2f2;
```

> 红线 10：颜色仅来自变量表；图表新增色先在扩展块注册，禁止散落硬编码。
> 例外：`#fff` 作为「彩底上的白字/白点」可与落地页技能保持同一写法。

## A3. 阴影家族（chrome 与浮层分开）

| 层级 | 阴影 | 说明 |
|---|---|---|
| 固定 chrome（顶栏） | `0 1px 0 rgba(15,23,42,.02), 0 2px 10px rgba(15,23,42,.03)` | 贴边 + border-bottom，**不做浮起** |
| 卡片 hover | `--shadow-card-hover` | 蓝系 12px 32px 10% |
| 面板内浮起（tabs 激活等） | `--shadow-panel` | 蓝系 8px 26px 8% |
| 浮层（下拉/抽屉/弹窗） | `--shadow-float` + blur + border | **唯一允许毛玻璃的层级** |

## A4. 密度与间距（信息密集型）

| 项 | 值 |
|---|---|
| 卡片/面板内边距 | `18px`（上限 20px） |
| 栅格与卡片间距 | `16px` |
| 表格行 | 上下 `12px`（总高约 44px） |
| 悬浮幅度 | 卡片 `-2px` |
| 内容区 padding | `20px 24px 28px`（≤640px `16px 14px 24px`） |

## A5. 字体与数字

| 项 | 规格 |
|---|---|
| 字体栈 | PingFang SC / Microsoft YaHei 优先 |
| 页标题 | `24px / 800 / -0.02em` |
| 面板标题 | `15.5px / 700` |
| 正文 / 辅助 | `13–14px` / `12–12.5px` |
| 英文小标注 | `10–11px / 600 / .1–.12em / uppercase / --text-4` |
| **数字** | 一律 `tabular-nums` |
| **代码** | 表格 ID / IP 用 `--font-mono` `12.5px` + `--text-1` |

## A6. 通用组件语言

| 元素 | 规范 |
|---|---|
| 按钮 | 一律胶囊；主=渐变白字+蓝影；副=白底描边 hover 蓝化；幽灵=透明底描边；小号 34px、标准 40px |
| 徽章 | 22px 胶囊；NEW 蓝（`--primary-soft`/`--primary-border`） |
| 状态点 | 8px 圆 + 同色 3px 外环；语义固定：绿=运行 橙=预警 灰=停止 红=紧急 |
| 图标 | SVG 线性 stroke 1.8 round；20 / 16 / 14px 三档 |
| 输入框 | 36–38px、12px 圆角；focus 蓝边 + `0 0 0 3px --primary-soft` |
| tabs | 胶囊槽（`--bg-alt` + 边框 + 4px 内衬）+ 32px 子项；激活=白底蓝字 + `--shadow-panel` |
| 图标按钮 | 36px 圆形无边框；hover `--primary-soft` 底 + 蓝 |
| 链接 | `--primary` 600；位移只用 `transform` |

## A7. 动效

- 白名单：`opacity / transform / border-color / color / background-color`。
- 禁 `transition:all`；禁过渡 `width / height / gap / top / left`（进度条、下划线用 `transform`）。
- 微交互 `.2s ease`；下拉 `.18s ease-out`；抽屉 `.26s var(--ease-slide)`。
- `prefers-reduced-motion` 降级块在 `tokens.css`，全站共用，不要在页面里重写。

## A8. 响应式断点

| 断点 | 行为 |
|---|---|
| `>1280px` | 主区两列（内容 + 320px 右栏），概览卡 4 列 |
| `≤1280px` | 右栏并入下方（先 2 列再 1 列） |
| `≤1100px` | 顶栏主导航/用户名隐藏、显示汉堡；侧边栏变抽屉 |
| `≤900px` | 概览卡/费用格 2 列；搜索键帽隐藏 |
| `≤640px` | 单列；安全区兜底 |
