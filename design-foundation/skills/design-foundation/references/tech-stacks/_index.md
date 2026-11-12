# 技术栈选型

**技术栈决定设计基准、单位、可发布的平台，以及组件从哪来。** 平台是「用户在哪用」，技术栈是「怎么实现」——两者要一起定。

---

## 选型决策树

```
要发布到哪些端？
├─ 只发小程序 ─────────────────────→ 原生小程序开发（微信/支付宝…）
├─ 小程序 + H5 + App 都要 ──────────→ 跨端框架
│    ├─ Vue 技术栈 ─────────────────→ uni-app
│    ├─ React 技术栈 ────────────────→ Taro
│    └─ 一套代码 + 原生体验要求高 ───→ Flutter
├─ 只做 H5（手机浏览器/内嵌）───────→ 纯 H5（Vue/React + Vite）
├─ 只做 App ───────────────────────→ 
│    ├─ React 技术栈 ────────────────→ React Native
│    ├─ Dart ───────────────────────→ Flutter
│    └─ 追求原生体验/平台特性 ──────→ 原生（SwiftUI / Kotlin）
└─ PC 网站 + 管理后台 ──────────────→ Web 框架（React/Vue/Next/Nuxt）
```

---

## 技术栈 × 平台 × 基准 对照表

| 技术栈 | 能发布到 | 设计稿基准 | 单位 | 关键尺寸 |
|---|---|---|---|---|
| **原生小程序** | 微信/支付宝等小程序 | 375px（固定）/ 390px（响应式） | rpx（750=屏宽） | 导航 128rpx · TabBar 98rpx · 胶囊 32pt |
| **uni-app x**（新一代） | iOS/Android/鸿蒙/H5/各小程序 | **390px（1x）** | **px** | 导航栏 **44px** · TabBar **50px** |
| **uni-app**（经典版） | 同上 | 750px（1px=1rpx）或 375px | rpx | 同上 |
| **Taro** | 小程序/H5/React Native | **750px**（默认） | px → rpx / rem / pt | 支持 750 / 640 / 828 |
| **纯 H5** | 手机浏览器 / 内嵌 WebView | **375px**（响应式）或 750px | rem / vw / px | 无系统导航，自己画 |
| **React Native** | iOS / Android | 375px 或 375@2x | dp / pt | 需自己实现导航 |
| **Flutter** | iOS/Android/H5/桌面 | 375px 或 360px | 逻辑像素（dp） | 需自己实现导航 |
| **原生 iOS** | iOS | 375 / 390 pt | pt | 导航栏 44pt · TabBar 49pt |
| **原生 Android** | Android | 360 dp | dp / sp | Top App Bar 64dp · Bottom Nav 80dp |
| **Web 框架** | 浏览器 | 1440px | px / rem | 内容最大宽 1200–1280px |

### Web 框架细分（选「Web 框架」时继续问）

| # | 框架 | 生态 | 常用 UI 库 |
|---|---|---|---|
| 1 | **React** | 最大 | Ant Design、MUI、shadcn/ui、Chakra UI、Mantine |
| 2 | **Vue 3** | 国内主流 | Element Plus、Ant Design Vue、Naive UI、Vant |
| 3 | **Next.js** | React 全栈 | 同 React + Tailwind |
| 4 | **Nuxt** | Vue 全栈 | 同 Vue + Tailwind |
| 5 | **Svelte / SvelteKit** | 新兴 | Skeleton、shadcn-svelte |
| 6 | **Angular** | 企业级 | Angular Material、NG-ZORRO |

详见 [web-frameworks.md](./web-frameworks.md)。

---

## 平台 × 技术栈 适配矩阵 ★

**这是最主要的过滤器。** 定了平台，可选技术栈就受限；反过来也一样。

| 平台 | 可选技术栈 | 备注 |
|---|---|---|
| 微信小程序 | 原生小程序 · **uni-app** · **Taro** | 三者最常用 |
| 支付宝小程序 | 原生 · uni-app · Taro | 有支付宝专属设计规范 |
| 抖音小程序 | 原生 · uni-app · Taro | — |
| 飞书小程序 | 原生 · uni-app · Taro | 飞书设计规范 |
| 百度小程序 | 原生 · uni-app · Taro | Smart Program |
| QQ 小程序 | 原生 · uni-app · Taro | — |
| 快手小程序 | 原生 · uni-app · Taro | — |
| **iOS App** | 原生 SwiftUI · **React Native** · **Flutter** · uni-app · Taro(RN) | — |
| **Android App** | 原生 Compose · **React Native** · **Flutter** · uni-app · Taro(RN) | — |
| **鸿蒙 HarmonyOS** | 原生 ArkTS · Flutter · uni-app | 华为 HIG |
| **H5** | 纯 H5 · uni-app · Taro · React · Vue | 基准 375px |
| **PC 网站** | React · Vue · Next.js · Nuxt · Svelte · Angular | 基准 1440px |
| **PC 客户端** | Electron · Tauri · Flutter Desktop · 原生 | — |
| **平板** | 同对应平台 + 大屏适配 | 不要等比放大 |
| **快应用** | 原生 · uni-app | 厂商联盟规范 |

