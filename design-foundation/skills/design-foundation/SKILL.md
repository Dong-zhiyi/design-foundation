---
name: design-foundation
description: "产品设计前的基座工作流：先定平台（微信小程序 / iOS App / Android App / 网站 / H5）和技术栈（uni-app / Taro / React Native / Flutter / 原生 / 纯 H5），再选设计风格、设计架构、字体库与图标库，讨论清楚需求，过质量门禁，验证 Figma MCP 连通性并确认目标文件，最后落地成设计稿。当用户提到 做设计/设计稿/界面/页面/原型/Figma/选型/风格/视觉规范/设计规范/小程序界面/App 界面/网站页面/H5 页面/设计系统/设计令牌/图标库/字体库/uniapp/taro，或要给某个产品出设计方案时，都应使用本技能——即使用户没有明确说「设计」二字。也适用于：评审设计稿、检查设计稿有没有 AI 味、给已有项目补充设计规范。"
---

# Design Foundation（设计基座）

在动手画之前，先把**平台、技术栈、风格、架构、字体、图标**定下来，把需求聊清楚，过一遍质量门禁，确认 MCP 通了、文件对了，再落地。

这个技能的价值不在于替你做审美判断，而在于**让每次设计都从同一套经过验证的知识库里取值**，而不是每次现编。

---

## 先读这两句

**1. 平台 + 技术栈永远是第一决策。**
小程序有胶囊按钮要避让、导航 128rpx；uni-app 导航只有 44px、TabBar 50px；H5 基准也是 375px 但要自己画导航；App 有 Safe Area 和 44/48pt 热区；PC 网站是 1440。**平台没定、技术栈没定，就谈风格，等于先选壁纸再决定盖几层楼。**

**2. 落地前必须验证 MCP 连通性、确认目标文件。**
MCP 没通就开画，画完发现写不进去；文件没确认就开画，画在错的文件里。**这两件事在动手前 30 秒就能确认完。**

---

## 什么时候用

- 用户要做界面 / 页面 / 原型 / 设计稿 / 视觉方案
- 用户要选型：平台、技术栈、风格、架构、字体、图标库
- 用户要评审或改进现有设计稿（尤其是「有没有 AI 味」）
- 用户要给项目建立或补充设计规范

## 什么时候不用

- 只是改一个颜色、调一个间距 → 直接改，不用走全流程
- 纯代码问题（布局 bug、样式不生效）→ 不是设计问题
- 用户明确说「别问了，直接画」→ 跳过讨论，但**平台、技术栈、MCP 连通性、目标文件还是要确认**

---

## 选项怎么列：先收窄，再列全

**不要无脑列出所有选项。** 列全的前提是「在相关范围内列全」——先根据已知信息收窄，再列全该范围。

### 收窄依据

| 依据 | 例子 |
|---|---|
| **用户已经说了什么** | 用户说「做个小程序」→ 只列小程序平台，不列 iOS/Android/Web |
| **产品类型** | B 端工具 → 不列抖音小程序；潮玩 → 不列政务平台 |
| **技术栈** | 定了 uni-app → 平台只能是它能发布的；定了 React → UI 库只列 React 生态 |
| **团队能力** | 团队不会 Dart → 不主推 Flutter |

### 三步走

1. **收窄**：根据已知信息确定相关范围
2. **列全**：在该范围内**列全**（不要只给 2–3 个）
3. **说明**：明确告诉用户**排除了什么、为什么**

### 示例

❌ **错误**：用户说「做个 Web 后台」，你列了 15 个平台

✅ **正确**：
```
你要做 Web 端，平台就是 PC 网站（1440 基准）。
技术栈我列一下 Web 相关的：
  1. React   2. Vue 3   3. Next.js   4. Nuxt
  5. Svelte / SvelteKit   6. Angular
你用哪个？
（小程序和 App 的平台就不列了，除非你也要做）
```

### 各环节的过滤规则

