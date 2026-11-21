# 图标库（总索引）

**选型三问**：风格与产品调性一致吗？Figma 里有没有官方插件？许可允许商用吗？

> ⚠️ **必须列全让用户选**，不要只给 3–5 个推荐。11 个库都要列出来，可附一句推荐。

---

## 入库标准

**新增图标库必须同时满足**：

| 条件 | 说明 |
|---|---|
| **开源 + 可商用** | 许可必须是 MIT / ISC / Apache-2.0 / OFL 等宽松许可；**非商用（NC）一律不收录** |
| **支持字体图标** | 官方提供 iconfont / webfont 包，或能用工具（IcoMoon / Fontello / Iconify）从 SVG 转换 |
| **来源可查** | 有 GitHub 仓库或官网，能核实版本和许可 |

**新增流程**：按 `../02-how-to-add.md`，额外核实「许可」和「字体图标支持」两项。

---

## 速查表（11 个，已全部核实；均为免费可商用）

| 库 | 数量 | 风格 | 许可 | 商用 | 字体图标 | Figma 插件 | 文档 |
|---|---|---|---|---|---|---|---|
| **Lucide** | 1600+ | 线性、圆润、现代 | ISC | ✅ | ❌ 仅 SVG/组件 | ✅ 官方 | [→](./icons/lucide.md) |
| **Tabler Icons** | 6200+ | 线性、2px 统一、专业 | MIT | ✅ | ✅ `@tabler/icons-webfont` | ✅ 官方 | [→](./icons/tabler.md) |
| **Phosphor** | 9000+ | 统一家族、6 权重 | MIT | ✅ | ✅ `@phosphor-icons/web` | ✅ 官方 | [→](./icons/phosphor.md) |
| **Heroicons** | 300+/风格 | 极简、手工 | MIT | ✅ | ❌ 仅 SVG/组件 | ❌ | [→](./icons/heroicons.md) |
| **Bootstrap Icons** | 2000+ | 轻量、16px 栅格 | MIT | ✅ | ✅ `font/bootstrap-icons.min.css` | ⚠️ 社区 | [→](./icons/bootstrap-icons.md) |
| **Remix Icon** | 3200+ | 线性、中文友好 | Apache-2.0 | ✅ | ✅ `remixicon/fonts/remixicon.css` | ✅ 官方 | [→](./icons/remix-icon.md) |
| **IconPark** | 2000+ | 字节、4 主题 | Apache-2.0 | ✅ | ❌ 仅 SVG/组件 | ⚠️ 社区 | [→](./icons/iconpark.md) |
| **Octicons** | 200+ | GitHub 风格 | MIT | ✅ | ❌ 官方已移除 | ❌ | [→](./icons/octicons.md) |
| **Fluent System Icons** | 2000+ | 微软 Fluent | MIT | ✅ | ✅ 字体实现 | ❌ | [→](./icons/fluent-system-icons.md) |
| **Iconoir** | 1600+ | 线性、轻量 | MIT | ✅ | ❌ 仅 SVG/组件 | ⚠️ 社区 | [→](./icons/iconoir.md) |
| **Radix Icons** | 300+ | 线性、15px 栅格 | MIT | ✅ | ❌ 仅 SVG | ⚠️ 社区 | [→](./icons/radix-icons.md) |

> 💡 **Figma 插件列**：✅ 官方 = 有官方插件；⚠️ 社区 = 只有第三方/社区插件（Iconduck 等）；❌ = 无专门插件。
> ⭐ **11 个库都能在 Figma 里用**：4 个有官方插件，4 个有社区插件，其余 3 个（Heroicons / Octicons / Fluent）可手动导入 SVG，或用 Iconify 的 Figma 插件搜（**仅作浏览工具，用前核对许可**）。详见下方「Figma 落地」。
> 🗑️ **已移除**：Hugeicons（60,000+ 为付费 Pro）、Iconify（聚合含非商用集）——本清单只保留**完全免费可商用**的库。

**按字体图标支持分组**：

- ✅ **有官方字体包**：Tabler · Phosphor · Bootstrap Icons · Remix Icon · Fluent
- ❌ **无官方字体包**：Lucide · Heroicons · Iconoir · Radix · IconPark · Octicons

**官方链接**（选型时打开看实际图标，**在官网搜一遍你要用的所有图标**，不要只看首页推荐）：