**用法**：
- 用户说「做小程序」→ 平台收窄到 7 个小程序，技术栈只列 原生 / uni-app / Taro
- 用户说「做 App」→ 平台收窄到 iOS/Android/鸿蒙，技术栈列 原生 / RN / Flutter / uni-app
- 用户说「做 Web」→ 平台收窄到 H5 / PC 网站 / PC 客户端，技术栈列 Web 框架

---

## 技术栈 × UI 库 适配矩阵 ★

**UI 库是最强的过滤器**——定了技术栈，可选库就那么几个。

| 技术栈 | 可选 UI 库 |
|---|---|
| **uni-app** | uView UI · uni-ui（官方）· NutUI · Wot Design Uni |
| **Taro** | NutUI React · Taro UI · Ant Design Mobile · Taroify |
| **React Native** | React Native Paper · NativeBase · Tamagui · RN Elements |
| **Flutter** | Material 3（默认）· Cupertino · GetWidget · Fluent UI |
| **React** | Ant Design · MUI · shadcn/ui · Chakra UI · Mantine · Radix |
| **Vue 3** | Element Plus · Ant Design Vue · Naive UI · Vant（移动端）|
| **原生 iOS** | SwiftUI 原生组件 · UIKit |
| **原生 Android** | Material 3 组件（Compose / XML）|

**用法**：技术栈定了之后，**只列该栈的 UI 库**，不要列别家的。

---

## 平台 × 字体 限制矩阵

| 平台 | 字体限制 |
|---|---|
| **小程序（各平台）** | ⚠️ **只能用系统字体**（自定义字体需 base64 内嵌，体积大且部分平台受限） |
| **App（iOS/Android/鸿蒙）** | ✅ 可自定义字体（会增加包体积） |
| **H5 / Web** | ✅ 可自定义 Web Font（注意加载性能） |

**用法**：做小程序时，**不要推荐自定义字体**，直接说明限制。

> ⚠️ **重点 1**：**H5 的基准也是 375px**（移动端），不是 1440。1440 是 PC 网站的基准。设计 H5 时按 375 出稿，用 rem/vw 适配。
>
> ⚠️ **重点 2**：**uni-app 要先分清是 x 还是经典版**——x 用 **390px 1x + px**，经典版用 **750px + rpx**。官方（uni-app x）明确推荐「1x 画布，按 390px 出图」，且「推荐开发者尽量使用 px」，rpx 只用于需要随屏宽变化的宽度。

---

## 技术栈详细文档

- 📄 [uni-app.md](./uni-app.md) — Vue 跨端框架，**最常用**（含 x / 经典版差异）
- 📄 [taro.md](./taro.md) — React 跨端框架
- 📄 [h5.md](./h5.md) — 纯 H5
- 📄 [react-native-flutter.md](./react-native-flutter.md) — RN 与 Flutter
- 📄 [native.md](./native.md) — 原生 iOS / Android
- 📄 [web-frameworks.md](./web-frameworks.md) — Web 前端框架

---

## 技术栈对设计的影响

| 影响项 | 说明 |
|---|---|
| **设计基准** | uni-app x 用 390（1x）；经典 uni-app / Taro 用 750；H5 用 375；原生 App 用 390/360 |
| **单位** | rpx / rem / vw / dp / pt —— 标注要写清 |
| **导航** | 小程序有系统导航（胶囊）；uni-app 有 44px 导航；H5 要自己画 |
| **组件** | 跨端框架用自己的 UI 库（uView / NutUI / Taro UI） |
| **字体** | 小程序用系统字体；H5 可自定义 |
| **能力** | 小程序能力受限（如 backdrop-filter）；H5/App 更自由 |
| **动效** | 小程序性能受限；H5/App 自由度高 |

---

## 常见错误

| 错误 | 后果 | 正确做法 |
|---|---|---|
| 用 1440 基准设计 H5 | 手机上字太小、布局挤 | H5 用 375 基准 |
| 把经典 uni-app 的 750 基准套到 uni-app x | 尺寸全错 | 先问清是 x 还是经典版 |
| 在 uni-app x 里大量用 rpx | 大屏字体过大、性能差 | x 推荐 px，rpx 只用于弹性宽度 |
| 跨端项目只做一套尺寸 | 小程序和 App 表现差异大 | 一套设计系统 + 各端适配 |
| 忽略技术栈的组件限制 | 设计了实现不了的交互 | 开工前查技术栈的组件能力 |
| 标注只写 px 不写单位 | 开发不知道用 rpx 还是 rem | 标注里写清目标单位 |

---

## 与设计流程的关系

```
第 1 步：定平台（用户在哪用）
第 2 步：定技术栈（怎么实现）★ ← 这里
        ↓
设计基准、单位、导航、组件库都随之确定
        ↓
第 3–9 步：需求 → 风格 → 架构 → 字体图标 → 简报 → 门禁 → 落地
```

**技术栈和技术选型要在同一步确认**，因为它们共同决定了后续所有取值。

---

## 检查清单

- [ ] 技术栈已确认（uni-app / Taro / H5 / RN / Flutter / 原生）
- [ ] 设计稿基准已确认（750 还是 375）
- [ ] 目标单位已确认（rpx / rem / vw / dp / pt）
- [ ] 导航方案已确认（系统导航 / 自定义）
- [ ] 组件库已确认（用框架自带的还是自建）
- [ ] 标注里写清了单位和换算关系