| 环节 | 根据什么过滤 |
|---|---|
| **平台** | 用户说的话、产品类型、技术栈能发布的范围 |
| **技术栈** | 平台、团队技术背景、项目规模 |
| **风格** | 平台适配性（如强风格不适合后台）、产品类型 |
| **架构** | 项目规模、团队人数 |
| **字体** | 平台限制（小程序只能用系统字体）、技术栈 |
| **图标** | 技术栈（小程序要切图，Web/App 可用 SVG） |
| **UI 库** | **技术栈**（最强约束，定了框架才能选库） |

> 💡 **UI 库是最强的过滤器**：定了技术栈，可选的 UI 库就那么几个。见 `references/tech-stacks/_index.md` 的适配矩阵。

---

## 工作流（11 步 + 2 个检查点）

### 第 1 步：定平台（必做，不可跳过）

读 `references/platforms/_index.md`。**先收窄，再列全**（见上文「选项怎么列」）：

- **用户已经说了平台**（如「做个小程序」「做 App」）→ 只列该类别，并说明其他类别不列
- **用户没说** → 列全部 15 个：

```
1. 微信小程序        8. iOS App
2. 支付宝小程序      9. Android App
3. 抖音小程序       10. 鸿蒙 HarmonyOS
4. 飞书小程序       11. PC 客户端（macOS/Windows）
5. 百度小程序       12. H5（手机浏览器/WebView）
6. QQ 小程序        13. PC 网站 / 管理后台
7. 快手小程序       14. 平板（iPad/Android 平板）
                    15. 快应用
```

然后问：**是否多端都要**（例如小程序 + App）？

**产出**：平台 ID + 该平台的规范文档。

> ⚠️ **H5 的基准也是 375px**，不是 1440。1440 是 PC 网站的基准。别搞混。
> ⚠️ 标 ⚠️ 的平台（支付宝/抖音/飞书等）没有独立文档 → 查该平台官方规范，或按「参照微信」处理并**明确告知用户**。

### 第 2 步：定技术栈（与平台同时确认）

读 `references/tech-stacks/_index.md`。**根据第 1 步的平台收窄，再在范围内列全**（见适配矩阵）：

- 平台是**小程序** → 只列：原生小程序 / uni-app x / 经典 uni-app / Taro
- 平台是 **iOS/Android/鸿蒙** → 只列：原生 / React Native / Flutter / uni-app / Taro(RN)
- 平台是 **H5** → 只列：纯 H5 / uni-app / Taro / React / Vue
- 平台是 **PC 网站** → 只列：React / Vue / Next.js / Nuxt / Svelte / Angular
- 平台是 **PC 客户端** → 只列：Electron / Tauri / Flutter Desktop / 原生
- 拿不准 → 列全部 10 个：

```
1. 原生小程序          6. React Native
2. uni-app x（新）     7. Flutter
3. 经典 uni-app        8. 原生 iOS（SwiftUI / UIKit）
4. Taro               9. 原生 Android（Compose / XML）
5. 纯 H5              10. Web 框架（React / Vue / Next / Nuxt / Svelte / Angular）
```

**必须多问的两句**：
- **如果是 uni-app，是 x 还是经典版？**（基准完全不同：x 是 390px + px，经典是 750px + rpx）
- **项目用了哪个 UI 库？**（uView / NutUI / Ant Design / Element Plus / MUI / shadcn…）

**为什么必须问**：技术栈决定设计基准、单位、导航高度、可用组件。

| 技术栈 | 基准 | 单位 | 导航 | TabBar |
|---|---|---|---|---|
| 原生小程序 | 375px | rpx | **128rpx** | **98rpx** |
| **uni-app x**（新一代） | **390px（1x）** | **px** | **44px** | **50px** |
| **uni-app**（经典版） | 750px | rpx | **44px** | **50px** |
| **Taro** | 750px | px→rpx/rem/pt | 自己实现 | 自己实现 |
| **纯 H5** | **375px** | rem/vw | 自己画 | 自己画 |
| 原生 iOS | 390pt | pt | 44pt | 49pt |
| 原生 Android | 360dp | dp/sp | 64dp | 80dp |
| PC Web | 1440px | px/rem | 自定义 | 无 |

