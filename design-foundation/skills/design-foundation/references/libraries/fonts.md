# 字体库

**选型三问**：有没有中文（CJK）？Figma 里能不能直接用？商用许可有没有限制？

> ⚠️ **必须列全让用户选**，不要只给 2–3 个推荐。下面速查表里的 13 个字体都要列出来，可附一句推荐（如「中文产品建议看 2、3、4、5」）。

## 入库标准

| 条件 | 说明 |
|---|---|
| **开源 + 可商用** | 许可必须是 SIL OFL / Apache-2.0 / MIT 等宽松许可；**非商用（NC）一律不收录** |
| **来源可查** | 有 GitHub 仓库或官网，能核实版本和许可 |
| **系统字体要标注** | PingFang SC / 微软雅黑这类**不可随产品分发**，必须标注「仅限系统引用」 |

**核实方法**：打开仓库的 `LICENSE` 或官网 License 页面，**不要凭印象**。

**新增流程**：按 `../02-how-to-add.md`。

---

## 速查表

| 字体 | 中文 | 可变字体 | 许可 | 风格 | 来源 |
|---|---|---|---|---|---|
| **⊘ 默认字体（不指定）** | ✅ | — | 无风险 | 跟随平台 | 系统字体栈，见下方「选项 0」 |
| **Inter** | ❌ 需配中文字体 | ✅ | SIL OFL 1.1 | 中性、UI 标杆 | `rsms/inter` 20k★ |
| **IBM Plex Sans** | ✅ 简繁日韩 | ✅ | SIL OFL | 工程感、IBM 风 | `IBM/plex` |
| **Noto Sans / Noto Sans SC** | ✅ 全 CJK | ✅ | SIL OFL | 中性、谷歌风 | `notofonts/noto-cjk` 4.1k★ |
| **Source Han Sans（思源黑体）** | ✅ 全 CJK | ✅ | SIL OFL | 中性、Adobe/谷歌 | `adobe-fonts/source-han-sans` |
| **Source Han Serif（思源宋体）** | ✅ 全 CJK | ✅ | SIL OFL | 标题、编辑感 | `adobe-fonts/source-han-serif` |
| **Geist** | ❌ | ✅ | SIL OFL | 现代、Vercel 风 | `vercel/geist-font` |
| **Public Sans** | ❌ | ✅ | SIL OFL（含 USWDS） | 政务、中性 | `uswds/public-sans` |
| **Space Grotesk** | ❌ | ✅ | SIL OFL | 几何、科技 | `floriankarsten/space-grotesk` |
| **DM Sans** | ❌ | ✅ | SIL OFL | 圆润、友好 | `googlefonts/dm-fonts` |
| **Manrope** | ❌ | ✅ | SIL OFL | 现代、圆润 | `sharanda/manrope` |
| **JetBrains Mono** | ❌ | ✅ | SIL OFL | 等宽、代码 | `JetBrains/JetBrainsMono` |
| **阿里巴巴普惠体** | ✅ | ✅ | 官方免费商用 | 现代、字重丰富 | `alibabapuhuiti` |

> ✅ **本表所有字体均可商用、可随产品分发**（12 个 SIL OFL 1.1 + 1 个官方免费商用）。
> ⚠️ **系统字体（PingFang SC / 微软雅黑）不列入本表**——它们不能随产品分发，只能通过**选项 0（默认字体）**作为系统字体栈引用。

---

## 在线预览（选型时打开看效果）

**⭐ 首选：字体样张页**——插件配置项 `font_preview_url` 指向的地址（独立静态项目 `zcode-design-foundation-font-preview`，部署后一页对比 13 个字体 + 可输入真实文案）。**列字体、让用户看效果时打开这个地址**。

> ⚠️ **不要用它各字体的官网地址**：国内多数打不开（Google Fonts / fontsource / Adobe 等），且看不到中文渲染效果。

**补充：各字体官网**（仅在需要核对授权原文、字样细节时用）：

| 字体 | 官网 |
|---|---|
| Inter | https://rsms.me/inter/ · https://fonts.google.com/specimen/Inter |
| IBM Plex Sans | https://www.ibm.com/plex/ |
| Noto Sans SC | https://fonts.google.com/noto/specimen/Noto+Sans+SC |
| Source Han Sans（思源黑体） | https://source-han-sans.adobe.com/ |
| Source Han Serif（思源宋体） | https://source-han-serif.adobe.com/ |
| Geist | https://vercel.com/font |
| Public Sans | https://fonts.google.com/specimen/Public+Sans |
| Space Grotesk | https://fonts.google.com/specimen/Space+Grotesk |
| DM Sans | https://fonts.google.com/specimen/DM+Sans |
| Manrope | https://fonts.google.com/specimen/Manrope |
| JetBrains Mono | https://www.jetbrains.com/lp/mono/ |
| 阿里巴巴普惠体 | https://www.alibabafonts.com/ |
| 各字体汇总预览 | ~~https://fontsource.org/~~ （2100+ 字族，**国内可能打不开**，见下方替代） |