| 库 | 官方网址 |
|---|---|
| Lucide | https://lucide.dev |
| Tabler | https://tabler.io/icons |
| Phosphor | https://phosphoricons.com |
| Heroicons | https://heroicons.com |
| Bootstrap Icons | https://icons.getbootstrap.com |
| Remix Icon | https://remixicon.com |
| IconPark | https://iconpark.oceanengine.com |
| Octicons | https://primer.style/foundations/icons |
| Fluent System Icons | https://learn.microsoft.com/design/fluent-2/iconography/ |
| Iconoir | https://iconoir.com |
| Radix Icons | https://www.radix-ui.com/icons |

---

## 版权与免费（重要）

**本清单 11 个库全部免费可商用**：

| 许可 | 库 |
|---|---|
| **MIT** | Tabler、Phosphor、Heroicons、Bootstrap Icons、Octicons、Fluent System Icons、Iconoir、Radix Icons |
| **ISC** | Lucide |
| **Apache-2.0** | Remix Icon、IconPark |

> ⚠️ **已移除的 2 个**：
> - **Hugeicons** —— freemium，**60,000+ 是付费 Pro**（免费版只有 6,000+ 且单一风格）
> - **Iconify** —— 聚合 200+ 套，**含 CC-BY-NC（非商用）集**，许可不可控
>
> ⚠️ **iconfont.cn 是 UGC 平台**，图标版权按站内协议，**不是全部免费可商用**，别和开源库混用。
>
> 💡 **新增库门槛**：必须**完全免费可商用**（MIT / ISC / Apache-2.0 / OFL）；freemium、聚合含 NC 集、需署名的库**不收录**。

---

## 按场景推荐

| 场景 | 推荐 | 理由 |
|---|---|---|
| **通用产品 / 默认选择** | **Lucide** | 风格中性、社区最大、shadcn/ui 默认（但无官方字体包） |
| **小程序项目（要 iconfont）** | **Tabler** 或 **Phosphor** | 都有官方 webfont + Figma 插件，MIT 可商用 |
| **后台 / SaaS / 数据面板** | **Tabler** | 数量大、2px 统一、专业感强 |
| **需要多权重切换** | **Phosphor** | 6 种权重，一套图标覆盖多种语境 |
| **极简 / 小而精** | **Heroicons** | 质量高、小尺寸清晰（有专门 16px 版） |
| **中文生态 / 国内 CDN** | **Remix Icon** | 中文文档友好、有字体包、Apache-2.0 |

---

## 风格统一原则（重要）

⚠️ **不要混用多个图标库**。即使单个图标好看，混用会立刻显得不专业。

**必须统一**：
- **栅格**：24×24（主流）或 20×20
- **线宽**：1.5px / 1.8px / 2px（三选一，全站统一）
- **端点**：圆头（round）或平头（butt）
- **转角**：圆角或直角
- **视觉重量**：线性 vs 面性（不要混）

**检查方法**：把 10 个图标排一排，如果能看出「不是同一套」，就是没统一。

---

## 字体图标为什么重要

| 场景 | 说明 |
|---|---|
| **微信小程序** | 常用 iconfont（SVG 支持有限，iconfont 体积小、改色方便） |
| **uni-app / Taro** | 同小程序，iconfont 兼容好 |
| **Web** | SVG 和 iconfont 都行 |
| **App** | SVG 更常见（可缩放、可着色） |

**判断**：目标端是小程序 → **优先选有官方字体包的库**，省掉转换步骤。

---

## 尺寸规范

| 场景 | 尺寸 | 线宽 |
|---|---|---|
| 导航栏图标 | 24px | 1.8–2px |
| 列表项图标 | 20px | 1.8px |
| 内联图标（跟文字） | 16px | 1.5px |
| 密集表格 | 16px | 1.5px |
| 功能入口（金刚区） | 24–32px | 1.8–2px |
| TabBar 图标 | 24–28px | 1.8–2px |

---

## Figma 落地

### 首选：用所选库的官方插件

| 库 | 官方插件 |
|---|---|
| **Lucide / Tabler / Phosphor** | 各自官方插件（Figma Community 搜库名） |
| **Remix Icon** | RemixIcon Official Plugin |

### 没有官方插件的库

- **Bootstrap Icons / IconPark**：Figma Community 有社区插件
- **Iconoir / Radix Icons**：Iconduck 出的插件
- **Heroicons / Octicons / Fluent System Icons**：无专门插件 → **手动拖 SVG**，或用 **Iconify 的 Figma 插件**搜

### 关于 Iconify 的 Figma 插件（工具，不是库）

Iconify 的 Figma 插件能浏览/搜索几十个开源库（含本清单全部 11 个），全部以 **SVG** 导入，**可改颜色、可导入为组件**。