**必须多问一句**：用 **uni-app x 还是经典 uni-app**？
- **uni-app x**（新一代）→ 基准 **390px（1x）**，单位 **px**
- **经典 uni-app** → 基准 **750px**，单位 **rpx**

**产出**：技术栈 ID + 对应文档 + 设计基准 + 单位。

### 第 3 步：澄清需求

读 `references/requirements/brief-template.md`，挑最影响设计决策的 4–6 个问：

- 产品是做什么的？给谁用？
- 核心使用场景是什么（碎片化浏览 / 深度阅读 / 高频操作）？
- 内容从哪来（UGC / PGGC / 商品 / 工具）？
- 品牌有没有既定的调性或参考对象？
- 这次要做哪些页面？

信息够了就往下走，别为了填模板而填模板。

### 第 4 步：选风格

读 `references/styles/_index.md`，**把全部风格列出来让用户选**，不要只给 3 个。

**列表格式**（一行一个，简洁）：
```
1. 经典剧院（红金）— 庄重有仪式感，适合剧院/文博/高端品牌
2. 现代深色 / Linear — 克制精确，适合开发者工具/数据产品/SaaS
3. 简约浅色 / Swiss — 理性清晰，适合工具/后台/官网
...
```

**可以在列表前给一句推荐**（例如「根据你的产品，建议重点看 2、5、9」），但**必须列全**，选择权交给用户。

用户选定后读那张风格卡片，把它的色板 / 字体 / 圆角 / 阴影 / 布局规则带进后面的简报。

用户说「都不对」→ 按 `02-how-to-add.md` 去 GitHub 现搜新风格。

### 第 4.5 步：参考提取（可选但推荐）

读 `references/figma-delivery/reference-extraction.md`。**选完风格后，把「形容词」转成「数值」**：

- 问用户要 **1–3 个参考**（网址 / 截图 / 现成 Figma）
- 网址 → 用 `browser-use` skill 打开截图 + 读计算样式（主色/字阶/圆角/间距/密度）
- 截图 → 读图提取同样的数值
- Figma → 用 `framelink-mcp` 读 + `token_map` 映射
- 产出「品牌提取板」（**带数值**），写进第 7 步的《设计规范简报》，**注明来源**

**没给参考**：用风格卡默认值，并**明确告诉用户**「没给参考，用的是风格卡默认值」。**不要假装提取过。**

> ⚠️ **提取的是「数值和规律」，不是「照抄布局」。** 学色板/字阶/密度，不抄版式/文案/图片。

### 第 5 步：选架构

读 `references/architectures/_index.md`，**把全部架构列出来让用户选**（可以附一句推荐）：

```
1. 分层令牌 — primitive → semantic → component（底座，几乎必选）
2. 原子设计 — atom → molecule → organism → template → page
3. 领域分层 — Foundation / Patterns / Features
4. 单包 vs 多包 — 代码仓库怎么组织
```

**说明**：1 是地基，2/3/4 是上层选择，可以叠加。

### 第 6 步：选字体与图标库

读 `references/libraries/fonts.md` 和 `icons.md`，**把候选项列出来让用户选**（可按场景分组，但必须列全）。

**字体（13 个选项，含「默认」）——全部可商用**：

| # | 字体 | 风格 | 中文 | 许可 |
|---|---|---|---|---|
| 0 | ⊘ 默认字体 | 跟随平台 | ✅ | 无风险 |
| 1 | Inter | 中性 UI 标杆 | ❌ | SIL OFL 1.1 |
| 2 | IBM Plex Sans | 工程感 | ✅ | SIL OFL 1.1 |
| 3 | Noto Sans / Noto Sans SC | 中性 | ✅ | SIL OFL 1.1 |
| 4 | Source Han Sans（思源黑体） | 中性 | ✅ | SIL OFL 1.1 |
| 5 | Source Han Serif（思源宋体） | 标题/编辑感 | ✅ | SIL OFL 1.1 |
| 6 | Geist | 现代（Vercel） | ❌ | SIL OFL 1.1 |
| 7 | Public Sans | 政务 | ❌ | SIL OFL 1.1 |
| 8 | Space Grotesk | 几何科技 | ❌ | SIL OFL 1.1 |
| 9 | DM Sans | 圆润友好 | ❌ | SIL OFL 1.1 |
| 10 | Manrope | 现代圆润 | ❌ | SIL OFL 1.1 |
| 11 | JetBrains Mono | 等宽/代码 | ❌ | SIL OFL 1.1 |
| 12 | 阿里巴巴普惠体 | 字重丰富 | ✅ | 官方免费商用 |

