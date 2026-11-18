# 精确动效数值参考

**每种风格的动效参数都有精确值，不是范围。** `cubic-bezier(0.2, 0, 0, 1)` 不是 `cubic-bezier(0.4, 0, 0.2, 1)`，`0.96` 不是 `0.95`。

> 来源：`jakubkrehel/skills` → `better-ui` SKILL.md（精确动效值体系）

---

## 一、通用精确值（所有风格共享）

以下数值是 UI 动效的「物理定律」，不因风格而变：

| 参数 | 精确值 | 说明 |
|---|---|---|
| **按下缩放** | `scale(0.96)` / `150ms` / `ease-out` | 按钮按下反馈；禁用态不缩放 |
| **高频交互** | `≤150ms`，仅 `opacity` 或 `background-color` | 行悬停、Tab 切换、键盘反馈 |
| **入场动画** | `opacity + blur(4px) + translateY(12px)` / `300ms` / `ease-out` |  staged entrance 的基础形态 |
| **退场动画** | `opacity + blur(4px) + translateY(-12px)` / `150ms` / `ease-out` | 退场比入场更短更小 |
| **交错间隔** | `100ms` | 多个元素依次入场时的间隔 |
| **图标交叉淡入** | `scale(0.25→1) + opacity(0→1) + blur(4px→0)` / `300ms` | play↔pause、copy↔copied 等状态图标切换 |
| **标准缓动** | `cubic-bezier(0.2, 0, 0, 1)` | 通用 UI 缓动曲线 |
| **弹簧缓动** | `{ type: "spring", duration: 0.3, bounce: 0 }` | 图标切换用弹簧时 bounce 始终为 0 |
| **主题切换** | 禁用所有过渡 → 强制样式刷新 → 下一帧恢复 | 防止颜色过渡同时触发导致「涂抹」 |
| **过渡属性** | 只命名变化的属性（`scale, opacity`），不用 `all` | 性能 + 可预测性 |
| **悬停效果** | 仅在 `@media (hover: hover)` 下生效 | 触摸设备上 `:hover` 会卡住 |

### prefers-reduced-motion 降级

| 正常模式 | 降级模式 |
|---|---|
| 移动、缩放、模糊 | 仅 `opacity` 交叉淡入 |
| 视差滚动 | 完全禁用 |
| 自动播放 | 完全禁用 |
| 按下缩放 `scale(0.96)` | **保留**（短暂反馈，不算位移） |
| Loading 旋转器 | **保留**（功能性动画） |

---

## 二、16 种风格的动效参数速查