> 💡 **选型建议**：把候选字体打开，**输入你的真实文案**（你自己的产品名、标题、价格）看效果，比看字样图准确得多。

---

## 国内可访问的预览站（推荐）

> fontsource.org 和 Google Fonts 在中国大陆**可能打不开或加载极慢**，用下面这些。

| 站点 | 网址 | 特点 |
|---|---|---|
| **Anyway.Fonts** | https://anyway.fm/fonts/ | ⭐ **中文字体预览 + 对比 + 缺字检测**，最适合选型 |
| **Google Fonts 中文镜像** | https://googlefonts.cn/ | Google Fonts 的中文镜像，可编辑预览文字 |
| **字由** | https://www.hellofont.cn/ | 5000+ 字体，客户端一键切换 PS/AI/Figma |
| **字体天下** | https://www.fonts.net.cn/ | 中文字体 9862 款，可在线预览，有「商用免费」筛选 |
| **站长字体** | https://www.100font.com/ | 1500+ 免费商用，**每款标注授权协议** |
| **字加** | https://www.zijia.com/ | 综合字体库 |
| **优设字体** | https://www.uisdc.com/fonts | 免费商用字体合集，带案例 |
| **FontBase**（客户端） | https://fontba.se/ | 跨平台字体管理 + 预览，免费版够用 |

### Google Fonts 国内镜像（网页引入用）

| 镜像 | 地址 | 说明 |
|---|---|---|
| **⭐ Google 官方中国镜像** | `https://fonts.googleapis.cn` | **首选**。谷歌广告（上海）备案域名，国内可直接访问；返回的字体文件自动指向 `fonts.gstatic.cn` |
| loli.net | `https://fonts.loli.net` | 第三方镜像（日本节点，有时不稳定） |
| font.im | `https://fonts.font.im` | 第三方备选 |
| googlefonts.cn | `https://googlefonts.cn` | 中文镜像，可在线预览 |

**用法**：把 `fonts.googleapis.com` 直接替换成 `fonts.googleapis.cn` 即可，路径不变：
```
https://fonts.googleapis.cn/css2?family=Inter:wght@400;700&display=swap
```

> ✅ **推荐用官方中国镜像**，不要再用第三方镜像或自己托管（除非需要完全离线）。

---

## 许可证与商用（重要）

### 结论：本表 12 个字体全部可商用 ✅

**全部是 SIL OFL 1.1 许可**，允许：
- ✅ 商业使用（网站、App、海报、产品）
- ✅ **嵌入到软件/产品里分发**
- ✅ 修改字体
- ✅ 重新分发

> **Google Fonts 官方 FAQ 原话**：「Can I use any font in a commercial product? **Yes**, you can use them commercially, and even include them within a product that is sold commercially.」

### 系统字体（PingFang SC / 微软雅黑）为什么不列进来

| 字体 | 许可 | 能做什么 | 不能做什么 |
|---|---|---|---|
| **PingFang SC**（苹方） | ❌ Apple 专有 | 在 iOS/macOS 上**作为系统字体引用** | ❌ 打包进 App / 转 Web Font / 随产品分发 |
| **微软雅黑** | ❌ Microsoft 专有 | 在 Windows 上**作为系统字体引用** | ❌ 打包进产品 / 转 Web Font |

**处理方式**：它们**不列为字体选项**，只通过 **选项 0（默认字体）** 作为系统字体栈引用。

**判断标准**：
- ✅ `font-family: "PingFang SC", sans-serif;`（只是引用，不打包）
- ❌ 把 `PingFangSC.ttf` 放进 App 安装包或网站字体目录

> 💡 **这就是为什么「选项 0：默认字体」是最安全的选择**——不引入任何字体文件，零授权风险。

### OFL（SIL Open Font License）能做什么、不能做什么

**✅ 允许**：
- 商业使用（网站、App、海报、产品）
- **嵌入到软件/产品里分发**（这是关键，很多字体不允许）
- 修改字体
- 重新分发

**❌ 不允许**：
- **单独出售字体文件本身**（不能把 .ttf 当商品卖）
- 修改后**沿用原字体名**（Reserved Font Name，如 IBM Plex、Noto Sans SC 有 RFN）

**实操建议**：
- 直接用，不改名 → 完全没问题
- 要改字体 → 改完**必须改名**
- 随 App/Web 分发 → OFL 字体都可以

### 中文字体的特别提醒