> ⚠️ **列字体时打开/展示的预览地址 = 插件配置项 `font_preview_url`（字体样张页，见下方样张工具）**——一页看全 13 个候选字体，可输入**用户自己的真实文案**（如「歌剧《图兰朵》」）对比。**不要用它各字体的官网地址**：国内多数打不开（Google Fonts / fontsource / Adobe 等），且看不到中文渲染效果。
> ✅ **1–12 全部可商用、可随产品分发**（1–11 为 SIL OFL 1.1，12 为官方免费商用）。
> ⚠️ **系统字体（PingFang SC / 微软雅黑）不列入选项**——不能随产品分发，只能通过选项 0 引用。
> 📋 **许可证细节**见 `references/libraries/fonts.md` 的「许可证与商用」

**⭐ 字体样张工具（首选）**：
样张页是一个**独立的静态项目**，不在插件里，需要用户自己部署到服务器：
- 项目位置：字体样张项目是一个**独立的静态项目**（不在本插件仓库内），包含 `index.html` + `fonts/` 同目录（共约 62MB）
- 部署：整个目录上传到任意静态服务器，`index.html` 与 `fonts/` 必须同级（页面用相对路径 `./fonts/...` 加载字体）
- 插件配置项 `font_preview_url` 填部署后的地址（示例值 `https://your-domain.com/zcode-design-foundation-font-preview/`）
- 让用户看样张时，直接打开该地址即可；地址是示例值或打不开时，提示用户改成自己的

页面能力：13 个字体渲染真实文案（可自己输入）、快速对比网格、详细样张（字重/字号/中英文/数字）。
8 个拉丁字体走 CDN（`font.onmicrosoft.cn`），中文字体用项目里随附的 `fonts/`。

> ⚠️ **必须通过 http(s) 访问，不能直接双击 `index.html`**：浏览器把 `file://` 页面视为不透明来源（origin: null），会拦截 `@font-face` 加载**任何**本地字体文件——包括和页面同目录的（CVE-2019-11730 之后的 Chrome / Edge / Firefox 都这样）。本地想预览就在项目目录起个服务（`python -m http.server 8731`）再打开。

> ⚠️ **字体授权**：阿里巴巴普惠体 3.0 是官方免费商用（非 OFL），3.0 起已删除「不得上传、发布、转载字体文件」条款；公开 CDN 或随产品分发前请核对官方现行声明。详见 `zcode-design-foundation-font-preview/README.md`。

**字体在线预览**：**首选字体样张页**（`font_preview_url`）——一页对比 13 个字体 + 真实文案。
> ⚠️ **不要用各字体官网**：国内多数打不开（Google Fonts / fontsource / Adobe 等），且看不到中文渲染效果。
> 备选（样张页打不开时）：**Anyway.Fonts**（anyway.fm/fonts，中文预览+对比）、**字由**（hellofont.cn）、**字体天下**（fonts.net.cn）。
> 选型时**输入你的真实文案**（如「歌剧《图兰朵》」）预览，比看字样图准确。

**图标（11 个主要库，全部免费可商用）**——**每项标注 iconfont 支持 + Figma 插件 + 官方网址**：