| 风格 | 入场曲线 | 退场曲线 | 按下反馈 | 特色动效 | 缓动 |
|---|---|---|---|---|---|
| **1. 经典剧院** | `opacity + translateY(8px)` / `400ms` | `opacity` / `200ms` | `scale(0.97)` / `180ms` | 金色描边渐显 | `cubic-bezier(0.25, 0.1, 0.25, 1)` |
| **2. 现代深色** | `opacity + translateY(4px)` / `200ms` | `opacity` / `120ms` | `scale(0.98)` / `100ms` | 骨架屏脉动 | `cubic-bezier(0.2, 0, 0, 1)` |
| **3. 简约浅色** | `opacity` / `150ms` | `opacity` / `100ms` | `background-color` / `100ms` | 无装饰动画 | `ease-out` |
| **4. 玻璃拟态** | `opacity + blur(0→20px) + scale(0.98→1)` / `350ms` | `opacity + blur(20px→0)` / `200ms` | `scale(0.97)` / `150ms` | 玻璃呼吸（blur 脉动） | `cubic-bezier(0.2, 0, 0, 1)` |
| **5. 新粗野主义** | `translateY(16px→0) + rotate(-2°→0°)` / `250ms` | `translateY(0→8px)` / `100ms` | `translate(2px, 2px)` / `80ms` | 硬阴影位移 | `cubic-bezier(0.34, 1.56, 0.64, 1)` |
| **6. 黏土拟态** | `scale(0.9→1) + opacity` / `400ms` | `scale(1→0.95)` / `200ms` | `scale(0.95)` / `150ms` | 弹跳球物理 | `cubic-bezier(0.34, 1.56, 0.64, 1)` |
| **7. 柔 UI** | `opacity + boxShadow 变化` / `300ms` | `opacity` / `150ms` | `boxShadow inset 加深` / `100ms` | 凹陷呼吸 | `ease-in-out` |
| **8. Bento Grid** | `opacity + scale(0.95→1)` / `250ms` | `opacity + scale(1→0.97)` / `120ms` | `scale(0.98)` / `100ms` | 格子依次亮起 | `cubic-bezier(0.2, 0, 0, 1)` |
| **9. 编辑杂志** | `opacity + translateY(12px)` / `500ms` | `opacity` / `200ms` | `opacity(0.7)` / `100ms` | 文字逐行渐显 | `cubic-bezier(0.25, 0.1, 0.25, 1)` |
| **10. 终端 TUI** | `opacity` / `50ms` | `opacity` / `50ms` | `background-color invert` / `50ms` | 光标闪烁 `█` | `linear` 或无过渡 |
| **11. 赛博朋克** | `opacity + glitch(3帧)` / `300ms` | `opacity + glitch(2帧)` / `150ms` | `scale(0.96) + glow` / `100ms` | 霓虹闪烁 / 字符雨 | `cubic-bezier(0.2, 0, 0, 1)` |
| **12. 日系极简** | `opacity` / `600ms` | `opacity` / `400ms` | `opacity(0.6)` / `200ms` | 墨晕展开 | `cubic-bezier(0.25, 0.1, 0.25, 1)` |
| **13. 斯堪的纳维亚** | `opacity + translateY(8px)` / `350ms` | `opacity` / `200ms` | `scale(0.98)` / `150ms` | 柔和脉冲 | `cubic-bezier(0.25, 0.1, 0.25, 1)` |
| **14. 包豪斯** | `opacity + rotate(0°)` / `200ms` | `opacity` / `100ms` | `translate(2px, 2px)` / `80ms` | 几何旋转 | `cubic-bezier(0.4, 0, 0.2, 1)` |
| **15. 国风宋墨** | `opacity + scale(0.97→1)` / `500ms` | `opacity` / `300ms` | `opacity(0.7)` / `150ms` | 笔触绘制 / 印章盖下 | `cubic-bezier(0.25, 0.1, 0.25, 1)` |
| **16. AI Native UI** | `opacity + translateY(8px)` / `250ms` | `opacity` / `120ms` | `scale(0.98)` / `100ms` | 流式打字 + 光标 | `cubic-bezier(0.2, 0, 0, 1)` |

---

## 三、动效时长分级

| 级别 | 时长 | 适用场景 |
|---|---|---|
| **即时** | `≤50ms` | 终端 TUI 状态切换、光标闪烁 |
| **快速** | `100–150ms` | 高频交互（悬停、Tab 切换、按下反馈） |
| **标准** | `200–300ms` | 大多数 UI 状态变化（入场、退场、图标切换） |
| **舒缓** | `350–500ms` | 编辑杂志、日系极简、国风宋墨等慢节奏风格 |
| **戏剧性** | `500ms+` | 仅限首次加载的 staged entrance，不用于常规交互 |

---

## 四、检查清单

- [ ] 按下反馈 `scale(0.96)` 或风格等效值，禁用态不缩放
- [ ] 高频交互 `≤150ms`，仅 `opacity` 或 `background-color`
- [ ] 退场比入场短（退场 `150ms` vs 入场 `300ms`）
- [ ] 交错入场间隔 `100ms`
- [ ] 图标状态切换有交叉淡入（不是硬切）
- [ ] 过渡只命名变化属性，不用 `all`
- [ ] 悬停效果在 `@media (hover: hover)` 内
- [ ] `prefers-reduced-motion` 有降级方案
- [ ] 主题切换时禁用过渡再恢复
- [ ] 动效时长与风格匹配（查上方速查表）