- **思源黑体 / 思源宋体**：OFL，**可商用、可分发**，是国内项目的首选
- **阿里巴巴普惠体**：免费商用（非 OFL，但有官方授权声明），字重丰富
- **思源系列体积大**（单字重 10–20MB）→ Web 端务必**子集化**或只加载需要的字重
- **小程序**：自定义字体受限，优先用系统字体

---

## 选项 0：默认字体（不指定）

**什么时候选它**：
- 微信小程序（**只能用系统字体**，自定义要 base64 内嵌，体积大）
- 不想引入额外字体文件
- 没有强品牌字体要求

**做法**：不引入任何自定义字体，用**系统字体栈**：

```css
font-family: -apple-system, BlinkMacSystemFont, "PingFang SC",
             "Helvetica Neue", "Microsoft YaHei", "Segoe UI", Roboto, sans-serif;
```

**各平台实际渲染**：
| 平台 | 实际字体 |
|---|---|
| iOS | PingFang SC（苹方） |
| Android | Roboto / 系统默认 |
| 微信小程序 | 系统默认（iOS 苹方 / Android Roboto） |
| Windows | 微软雅黑 |
| macOS | PingFang SC / Helvetica Neue |

**优点**：零成本、零体积、无加载延迟
**缺点**：各平台观感不统一，无法体现品牌字体个性

---

## 按场景推荐

### 中文优先（国内产品）

| 场景 | 推荐 |
|---|---|
| 通用 App / 小程序 | **Inter + 思源黑体**（英文 Inter，中文思源黑） |
| 内容阅读 | **Source Han Serif（思源宋体）** 标题 + 思源黑正文 |
| 政务 / 商务 | **Source Han Sans** 全站 |
| 品牌感强 | **IBM Plex Sans**（含 CJK，一字开双端） |

### 纯英文 / 出海产品

| 场景 | 推荐 |
|---|---|
| 通用 UI | **Inter** |
| 现代科技 | **Geist** / **Space Grotesk** |
| 友好亲和 | **DM Sans** / **Manrope** |
| 代码/数据 | **JetBrains Mono** |
| 政务 | **Public Sans** |

---

## 字阶建议

| 用途 | 英文（px） | 中文（px） | 字重 |
|---|---|---|---|
| 大标题 | 32–48 | 28–40 | Bold / Semi Bold |
| 标题 | 20–28 | 18–24 | Semi Bold |
| 正文 | 14–16 | 14–16 | Regular |
| 辅助 | 12–13 | 12–13 | Regular |
| 标注 | 11–12 | 11–12 | Medium |

**中文注意**：
- 中文**最小 12px**（低于 12 会糊）
- 中文**行高 1.7–1.9**（英文 1.5–1.6）
- 中文**字重对比要小**（Regular/Medium 够了，Bold 容易糊）

---

## 字体配对（标题 + 正文）

| 风格 | 标题 | 正文 |
|---|---|---|
| 经典剧院 | 思源宋体 Bold | 思源黑体 Regular |
| 现代深色 | Geist Semi Bold | Inter Regular |
| 简约瑞士 | Inter Bold | Inter Regular |
| 编辑杂志 | Playfair Display | Source Serif |
| 友好亲和 | DM Sans Bold | DM Sans Regular |
| 科技未来 | Space Grotesk | Inter |
| 代码/数据 | JetBrains Mono | JetBrains Mono |

**要点**：**最多两种字体**。第三种开始就乱了。

---

## Figma 落地

- 在 Figma 里建 **Text Style**，命名与令牌对应
- 例：`Text/Heading/LG`、`Text/Body/MD`、`Text/Caption`
- 中英文字体要**分别指定**（Figma 的 fallback 机制不可靠，建议用两个文本样式）

---

## 更多字体资源

| 仓库 | 内容 |
|---|---|
| [`google/fonts`](https://github.com/google/fonts) | Google Fonts 官方仓库（1600+ 字体） |
| [`fontsource/fontsource`](https://github.com/fontsource/fontsource) | npm 自托管字体（2100+ 字族） |
| [`drwpow/awesome-oss-fonts`](https://github.com/drwpow/awesome-oss-fonts) | 开源字体清单 |
| [`menozero/usable-fonts`](https://github.com/menozero/usable-fonts) | 可用的开源字体 |
| [`brabadu/awesome-fonts`](https://github.com/brabadu/awesome-fonts) | 字体资源大全 |

---

## 检查清单

- [ ] 中文支持已确认（CJK 字形完整、字重够）
- [ ] 许可允许商用
- [ ] 字体文件大小可接受（中文字体注意子集化）
- [ ] Figma 里可直接使用
- [ ] 中英文字阶分别定义
- [ ] 行高中文 ≥1.7
