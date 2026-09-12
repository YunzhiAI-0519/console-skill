# B. 控制台顶栏 · C. 侧边栏产品树

> 两者都是**固定 chrome**：贴边 + border + 极淡阴影，**不做浮起、不加毛玻璃**（红线 2）。

---

# B. 顶栏（56px 固定）

## B1. 定位

```css
.ctop {
  position: fixed; top:0; left:0; right:0; height: var(--ctop-h); /* 56px */
  background: var(--bg);
  border-bottom: 1px solid var(--border-light);
  box-shadow: 0 1px 0 rgba(15,23,42,.02), 0 2px 10px rgba(15,23,42,.03);
  z-index: 90;
}
.cbody { padding-top: var(--ctop-h); }   /* 内容区让位 */
```

## B2. 结构分区

```
[logo 芯片 30px·圆角8 + 品牌名 16px/800] ┊ [控制台]  产品 解决方案 最新活动 成长体系
              ………  全局搜索（flex:1, max-width:460px, margin-inline:auto）  ………
[文档][工单][🔔红点] ┊ [余额胶囊] [头像+用户名+▾] [汉堡 ≤1100px]
```

| 分区 | 规格 |
|---|---|
| 品牌区 | logo 芯片 30px / 圆角 8px / `--ink` 底白图标；品牌名 16px/800；1px 竖分隔线；站点名 13px `--text-3` |
| 主导航 | 13–14px 文字链，激活**蓝字即可**（不用白底浮起胶囊）；hover `--primary-soft` 底 |
| 图标按钮 | 36px 圆形无边框；hover `--primary-soft` 底 + 蓝；铃铛未读点 7px 红（`--dot-danger`）+ `0 0 0 2px var(--bg)` |
| 余额胶囊 | 高 32px、`--primary-soft` 底；`余额` 12px `--text-3` + 数字 13px/700 tabular 蓝字；hover `-1px` + `--shadow-panel`；点击跳费用中心 |
| 用户胶囊 | 高 36px 透明底 hover `--bg-alt`；28px 渐变圆头像（`--grad-brand`）+ 用户名 `max-width:120px` 截断 + 下拉箭头 |

响应式：≤1100px 主导航与用户名隐藏、显示汉堡（36px 圆）。

## B3. 全局搜索

- 胶囊、高 38px、`--bg-alt` 灰底；放大镜绝对定位 `left:14px`；右侧 `Ctrl K` 键帽（11px 边框小方块，≤900px 隐藏）。
- focus：白底 + `--primary-border` + `0 0 0 3px var(--primary-soft)`。
- JS：`Ctrl/Cmd + K` 聚焦，拦截默认行为。

## B4. 顶栏下拉浮卡

- 见 `overlays.md` E1：300px、blur+border+shadow、右对齐（通知 `right:150px`、用户 `right:16px`）。
- 通知项：状态点 + 标题 13px/700 + 说明 12.5px；头部分隔线 + `3 UNREAD` 标注；底部「查看全部」蓝链接。
- 用户菜单：头像卡（38px 头像 + 用户名 + `UID 100086`）→ 菜单项 40px（费用中心/账户设置/访问管理）→ 退出登录（红，hover `--danger-soft`）。

---

# C. 侧边栏（产品树）

## C1. 定位与尺寸

```css
.sidebar {
  width: var(--side-w);           /* 240px */
  background: var(--bg);
  border-right: 1px solid var(--border-light);
  position: sticky; top: var(--ctop-h);
  height: calc(100dvh - var(--ctop-h));
  overflow-y: auto; overflow-x: hidden;
}
```

## C2. 分组标题

`11px / 700 / .1em / uppercase / --text-4`，`margin:14px 10px 6px`，`white-space:nowrap`。
分组：计算 / 数据库 / 存储与网络 / 运维与安全 / 费用。

## C3. 条目

- 高 38px、圆角 12px、`gap:10px`、`margin-bottom:2px`、`white-space:nowrap`。
- 图标 18px + 文本（`flex:1; min-width:0; ellipsis`）+ 可选**计数徽标**。
- 计数徽标：`11.5px` 胶囊，`--bg-alt` 底 + `--border-light` 边 + `--text-4`；激活态转蓝边蓝字白底。
- hover：`--bg-alt` 底 + 蓝字。

## C4. 激活态

`--primary-soft` 底 + 蓝字 600 + **左侧 3px 蓝色指示条**（`top/bottom:8px`，右侧圆角 2px）。
「当前页」靠底色 + 指示条双信号，不靠加粗硬撑。

## C5. 收起态（桌面）

- `.is-collapsed`：240px → `64px`；隐藏 label / 计数 / 分组标题，仅剩图标列。
- 底部 `border-top` 分隔线上放「收起/展开」按钮（44px，图标 + 文本）。
- 收起用瞬时切换，**不要过渡 width**（红线 4）。

## C6. 移动端抽屉（≤1100px）

```css
.sidebar { position: fixed; top: var(--ctop-h); bottom: 0; left: 0; z-index: 84;
           transform: translateX(-100%); transition: transform .26s var(--ease-slide); }
.sidebar.is-open { transform: none; box-shadow: var(--shadow-float); }
.side-drawer__mask { position: fixed; inset: 0; background: rgba(15,23,42,.42); }
```

- 面板内边距：`calc(var(--ctop-h) + 8px) 10px calc(16px + env(safe-area-inset-bottom))`。
- 打开时锁 `body` 滚动；点条目或遮罩关闭；点条目同时更新激活态。
- 汉堡在顶栏内，同步 `aria-expanded`。