| # | 图标库 | 风格 / 数量 / 许可 | iconfont | Figma 插件 | 官方网址 |
|---|---|---|---|---|---|
| 1 | Lucide | 线性现代，1500+（ISC） | ❌ 只有 SVG/组件 | ✅ 官方 | https://lucide.dev |
| 2 | Tabler | 2px 统一，5900+（MIT） | ✅ `@tabler/icons-webfont` | ✅ 官方 | https://tabler.io/icons |
| 3 | Phosphor | 6 种权重，9000+（MIT） | ✅ `@phosphor-icons/web` | ✅ 官方 | https://phosphoricons.com |
| 4 | Heroicons | 极简，300+（MIT） | ❌ 只有 SVG/组件 | ❌ | https://heroicons.com |
| 5 | Bootstrap Icons | 16px 栅格，2000+（MIT） | ✅ `font/bootstrap-icons.min.css` | ⚠️ 社区 | https://icons.getbootstrap.com |
| 6 | Remix Icon | 中文友好，2800+（Apache-2.0） | ✅ `remixicon/fonts/remixicon.css` | ✅ 官方 | https://remixicon.com |
| 7 | IconPark | 字节，2000+（Apache-2.0） | ❌ 只有 SVG/组件 | ⚠️ 社区 | https://iconpark.oceanengine.com |
| 8 | Octicons | GitHub 风格，200+（MIT） | ❌ 官方已移除 | ❌ | https://primer.style/foundations/icons |
| 9 | Fluent System Icons | 微软，2000+（MIT） | ✅ 有官方字体实现 | ❌ | https://learn.microsoft.com/design/fluent-2/iconography/ |
| 10 | Iconoir | 线性轻量，1600+（MIT） | ❌ 只有 SVG/组件 | ⚠️ 社区 | https://iconoir.com |
| 11 | Radix Icons | 15px 栅格，300+（MIT） | ❌ 只有 SVG/组件 | ⚠️ 社区 | https://www.radix-ui.com/icons |

> ⚠️ **小程序 / uni-app / Taro 项目**：图标通常以 iconfont 引入，**优先选 iconfont ✅ 的 5 个**（Tabler、Phosphor、Bootstrap Icons、Remix Icon、Fluent）。❌ 的库只有 SVG/组件，小程序里要自己转格式。
> ⭐ **Figma 插件**：✅ 官方 4 个（Lucide、Tabler、Phosphor、Remix Icon）；⚠️ 社区 4 个（Bootstrap Icons、IconPark、Iconoir、Radix）；❌ 3 个（Heroicons、Octicons、Fluent）——没有官方插件的可在 Iconify 插件里搜，或手动导入 SVG。
> ⚠️ **列图标库时必须把官方网址展示给用户，或直接打开**——**在官网搜一遍你要用的所有图标**（ticket / seat / stage…），不要只看首页推荐。
> ⭐ **图标预览页（选图标配图，每次选图标必走）**：官方地址 `https://dong-zhiyi.github.io/zcode-design-foundation-icon-preview/`（配置项 `icon_preview_url` 可覆盖）——一页跨库对比 11 个图标库（35 组常用图标已核实命名、SVG 内嵌零网络）+ 中文关键词搜索 + 结果带 `#N` 编号。**流程：用内置浏览器打开预览页 → 搜关键词 → 用户回复「用 N 号」→ 读页面编号映射拿到图标名 → 再落地。不要凭名字猜图标。** 预览页打不开时，才走下面的临时 HTML 流程。
> 🔍 **跨库图标搜索（标准流程）**：选定库名词后，用 Iconify API 把 11 个库里的候选图标**全部列出、看到形状再选**——搜索 → 下载 SVG → 生成 HTML 预览页 → 截图给用户选 → 落地。读 `references/libraries/icon-search.md`。
> 📋 详细对比见 `references/libraries/icons.md`（含「按字体图标支持分组」「Figma 落地」）。
> 🗑️ **已移除**：Hugeicons（60,000+ 为付费 Pro）、Iconify（聚合含非商用集，许可不可控）——本清单只保留**完全免费可商用**的库。

**优先确认**：**能否以 iconfont 引入项目** / 中文支持 / Figma 可用性 / 许可证 / 与项目 UI 库匹配。

### 第 7 步：产出《设计规范简报》

按 `references/requirements/brief-template.md` 的模板，写入项目 `docs/design-brief.md`。

**简报必须包含**：平台规格 + 技术栈规格 / 色板 / 字阶 / 间距 / 圆角 / 阴影 / 组件清单 / 页面清单 / 状态清单。

**这份简报是后续所有设计的唯一真源。**

### 第 8 步：质量自检（不合格先改简报）

