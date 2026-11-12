# Android App（Material Design 3）

> 来源：Material Design 3 官方规范（m3.material.io）

---

## 一、画板基准

| 设备类型 | 逻辑尺寸 | 密度 |
|---|---|---|
| 普通手机 | 360×800 dp | 420dpi（xxhdpi） |
| 大屏手机 | 412×915 dp | 420dpi |
| 平板 | 800×1280 dp | — |

**设计稿基准建议：360×800dp**（Android 生态最通用的基准宽度）。

---

## 二、窗口尺寸分类（Window Size Classes）

Material 3 按**可用宽度**分类，而不是按设备型号：

| 分类 | 宽度范围 | 典型设备 | 布局策略 |
|---|---|---|---|
| **Compact** | < 600dp | 手机竖屏 | 单列，底部导航 |
| **Medium** | 600–839dp | 折叠屏展开、小平板 | 双列，Navigation Rail |
| **Expanded** | ≥ 840dp | 平板、桌面 | 多列，侧边导航栏 |

高度也分 Compact / Medium / Expanded。

**为什么重要**：折叠屏展开时宽度从 Compact 跳到 Medium，布局必须能响应。

---

## 三、关键尺寸

| 元素 | 数值 |
|---|---|
| **最小触控目标** | **48×48 dp** |
| Top App Bar（标准） | 64dp 高 |
| Top App Bar（大号） | 112–152dp |
| Bottom Navigation Bar | **80dp** 高 |
| Navigation Rail | 80dp 宽 |
| Navigation Drawer | 360dp 宽 |
| FAB | 56dp（普通）/ 40dp（小） |
| 标准边距 | 16dp |
| 栅格边距 | 16dp（Compact）/ 24dp（中大屏） |
| 列表项高度 | 56dp（单行）/ 72dp（双行）/ 88dp（三行） |

---

## 四、字体

- 系统字体：**Roboto**（中文回退系统默认，如思源黑体/小米兰亭）
- 正文基准：**14sp**
- Material 3 字阶（sp）：
  - display: 57 / 45 / 36
  - headline: 32 / 28 / 24 / 22
  - title: 22 / 16 / 14
  - body: 16 / 14 / 12
  - label: 14 / 12 / 11
- **sp 会随用户系统字号缩放**，布局要能伸缩

---

## 五、色彩

- 支持**深色模式**（必须适配）
- Material 3 用**动态取色**（从壁纸提取）+ **Tonal Palette（色调板）**
- 语义化角色：`primary` / `onPrimary` / `primaryContainer` / `secondary` / `tertiary` / `error` / `surface` / `background` / `outline`
- **不要写死 hex**，用角色 token

**Material 3 色彩层级**：
- 普通色（surface / onSurface）
- Vibrant 色（用于内容之上）
- Vibrant-Controls（用于控件）

---

## 六、材质与阴影

- Material 3 用**高度（Elevation）** + **光晕（Glow）** 表达层级
- 高度档：0 / 1 / 2 / 3 / 4 / 5 dp
- **Tonal Elevation**（iOS 26 的 Liquid Glass 是另一套：折射/色散/光角/磨砂强度）

---

## 七、导航模式

| 场景 | 组件 |
|---|---|
| 3–5 个平级板块 | **Bottom Navigation Bar**（80dp） |
| 5 个以上 / 板块可切换 | **Navigation Rail**（80dp 宽） |
| 大量板块 | **Navigation Drawer**（360dp 宽） |
| 页面内切换 | **Tabs** |
| 临时操作 | **FAB** |

**选择依据是板块数量和信息架构，不是好看。**

---

## 八、手势与系统交互

| 手势 | 行为 |
|---|---|
| 系统返回键 | 返回上一级（必须支持） |
| 侧滑返回（Gesture Nav） | 返回上一级 |
| 底部上滑 | 回主屏幕 |
| 长按 | 上下文菜单 |

**设计约束**：
- 不要覆盖系统返回行为
- 边缘手势区域不要放自定义交互
- 支持**返回预测**（Predictive Back）

---

## 九、平板与折叠屏适配

- 折叠屏展开 → Compact 变 Medium，要能双列
- 平板 → Expanded，用 Navigation Rail / Drawer
- **不要简单拉伸手机布局**，要重排信息密度

---

## 十、设计交付检查

- [ ] 画板 360×800dp
- [ ] 热区 ≥48×48dp
- [ ] 字号用 sp 档位（14 基准）
- [ ] 深色模式已适配
- [ ] 用色彩角色而非写死 hex
- [ ] 导航模式与信息架构匹配
- [ ] 平板/折叠屏布局已考虑
- [ ] 返回键行为正确
