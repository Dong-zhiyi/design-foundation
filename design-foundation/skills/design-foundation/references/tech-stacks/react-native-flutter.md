# React Native 与 Flutter

两个主流的跨端 App 框架。设计基准接近原生，但**组件要按框架的规范来**。

---

# React Native

> 来源：React Native 官方文档、`reactnative.dev`

## 设计基准

| 项目 | 数值 |
|---|---|
| 设计稿基准 | **375px**（或 375@2x） |
| 单位 | **dp / pt**（逻辑像素） |
| 像素密度 | 由系统处理（`PixelRatio`） |

## 关键尺寸

| 元素 | 数值 |
|---|---|
| 触控目标 | **≥ 44pt**（对齐 iOS）/ 48dp（对齐 Android） |
| 状态栏 | 由 Safe Area 决定 |
| 导航栏 | 自定义（RN 无系统导航） |
| 底部导航 | 自定义 |

## 样式限制（设计时要考虑）

- **只支持 Flexbox**（无 CSS Grid、无浮动）
- 无伪类（`:hover`、`:focus` 要用状态模拟）
- 无 `background-size: cover`（要用 `Image` 组件的 `resizeMode`）
- 动画用 `Animated` / `Reanimated`
- 阴影：iOS 和 Android 写法不同

**设计影响**：不要设计依赖 CSS 特性的效果（如复杂的渐变、多重阴影）。

## 常用 UI 库

| 库 | 说明 |
|---|---|
| **React Native Paper** | Material Design 风格 |
| **NativeBase** | 通用组件 |
| **Tamagui** | 跨端（RN + Web） |
| **React Native Elements** | 轻量 |

## 设计交付要点

- 基准 375px
- 明确触控目标 ≥44pt
- 说明哪些效果 RN 实现不了
- 与项目 UI 库规范对齐

---

# Flutter

> 来源：Flutter 官方文档、Material 3 规范

## 设计基准

| 项目 | 数值 |
|---|---|
| 设计稿基准 | **375px** 或 **360px** |
| 单位 | **逻辑像素（dp）** |
| 像素比 | `MediaQuery.devicePixelRatio` |

## 关键尺寸（Material 3）

| 元素 | 数值 |
|---|---|
| 触控目标 | **≥ 48dp** |
| Top App Bar | 64dp |
| Bottom Navigation | 80dp |
| Navigation Rail | 80dp |
| FAB | 56dp |
| 标准边距 | 16dp |

## 设计特点

- **Material Design 3** 是默认语言（圆角、阴影、色彩）
- 支持**动态取色**（从壁纸提取主题色）
- 组件自带**状态与动效**（Ripple、InkWell）
- 字体用 `sp`（随系统字号缩放）

**设计影响**：
- 按 Material 3 规范设计最省事
- 如果要强品牌风格，需要明确覆写 Material 默认值

## 常用 UI 库

| 库 | 说明 |
|---|---|
| **Material 3 组件** | 官方默认 |
| **Flutter Cupertino** | iOS 风格组件 |
| **GetWidget** | 第三方组件库 |
| **Fluent UI Flutter** | 微软 Fluent 风格 |

## 设计交付要点

- 基准 375 或 360px
- 触控目标 ≥48dp
- 明确是 Material 风格还是自定义
- 字号用 sp（随系统缩放）

---

## 两者对比

| 项目 | React Native | Flutter |
|---|---|---|
| 语言 | JavaScript / TypeScript | Dart |
| 设计基准 | 375px | 375 / 360px |
| 单位 | dp / pt | dp |
| 组件来源 | 社区库 | Material 3 官方 |
| 动效 | Animated / Reanimated | 内置（隐式动画） |
| 自定义程度 | 高 | 高 |
| 学习成本 | 前端友好 | 需学 Dart |

---

## 检查清单

- [ ] 框架已确认（RN / Flutter）
- [ ] 设计稿基准已确认（375 或 360）
- [ ] 触控目标达标（44pt / 48dp）
- [ ] 样式限制已了解（RN 不支持的特性）
- [ ] UI 库已确认并对齐
- [ ] 导航方案已确认（自己实现）
