# E. 浮层与反馈 · F. 页脚

> 浮层是**唯一允许毛玻璃的层级**：`blur + border + shadow` 三件套缺一不可（红线 2）。

---

## E1. 下拉浮卡（通知 / 用户）

```css
.ctop__menu {
  position: fixed; top: calc(var(--ctop-h) + 8px); width: 300px;
  background: rgba(255,255,255,.96);
  backdrop-filter: blur(14px);
  border: 1px solid var(--border-light);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-float);
  padding: 8px; z-index: 95;
  animation: menu-in .18s ease-out;      /* translateY(-6px)→0 + opacity */
}
.ctop__menu--notice { right: 150px; }
.ctop__menu--user   { right: 16px; }
```

- 通知浮卡：头部分隔线 + `3 UNREAD` 标注 → 通知项（状态点 + 标题 13px/700 + 说明 12.5px，hover `--bg-alt`）→ 底部「查看全部」蓝链接。
- 用户浮卡：头像卡（38px 头像 + 用户名 + UID）→ 菜单项 40px / 12px 圆角 / hover `--bg-alt` → 退出登录（红字，hover `--danger-soft` 底）。
- **交互契约**：点按钮开、点外部关、`Esc` 关、同步 `aria-expanded`；多个下拉互斥；菜单内点击不冒泡关闭。
- 定位用 `fixed` + 右对齐，不要相对按钮绝对定位（避免被裁切）。

## E2. 抽屉（移动端侧边栏 / 详情面板）

- 白底 + `--shadow-float` + `border-right`；入场 `translateX(-100%) → 0`、`.26s var(--ease-slide)`。
- 遮罩 `rgba(15,23,42,.42)`，点遮罩关闭；打开锁 `body` 滚动。
- 贴底内容必须 `env(safe-area-inset-bottom)`。

## E3. Toast / 确认弹窗（规范预留）

- **Toast**：白卡 + 蓝边 + `--shadow-float`；状态点标语义（成功绿/预警橙/错误红）；右下角或顶部居中；
  fixed 贴底必须带安全区；自动消失时长 ≥ 3s，错误类不自动消失。
- **确认弹窗**：380–480px 白卡 16px 圆角 + 遮罩；标题 + 说明 + 按钮组（取消=副 CTA，确认=主 CTA）；
  **危险操作确认按钮用红底白字胶囊**（`--danger`），并在文案中写明后果。
- 弹窗内表单遵循 D 区输入框规范；Esc 关闭、点遮罩关闭（表单填写中除外）。

## E4. 骨架屏（规范预留）

- `--bg-alt` 色块按真实尺寸占位（卡片按卡、表格按行高 44px 铺 6 行、右栏按块）。
- 禁满屏 spinner；骨架与内容的布局必须一致，避免加载完成后跳动。
- 可加轻微呼吸动画（opacity），并在 reduced-motion 下关闭。

---

# F. 页脚（slim legal bar）

控制台页脚**只有一条**，禁止搬落地页四层页脚：

```html
<p class="cfoot">
  © 2026 MiniRelax · <a>服务协议</a> · <a>隐私政策</a> · <span data-config="icp">沪ICP备00000000号</span>
  <span class="cfoot__pad"></span>  <!-- height: env(safe-area-inset-bottom) -->
</p>
```

- 12px `--text-4`；链接 hover 变蓝；ICP 等白标字段走 `/api/v1/site-config`，空值自动 `hidden`。
- 内容居左即可，不居中、不分列、不加背景色带。
