# 数据来源清单

**每条知识都要能追溯。** 这里记录所有引用的仓库和规范来源。

> 数据采集时间：**2026-11-04**
> 复核方法：用 GitHub API / 仓库页面查 star 数和最近提交

---

## 一、风格 skill（外部知识库）

| 仓库 | 风格数 | 许可 | 备注 |
|---|---|---|---|
| [`yanliudesign/design-style-skill`](https://github.com/yanliudesign/design-style-skill) | 67 | — | 含色板/CSS/字体/提示词 + DNA 评分 |
| [`cumabozkurt/god-of-design`](https://github.com/cumabozkurt/god-of-design) | 109 | — | 19 个 skill，含反 AI 味门禁 |
| [`chrzamz/ui-styles-skill`](https://github.com/chrzamz/ui-styles-skill) | 79 | — | **中英双语**，含自定义风格创建器 |
| [`luogao/design-atlas`](https://github.com/luogao/design-atlas) | 59 | — | 每个风格带 `tokens.css` |
| [`nicohodt/claude-code-ui-ux-skill`](https://github.com/nicohodt/claude-code-ui-ux-skill) | 84 | — | 离线 CSV，含 192 配色 |
| [`joeseesun/qiaomu-design`](https://github.com/joeseesun/qiaomu-design) | 58 | — | 反 AI 味设计顾问 |
| [`MKAbuMattar/uidesign`](https://github.com/MKAbuMattar/uidesign) | 9 | MIT | 强无障碍导向 |
| [`ddddddps/claude-ui-style-skill`](https://github.com/dddddps/claude-ui-style-skill) | 70+ | — | 真实产品（Airbnb/Apple/Linear/Stripe） |
| [`bitjaru/styleseed`](https://github.com/bitjaru/styleseed) | — | MIT | 设计方法引擎，22 个工作流 |
| [`meefs/style-skills`](https://github.com/meefs/style-skills) | — | — | 风格分解提示词 |
| [`feimacode/open-design-agent-kit`](https://github.com/feimacode/open-design-agent-kit) | 270+ | — | 150+ 品牌设计系统 |

---

## 二、设计系统与架构参考

| 仓库 | Star | 许可 | 用途 |
|---|---|---|---|
| [`carbon-design-system/carbon`](https://github.com/carbon-design-system/carbon) | 9.5k | Apache-2.0 | 架构教科书，含 Figma Kit |
| [`carbon-design-system/carbon-design-kit`](https://github.com/carbon-design-system/carbon-design-kit) | — | — | Figma 设计套件 |
| [`primer/css`](https://github.com/primer/css) | 12.9k | MIT | GitHub 设计系统 |
| [`primer/react`](https://github.com/primer/react) | 3.8k | MIT | Primer 组件 |
| [`primer/primitives`](https://github.com/primer/primitives) | 415 | MIT | **令牌分层实践** |
| [`primer/octicons`](https://github.com/primer/octicons) | 8.7k | MIT | GitHub 图标 |
| [`arco-design/arco-design`](https://github.com/arco-design/arco-design) | 5.7k | MIT | 字节，含 Figma 库 |
| [`microsoft/fluentui`](https://github.com/microsoft/fluentui) | — | MIT | 259 个 workspace 包 |
| [`chakra-ui/chakra-ui`](https://github.com/chakra-ui/chakra-ui) | — | MIT | TypeScript 令牌 |
| [`mantinedev/mantine`](https://github.com/mantinedev/mantine) | — | MIT | TypeScript 令牌 |
| [`adobe/react-spectrum`](https://github.com/adobe/react-spectrum) | — | Apache-2.0 | 令牌独立仓库 |
| [`alexpate/awesome-design-systems`](https://github.com/alexpate/awesome-design-systems) | — | — | 设计系统索引（100+） |

---

## 三、令牌与架构规范

| 仓库 | Star | 许可 | 用途 |
|---|---|---|---|
| [`design-tokens/community-group`](https://github.com/design-tokens/community-group) | 2.2k | — | **DTCG 格式标准**（W3C 社区组） |
| [`tokens-studio/figma-plugin`](https://github.com/tokens-studio/figma-plugin) | 1.6k | MIT | Figma 令牌管理 |
| [`style-dictionary/style-dictionary`](https://github.com/style-dictionary/style-dictionary) | 4.7k | Apache-2.0 | 令牌 → 多端代码 |

> ⚠️ **注意**：Style Dictionary 已从 `amzn/style-dictionary` 迁移到 `style-dictionary/style-dictionary`，旧地址会 404。

---

## 四、字体

| 仓库 | Star | 许可 | 备注 |
|---|---|---|---|
| [`rsms/inter`](https://github.com/rsms/inter) | 20k | SIL OFL 1.1 | UI 字体标杆，可变字体 |
| [`IBM/plex`](https://github.com/IBM/plex) | — | SIL OFL | 含中日韩 |
| [`notofonts/noto-cjk`](https://github.com/notofonts/noto-cjk) | 4.1k | SIL OFL | 中日韩 |
| [`adobe-fonts/source-han-sans`](https://github.com/adobe-fonts/source-han-sans) | — | SIL OFL | 思源黑体 |
| [`google/fonts`](https://github.com/google/fonts) | — | 混合 | 1600+ 字体 |
| [`fontsource/fontsource`](https://github.com/fontsource/fontsource) | — | 混合 | npm 自托管 |
| [`drwpow/awesome-oss-fonts`](https://github.com/drwpow/awesome-oss-fonts) | — | — | 开源字体清单 |
| [`menozero/usable-fonts`](https://github.com/menozero/usable-fonts) | — | — | 可用开源字体 |
| [`brabadu/awesome-fonts`](https://github.com/brabadu/awesome-fonts) | — | — | 字体资源大全 |

---

## 五、图标库

| 仓库 | 数量 | 许可 | Figma 插件 |
|---|---|---|---|
| [`lucide-icons/lucide`](https://github.com/lucide-icons/lucide) | 1500+ | ISC | ✅ |
| [`tabler/tabler-icons`](https://github.com/tabler/tabler-icons) | 5900+ | MIT | ✅ |
| [`phosphor-icons/core`](https://github.com/phosphor-icons/core) | 9000+ | MIT | ✅ |
| [`tailwindlabs/heroicons`](https://github.com/tailwindlabs/heroicons) | 300+/风格 | MIT | ❌ |
| [`twbs/icons`](https://github.com/twbs/icons) | 2000+ | MIT | ❌ |
| [`iconify/icon-sets`](https://github.com/iconify/icon-sets) | 20 万+ | 各库独立 | ✅ |
| [`Remix-Design/RemixIcon`](https://github.com/Remix-Design/RemixIcon) | 2800+ | Apache-2.0 | ❌ |
| [`bytedance/IconPark`](https://github.com/bytedance/IconPark) | 2000+ | Apache-2.0 | ❌ |
| [`primer/octicons`](https://github.com/primer/octicons) | 200+ | MIT | ❌ |
| [`microsoft/fluentui-system-icons`](https://github.com/microsoft/fluentui-system-icons) | 2000+ | MIT | ❌ |
| [`radix-ui/icons`](https://github.com/radix-ui/icons) | 300+ | MIT | ❌ |
| [`hugeicons/hugeicons`](https://github.com/hugeicons/hugeicons) | 46000+ | MIT | ❌ |

---

## 六、平台官方规范

| 平台 | 来源 |
|---|---|
| 微信小程序 | 微信开放文档《小程序设计指南》developers.weixin.qq.com/miniprogram/design/ |
| 微信小程序（社区整理） | [`Billzhouheart/wechat-miniprogram-designUI`](https://github.com/Billzhouheart/wechat-miniprogram-designUI) |
| iOS | Apple Human Interface Guidelines developer.apple.com/design/human-interface-guidelines |
| Android | Material Design 3 m3.material.io |
| Web | 无统一规范，参考各设计系统 |

---

## 六·B、技术栈官方规范

| 技术栈 | 来源 |
|---|---|
| **uni-app x**（新一代） | [长度单位](https://doc.dcloud.net.cn/uni-app-x/css/common/length.html) —— **推荐 1x 画布 390px，用 px** |
| **uni-app**（经典版） | [页面样式与布局](https://uniapp.dcloud.net.cn/tutorial/syntax-css.html) —— rpx 750 基准 |
| **uni-app**（导航/TabBar） | [pages.json 页面路由](https://uniapp.dcloud.net.cn/collocation/pages.html) —— 导航 44px / tabBar 50px |
| **uni-app**（设计稿基准问答） | [DCloud 问答 221454](https://ask.dcloud.net.cn/question/221454) |
| **Taro** | [设计稿及尺寸单位](https://docs.taro.zone/docs/size) · [各端开发前注意](https://docs.taro.zone/docs/1.x/before-dev-remind) |
| **React Native** | reactnative.dev 官方文档 |
| **Flutter** | flutter.dev 官方文档 · Material 3 |
| **纯 H5** | 无统一规范；参考各 UI 库（Ant Design / Element Plus 等） |

---

## 七、质量与工程参考

| 仓库 | 用途 |
|---|---|
| [`plugin87/ux-ui-agent-skills`](https://github.com/plugin87/ux-ui-agent-skills) | 26 个 skill + 2144 设计系统 + WCAG 2.2 门禁 |
| [`jpoindexter/dec-skills`](https://github.com/jpoindexter/dec-skills) | 20 个设计工程 canon（UX 法则/无障碍/动效） |
| [`joepUI/motion-ref-skill`](https://github.com/joepUI/motion-ref-skill) | 111 个动效参考 |
| [`renfei-design/Figma-AI-Bridge`](https://github.com/renfei-design/Figma-AI-Bridge) | Figma MCP 能力面参考 |
| [`zhuyansen/awesome-claude-design-skills`](https://github.com/zhuyansen/awesome-claude-design-skills) | 196 个设计 skill 索引（安全分级） |
| [`bergside/design-md-figma`](https://github.com/bergside/design-md-figma) | Figma 提取 DESIGN.md |

---

## 八、闭环与生成质量（2026-11-10 调研）

> 调研主题：怎么让 AI 生成的 Figma 设计稿质量更高、更稳定、更像人做的。
> 完整报告：`plugins/design-foundation/research/figma-generation-optimization-2026-11-10.md`

### 闭环自检 / 视觉裁判

| 仓库 / 文章 | 做法 |
|---|---|
| [`model-citizen/design-loop`](https://model-citizen.dev/skills/design-loop/) | 独立 judge agent（干净上下文）+ 硬门禁；停滞 2 轮换结构性方案 |
| [`tathagat22/plumb-mcp`](https://github.com/tathagat22/plumb-mcp) | `plumb_review` = 结构 diff + 确定性 rubric + 视觉评判 → 排序修复清单 |
| [`zengwei-code/GameUIAgent`](https://github.com/zengwei-code/GameUIAgent) | 提示词 → JSON → 渲染 → GPT-4o 五维评分 → 反思迭代 |
| [`codenameakshay/design-compare-mcp`](https://github.com/codenameakshay/design-compare-mcp) | 参考 vs 实现打分（layout/color/content/typography/spacing + 动效） |
| [`amitpatole/agent-vision`](https://github.com/amitpatole/agent-vision) | render → perceive → report → fix；DOM 几何 + 计算样式对比度 |
| [`tonymfer/design-loop`](https://github.com/tonymfer/design-loop) | 分区截图 → 5 条反 slop 标准 1–5 分 → 修 top3 → 重复到 4/5+ |
| [`Lumi-node/design-loop-ai`](https://github.com/Lumi-node/design-loop-ai) | 确定性指标：WCAG 对比度（伽马校正）、布局对称性 |
| [`cgallic/design-os`](https://github.com/cgallic/design-os) | 规则目录 + block/flag/advise + waivers.yaml |

### 参考研究 / 品牌提取 / 生成质量

| 仓库 / 文章 | 做法 |
|---|---|
| [`mbeato/anti-slop-design`](https://github.com/mbeato/anti-slop-design) | 本地 281 真实应用 corpus → 检索 3 个结构正交竞品 → 用你的 token 重建 → 6 维评分 |
| [`zhuyansen/awesome-claude-design-skills`](https://github.com/zhuyansen/awesome-claude-design-skills) | 2026-10 实测：默认 AI 脸变成 teal on cream + serif headline |
| [`rohitg00/awesome-claude-design`](https://github.com/rohitg00/awesome-claude-design) | Claude Design 指纹清单（teal/状态点/container soup/默认衬线/三列网格…） |
| [`Dragoon0x/everything-design-taste`](https://github.com/Dragoon0x/everything-design-taste) | 288 skills + 8 条常驻规则 + taste-audit 5 维评分 |
| [NN/g：Prompt to Design Interfaces](https://www.nngroup.com/articles/vague-prototyping/) | 精确视觉关键词 > 冗长描述；附轻量参考图；首版当草稿 |
| [Figma 官方 MCP 最佳实践](https://help.figma.com/hc/en-us/articles/34855090823063-MCP-server-best-practices) | 组件 + Code Connect + 变量 + 语义命名 + auto layout |
| [dev.to：Figma MCP design system prep](https://dev.to/artemkireev/figma-mcp-design-system-prep-for-ai-agents-l61) | 硬编码 fill 是失败首因；变量名带用途；auto layout 被当意图读 |
| [MUI Recipes：AI prototyping](https://recipes.mui.com/blog/ai-prototyping-buildable-uis) | 强提示词定义「谁在做什么任务」；参考图要说明带什么丢什么 |

### 组件复用 / 生成即组件库

| 仓库 / 文章 | 做法 |
|---|---|
| [Figma `/figma-generate-design`](https://www.figma.com/resource-library/claude-skills-for-design/) | 从已批准组件库构建新页面 |
| [`miapre/mimic-ai`](https://github.com/miapre/mimic-ai) | 发现 DS → 匹配组件/token → 用真实实例 + 变量绑定构建 |
| [`musepy/genable`](https://github.com/musepy/genable) | 把受众/内容/层级/品牌/情绪/字体/密度/节奏写进 brief |
| [`guptaprakhariitr/aiconnect-figma-mcp`](https://github.com/guptaprakhariitr/aiconnect-figma-mcp) | `batch_ops` 整页一次往返；`apply_brand`；`check_contrast` |

---

## 八·B、通用组件生成（2026-11-11 调研）

> 调研主题：怎么让 AI 生成 Figma 时通用组件可复用、不重复造。
> 完整报告：`plugins/design-foundation/research/figma-common-component-generation-2026-11-11.md`
> 修改计划：`plugins/design-foundation/research/plugin-modification-plan-2026-11-12.md`

| 仓库 / 文章 | 做法 |
|---|---|
| [`tongsalangsingha/AI-prototyper-tool`](https://github.com/tongsalangsingha/AI-prototyper-tool) | 固定 `my_components.js`（32 组件）+ prompt 分解 + RAG 检索 → JSON 组件树 → Figma 原生节点 |
| [`nickciolpan/Rapid-Insert`](https://github.com/nickciolpan/Rapid-Insert) | 自然语言搜索组件 → 预览 → 组合（克隆 + 生成缺失）→ 插入 |
| [`figma/mcp-server-guide`](https://github.com/figma/mcp-server-guide) | `figma-generateLibraries`：变体 30 组合上限、scopes、codeSyntax、atoms→molecules→organisms 依赖顺序 |
| [`miapre/mimic-ai`](https://github.com/miapre/mimic-ai) | DS gap 检测 + 复用真实组件 / variables 构建 |
| [`robindicapua/figma-component-compose`](https://github.com/robindicapua/figma-component-compose) | 从代码组件提取 API → 生成 Figma 变体集（属性名与代码一致）→ 绑 DS 变量 |
| [`Liljimbo/Figma-Forge`](https://github.com/Liljimbo/Figma-Forge) | 把 Plugin API 封装成 26 个语义工具（`create_button` / `create_card`…） |
| [`musepy/genable`](https://github.com/musepy/genable) | 设计 agent 循环：计划 → 建原生结构 → 读回 → 截图验证 → 精修 |
| [`mrpeterperez/tellfigma`](https://github.com/mrpeterperez/tellfigma) | MCP 双向读写；读项目 Tailwind/CSS/组件 → 用**你的** token 生成 |
| [Figma 官方 structure-figma-file](https://developers.figma.com/docs/figma-mcp-server/structure-figma-file/) | 组件 + Code Connect + variables + 语义命名 + auto layout + annotations + dev resources |
| [`minha-eveningglow/design-something`](https://github.com/minha-eveningglow/design-something) | Figma / Penpot 抽象层，统一成 W3C DTCG token |

---

## 九、数据复核记录

| 日期 | 复核内容 | 结果 |
|---|---|---|
| 2026-11-04 | 首次采集 | — |
| 2026-11-10 | 闭环与生成质量专题调研（GitHub + 博客） | 新增 `figma-delivery/` 4 个文件，更新 anti-ai-slop / review-gate |
| 2026-11-11 | 通用组件生成专题调研 | 新增 `figma-delivery/component-catalog.md`、`reuse-first.md` 加检索目录步骤、`variants-states.md` 加矩阵治理 |

**复核清单**（每次复核时逐项确认）：
- [ ] 所有仓库是否仍然存在
- [ ] Star 数是否大幅变化（±50%）
- [ ] 最近提交时间（超过 1 年未更新的标注「停更」）
- [ ] 许可是否变更
- [ ] 平台规范是否有大版本更新（iOS / Android / 微信）
