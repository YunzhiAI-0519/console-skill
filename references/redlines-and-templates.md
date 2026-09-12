# G. 十二条红线 · H. 使用模板 · 交付自检清单

## G. 十二条红线

1. **灰画布 + 白卡片**：白卡必须带边框，禁无边框白卡压灰底。
2. **固定 chrome 不做浮起**：顶栏/侧栏贴边 + border + `--shadow-chrome`；**只有浮层**（下拉/抽屉/弹窗）
   用 `blur + border + shadow` 三件套；不要给侧边栏加毛玻璃、不要把顶栏做成悬浮胶囊。
3. 禁黑色阴影；阴影一律蓝主色系 / 中性蓝灰，大距离低透明度。
4. 禁 `transition: all`；禁过渡 `width / height / gap / top / left` 等布局属性——位移与增长一律 `transform`。
5. 数字必须 `tabular-nums`；表格 ID / IP 必须 mono；金额对齐能看出小数位。
6. 操作列右对齐；表格行必须 hover 反馈；空态必须给文案，禁白板。
7. 含长文本的 flex/grid 子项必须 `min-width: 0`（表格容器、菜单、卡片、侧边栏条目）。
8. **密度红线**：卡片 padding ≤ 18px、区块间距 16px、悬浮 ≤ 2px；
   禁止 88–96px 大留白、视差、跑马灯、打字机等营销动效。
9. 状态语义固定：绿=运行/正常，橙=即将到期/预警，灰=停止/无，红=紧急/危险；不得自造语义色。
10. 颜色仅来自变量表；图表新增色先在 tokens 扩展块集中注册（`--chart-*`）。
11. 浮层遵循「点外关闭 + Esc + aria-expanded」；多个下拉互斥；危险操作必须二次确认。
12. 所有交互元素有 hover / focus-visible / active 三态；`prefers-reduced-motion` 必须降级；
    fixed / 贴底元素必须处理 `env(safe-area-inset-bottom)`。

---

## H. 一句话使用模板

> 「基于 MiniRelax 控制台设计语言，为用户中心新增 XX 页面/模块：遵循 A 画布模型（灰底白卡）+ B 顶栏 +
> C 侧边栏产品树 + D 面板/表格/右栏规范 + E 浮层三件套；密度按 A4、动效按 A7，红线 G 全部满足；
> 白标字段走 `/api/v1/site-config`，数字一律 tabular-nums，表格 ID/IP 用 mono。」

示例：

- 「新增**数据库实例列表页**：复用 D4 表格 + D3 用量条 + tabs 筛选，状态语义按 G9，操作列首项『登录』。」
- 「新增**工单详情页**：左侧信息面板 + 右侧对话流，按钮/徽章/状态点沿用 A6。」
- 「新增**费用账单页**：D6 指标格 + 分段构成条 + 月度明细表格，导出按钮放 `panel__tools`。」
- 「新增**告警中心**：D4 表格 + E3 Toast，紧急级用 `--dot-warn` / 红点，禁自造新色。」

---

## 交付自检清单

**画布与布局**
- [ ] body 用 `--bg-alt` 灰底，所有内容在带边框白卡上
- [ ] 顶栏 56px 固定贴边 + border + `--shadow-chrome`，未做毛玻璃/胶囊
- [ ] 侧边栏 240px（可收起 64px），激活态有 3px 指示条
- [ ] 主栅格 `minmax(0,1fr) + 320px`，≤1280px 正确降列
- [ ] 所有含长文本的子项都有 `min-width: 0`

**组件**
- [ ] 概览卡数字 `24px/800 tabular`，图标芯片 38px，delta 用语义色
- [ ] 表格：表头灰底、行 hover、状态点带外环、ID/IP mono、操作列右对齐、有空态
- [ ] tabs 激活白底蓝字 + `--shadow-panel`；分页当前页蓝边加粗
- [ ] 进度条预警态用 `--grad-warn`；构成条用 `--chart-1..4`
- [ ] 浮层三件套齐全；危险项红色 + `--danger-soft` hover；交互契约完整

**红线 G**
- [ ] 无黑色阴影；无 `transition:all`；无布局属性过渡（width/gap…）
- [ ] 无营销动效（视差/跑马灯/打字机/大悬浮）
- [ ] 状态语义未自造；无未注册新色
- [ ] 数字 tabular、ID/IP mono
- [ ] 危险操作有二次确认

**可访问性 / 响应式**
- [ ] 三态齐全（hover / focus-visible / active）
- [ ] `prefers-reduced-motion` 降级生效（未在页面重写，走 tokens.css）
- [ ] fixed/贴底元素处理了 `env(safe-area-inset-bottom)`
- [ ] ≤1100px 侧边栏可开合、汉堡可用；≤640px 单列不横向滚动

**数据**
- [ ] 白标字段走 `/api/v1/site-config`，空值自动 `hidden`，无硬编码品牌信息