读 `references/quality/review-gate.md`，逐项过。重点：

- 对比度达 WCAG 2.2 AA
- 热区达**平台 + 技术栈**标准
- 每个交互组件都有完整状态
- 动效时长与缓动在规范内（查 `references/foundations/motion-values.md` 的精确值表）
- **升级触发器 12 条红线全过**（review-gate.md 中的升级触发器表）
- 没有 AI 味
- 命名符合 `figma-delivery/naming.md`
- 变量绑定覆盖率 100%（所有 fill / stroke / text / 间距 / 圆角都绑变量）
- 页面区块对通用组件目录（`figma-delivery/component-catalog.md`）的命中率达标

**门禁不过就不要开始画。**

### 第 9 步：验证 MCP 连通性 + 确认目标文件（★ 必做）

**这一步不做，后面全是白费。**

#### 9.1 验证 Figma MCP

用 `figwright-mcp` 的 `list_files` 检查连通性：

- **返回文件列表** → MCP 通了，继续
- **返回空数组** → 有插件但没打开文件，提示用户：在 Figma 里打开目标文件并运行 Figwright 插件
- **超时 / 报错** → MCP 没连通，提示用户：
  1. 检查 Figma 桌面端是否打开
  2. 检查 Figwright 插件是否运行
  3. 必要时重启 Figma

> **MCP 从哪来**：本插件自带 `figwright-mcp` 和 `framelink-mcp` 两个 MCP，不需要用户另外配置。
> - `figwright-mcp` 不需要 token，但要求 Figma 桌面端 + Figwright 插件处于运行状态。
> - `framelink-mcp` 需要 Figma token：让用户在 **设置 → 插件管理 → 本插件 → 高级** 里填 `Figma Access Token`。
>
> 服务器名带 `-mcp` 后缀，是为了和全局配置里可能存在的 `figwright` / `Framelink-Figma` 区分开，避免同名冲突。
>
> 如果工具列表里根本没有 `mcp__figwright-mcp__*`，是插件没启用或 MCP 没起来，**不要**让用户去改全局 `config.json`。

**不要用「试试看」的方式跳过这步。**

#### 9.2 确认目标文件

- 如果 `list_files` 只返回 **1 个文件** → 跟用户确认：「要放到 `<文件名>` 吗？」
- 如果返回 **多个文件** → **列出文件让用户选**，不要自己猜
- 确认**页面命名**（新建页面还是用现有页面）

**话术示例**：
```
MCP 已连通，检测到 2 个文件：
1. 剧院售票小程序
2. 风格示例

要把设计放到哪个文件？新建页面还是用现有页面？
```

**用户确认后才能进入下一步。**

### 第 10 步：MCP 落地

用 Figma MCP（`figwright-mcp`）按 `references/figma-delivery/` 的规范建页面。

**⚠️ 图标落地前置条件**：页面里每个图标落地前，用户必须已通过预览页选定（编号或图标名）。没走过的先回第 6 步的预览页流程——打开内置浏览器搜索，让用户选，拿到确定的 `prefix/name` 再 `import_svg`。

**先盘点，再动手**（读 `references/figma-delivery/reuse-first.md`）：

1. `get_local_components` 列出目标文件已有组件
2. 读 `references/figma-delivery/component-catalog.md`，把页面拆成区块（导航/搜索/列表/卡片/表单/空态…），逐个匹配通用组件
3. `component_map` 把要生成的页面映射到现有组件
4. 命中的用 `create_instance` 拉实例 + `set_instance_properties` 覆写
5. **映射不上的才新建**，新建顺手 `create_component` 转组件
6. 新建后输出「组件缺口报告」

然后按页面分区建：

1. `01_Foundation` 页：色板、字阶、间距、组件（变量用 `set_variable_code_syntax` 声明代码名）
2. `02_Components` 页：可复用组件（交互组件用 `combine_as_variants` 组变体集 + `add_component_property` 暴露属性，读 `variants-states.md`）
3. `03_Pages` 页：按页面清单逐页生成（整页用 `batch` 一次原子写入）

命名、图层结构、变量绑定一律按规范来。