- 安装：Figma Community 搜 **"Iconify"**，或从 https://iconify.design/docs/design/figma/ 进插件页
- 用法：Plugins → Iconify → 搜索/浏览 → "Import Icon" 或拖到画布

> ⚠️ **Iconify 只是浏览工具**，本清单已不把它列为可选图标库（它聚合的集合里含非商用许可）。用它搜到的图标，**版权归各自的原库**（如 Phosphor = MIT）。

### 手动导入（兜底）

- 官网下载 SVG → 拖进 Figma → 可编辑
- 导入 SVG 后建组件 → 用**变体（Variants）**管理状态（默认/hover/激活）
- **命名**：`Icon/<名称>`，如 `Icon/search`、`Icon/arrow-right`
- **颜色**：用 `currentColor`（继承文字色），不要写死颜色

### ⚠️ 中文图标库（iconfont.cn）的版权提醒

阿里 **iconfont.cn 没有官方 Figma 插件**（第三方 IconBridge 可连项目，但需自配 CORS 代理）。更重要的是：**它是 UGC 平台，图标版权按站内协议，并非全部免费可商用**。不要把来源不明的 iconfont 图标和已选定的开源库（如 Phosphor MIT）混用。

---

## 风格 × 图标库匹配矩阵

**选风格后直接查表。** 每种风格推荐 1-2 个图标库 + 粗细/尺寸规则。

| 风格 | 推荐图标库 | 粗细 | 尺寸 | 说明 |
|---|---|---|---|---|
| 经典剧院 | Phosphor (Regular) | 1.8px | 24px | 优雅线性，配合宋体标题 |
| 现代深色 | Lucide / Phosphor (Thin) | 1.5px | 20px | 极简线性，克制精确 |
| 简约浅色 | Lucide / Tabler | 1.5–2px | 20–24px | 几何感强，理性清晰 |
| 玻璃拟态 | Lucide (Thin) | 1.5px | 24px | 细线配合通透质感 |
| 新粗野主义 | Phosphor (Bold) / Tabler | 2–3px | 24–32px | 粗描边，配合硬阴影 |
| 黏土拟态 | Phosphor (Fill) | 面性 filled | 24–32px | 圆润面性，配合 3D 感 |
| 柔 UI | Phosphor (Regular) | 1.5px | 24px | 细线配合柔和凹凸 |
| Bento Grid | Phosphor (Fill) / Lucide | 面性或 2px | 24–40px | 可大至 40px 作视觉焦点 |
| 编辑杂志 | Lucide (Thin) | 1.5px | 20–24px | 细线配合衬线字体 |
| 终端 TUI | 无图标（用字符） | — | 16px 等宽 | ASCII 字符界面 |
| 赛博朋克 | Phosphor (Bold) / 像素图标 | 2px 或像素 | 24px | 可加霓虹发光效果 |
| 日系极简 | Lucide (Thin) / 自绘 | 1–1.5px | 20–24px | 极细线性，克制 |
| 斯堪的纳维亚 | Phosphor (Regular) | 1.8px | 24px | 圆润线性，温暖 |
| 包豪斯 | 自绘几何（○□△） | 2–4px | 24–32px | 纯几何构成 |
| 国风宋墨 | 自绘水墨 / Phosphor (Thin) | 1.5px 或笔触 | 24px | 线性或水墨风 |
| AI Native UI | Lucide / Phosphor (Regular) | 1.5–2px | 20–24px | 简洁线性，友好 |

**尺寸规则**：

| 场景 | 尺寸 | 线宽 |
|---|---|---|
| 导航栏图标 | 24px | 1.8–2px |
| 列表项图标 | 20px | 1.8px |
| 内联图标（跟文字） | 16px | 1.5px |
| 密集表格 | 16px | 1.5px |
| 功能入口（金刚区） | 24–32px | 1.8–2px |
| TabBar 图标 | 24–28px | 1.8–2px |
| 空状态插画 | 120–160px | 按风格 |
| Dashboard 指标图标 | 32–48px | 2px |

---

## 检查清单

- [ ] 全站只用**一个**图标库
- [ ] 许可**允许商用**（非 NC / 非专有）
- [ ] 有官方字体包，或已确认可转换
- [ ] 栅格、线宽、端点、转角统一
- [ ] 尺寸档位明确（24/20/16）
- [ ] Figma 里有对应组件（不是每次贴 SVG）
- [ ] 颜色用 currentColor
- [ ] 命名规范（`Icon/<名称>`）
- [ ] 图标风格与整体风格匹配（查上方矩阵）
- [ ] 选图前走过跨库搜索 + HTML 预览流程（见 `icon-search.md`）