**如果中途 MCP 断了**，停下来告诉用户，不要假装成功。

**落地后立刻存基线**：对每个页面 `design_diff`（首次调用返回 `baseline-created`），供第 11 步走查用。

### 第 10.5 步：截图自检闭环（★ 必做，生成完不算完）

读 `references/figma-delivery/verification-loop.md`。**模型看不到自己画出来的样子，必须截图 → 独立裁判打分 → 修 → 再截。**

1. **截**：对每个页面 `get_screenshot`（存 `_work/shots`）
2. **判**：开一个**干净上下文的子 agent**（本环境用 `visual-judge`）按 5 维打分——构图层级 / 排版 / 色彩对比度 / 视觉身份 / 一致性，总分 **≥8/10** 才过。**不要模型自评。**
3. **确定性检查**：`token_map` 看有没有写死值；`search_nodes` 搜 `Frame \d+`/`Group \d+` 等默认名；`get_design_context` 看 auto layout 覆盖率；对比度按 WCAG 算
4. **修**：按裁判清单修，**先修最难的**
5. **再截**：回到第 1 步
6. **退出**：≥8/10 且硬门禁全过 → 收工；连续 2 轮没涨分 → **停**，换结构性方案，别小修小补

**硬门禁（不过不许退出）**：对比度 ≥4.5:1 / 平台尺寸 / 令牌绑定 / 关键状态齐全。

### 第 10.8 步：变体探索（可选）

当用户对初始设计方向不确定，或想看到不同可能性时，沿**一个主轴**做 2-3 个变体。

**变体轴（每次只选一个）**：

| 轴 | 变化内容 | 例子 |
|---|---|---|
| **结构** | 分组、顺序、列数、什么折叠 | 单列 vs 双列 vs 卡片网格 |
| **密度** | 间距尺度、信息密度 | 宽松（24/40/64）vs 紧凑（8/12/16） |
| **强调** | 填充色位置、什么退居次要 | 主色填充按钮 vs 描边按钮 vs 文字按钮 |
| **字体** | 字号阶梯、字重对比 | 大标题小正文 vs 均匀字阶 |
| **语气** | 标签、文案量、正式程度 | 正式「提交申请」vs 轻松「开始吧」 |

**规则**：

1. **每个变体是同一 brief 的不同答案**，不是不同色调
2. **所有变体必须通过升级触发器**（无障碍红线，见 review-gate.md）
3. **嵌入真实页面**，用真实数据，不用 Lorem Ipsum
4. **不标记偏好**，让用户选择
5. 变体文件放在 `_work/variants/` 目录，交付后清理

**产出**：2-3 个变体截图 + 对比表（每行一个变体，列：轴位置、适合场景、代价）

**话术示例**：
```
我做了 3 个方向：
1. **宽松版** — 间距大、留白多，适合内容型产品
2. **紧凑版** — 信息密度高，适合工具型产品
3. **平衡版** — 折中方案

你更倾向哪个？或者想沿某个方向再细化？
```

> 来源：`jakubkrehel/skills` → `variant` SKILL.md（变体探索机制）

### 第 11 步：走查修正

用第 10 步存的 baseline，对每个页面 `design_diff`（返回 per-node、per-property 变化），**精确定位偏差**，回到第 10 步修。

**走查要做，不要只说「已生成」。**

**最新截图要给用户看**，不要只给链接。

---

## 知识库怎么用

| 目录 | 什么时候读 |
|---|---|
| `references/platforms/` | 第 1 步；任何涉及尺寸/导航/热区的问题 |
| `references/tech-stacks/` | 第 2 步；用户问技术栈、单位、框架时 ★ |
| `references/styles/` | 第 4 步；用户问「什么风格」时 |
| `references/architectures/` | 第 5 步；用户问「设计系统怎么组织」时 |
| `references/libraries/` | 第 6 步；用户问字体/图标选型时 |
| `references/libraries/icon-search.md` | 第 6 步选图标 + 第 10 步落地图标时；跨库搜索 + HTML 预览工作流 |
| `references/quality/` | 第 8 步；评审或自查时（含升级触发器红线） |
| `references/foundations/motion-values.md` | 第 8 步 + 第 10 步；动效精确数值 + 16 风格速查 |
| `references/figma-delivery/` | 第 10 步；动手画 Figma 之前 |
| `references/figma-delivery/reference-extraction.md` | 第 4.5 步；选完风格后，从参考提取数值 |
| `references/figma-delivery/reuse-first.md` | 第 10 步；动手前先盘点现有组件 |
| `references/figma-delivery/component-catalog.md` | 第 10 步；动手前先查通用组件目录 |
| `references/figma-delivery/variants-states.md` | 第 10 步；建交互组件时 |
| `references/figma-delivery/verification-loop.md` | 第 10.5 步；生成后截图自检 |
| `references/foundations/motion-values.md` | 第 8 步自检 + 第 10 步落地；动效精确数值参考 |
| `references/examples/` | 第一次走全流程，或卡在某个环节时 |
| `references/requirements/` | 第 3、7 步 |
| `references/01-sources.md` | 需要追溯数据来源时 |
| `references/02-how-to-add.md` | 用户要新增内容时 |

---

## 动态新增（重要）

知识库不是封闭的。用户说「加一个 XX 风格 / 平台 / 技术栈」时，按 `references/02-how-to-add.md` 走：

1. 搜索 GitHub（优先 GitHub MCP，不可用时用 exa 抓取 GitHub 页面）
2. 筛选（star > 500、近一年有提交、许可清晰）
3. 按 `_template.md` 写成卡片
4. 落盘 + 更新索引 + 更新 `01-sources.md`
5. 给用户看来源，确认后入库

**不要凭印象编造数值。**

---

## 几条原则

- **平台 + 技术栈优先于风格。** 顺序错了，后面全白做。
- **数值优先于形容词。** 说「圆角 12px」，不说「圆润感」。
- **状态优先于静态。** 没有 hover/disabled/loading/empty/error 的设计是不完整的。
- **门禁优先于交付。** 不过门禁的稿子不往外发。
- **先验证再动手。** MCP 没通、文件不对，就不要开始画。
- **可追溯优先于丰富。** 宁可少收录，不要收录没来源的内容。

---

## 评测

改完这个 skill 后，跑这几个 prompt 回归（一次一个，看结果）：

| # | Prompt | 预期 |
|---|---|---|
| 1 | 「我要做一个小程序」 | **只列 7 个小程序平台**（不列 App/Web）→ 技术栈只列 原生/uni-app/Taro |
| 1b | 「做一个剧院售票小程序首页」 | 平台已明确 → 技术栈只列小程序相关 → 按 375 基准 + 128rpx 导航 + 胶囊避让 |
| 2 | 「用 uni-app x 做一个演出 App」 | 基准 **390px（1x）**、单位 **px**、导航 44px、TabBar 50px |
| 3 | 「用经典 uni-app 做一个演出 App」 | 基准 **750px**、单位 **rpx** |
| 4 | 「做一个知识付费 H5」 | 基准 **375px**（不是 1440）、自己画导航 |
| 5 | 「做一个 SaaS 后台」 | 基准 1440px、12 栏栅格、hover 态 |
| 6 | 「帮我选个设计风格」 | **列出全部 16 个**，一行一个（可附一句推荐），让用户自己选 |
| 7 | 「这个设计稿有没有 AI 味？」 | 按 anti-ai-slop 清单逐项过（含 2026 指纹） |
| 8 | 「做一个剧院售票小程序首页」（走完第 10 步） | 生成后**必须截图 + 独立裁判打分**，≥8/10 才收工；不是「已生成」 |
| 9 | 「参考 https://xxx.com 做个落地页」 | 第 4.5 步先提取色板/字阶数值写进简报，再生成 |
| 10 | 「生成一个含通用组件的页面」 | 必须命中组件目录；变体组合 ≤30；token 绑定率 100%；输出组件缺口报告 |

- **第一次走全流程** → `references/examples/walkthrough.md`（通用，任何产品都能套用）
- **想看具体案例** → `references/examples/case-theater-ticketing.md`（剧院售票，含完整决策过程）
