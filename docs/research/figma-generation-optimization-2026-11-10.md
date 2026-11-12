# 优化「生成 Figma 设计」能力：业界方案调研与落地建议

> 调研日期：2026-11-10
> 对象：`design-foundation` 插件 v0.3.8（`skills/design-foundation/` + `figwright-mcp@0.7.0` + `framelink-mcp`）
> 问题：**怎么让 AI 生成的 Figma 设计稿质量更高、更稳定、更像人做的**

---

## 0. 结论速览

你的知识库（平台/技术栈/风格/字体/图标/质量/交付）已经很完整，**短板不在「知识」，在「闭环」**。

业界这一年的共识非常清楚：**单靠提示词和知识库，已经到顶了；真正拉开差距的是「渲染 → 看 → 打分 → 修」的闭环，以及独立裁判。**

按杠杆大小排序，最值得做的 5 件事：

| # | 优化 | 一句话 | 优先级 |
|---|---|---|---|
| 1 | **截图自检闭环** | 生成后必须截图，用独立裁判打分，修到 ≥8/10 或停滞 2 轮 | P0 |
| 2 | **参考研究 + 品牌提取** | 选风格后，从真实参考站/截图提取色板/字阶/密度，而不是凭空定 | P0 |
| 3 | **组件复用优先** | 先盘点现有组件库 → 映射 → 复用实例，缺的才新建 | P0 |
| 4 | **design_diff 基线** | 产出后存 baseline，走查时用 diff 精确定位偏差 | P1 |
| 5 | **2026 反 AI 味指纹** | 你的清单还是 2024 版（紫渐变/emoji/三卡片），默认 AI 脸已经变了 | P1 |

---

## 1. 现状能力盘点

### 你已经有的（别重复造）

- **知识库**：16 个风格卡片、平台规范、技术栈基准、字体/图标库、UX 法则、无障碍、动效、反 AI 味、门禁、命名/结构/变量/交付规范。
- **MCP 写能力**（`figwright-mcp`）很强：`create_frame` / `set_auto_layout` / `create_component` / `combine_as_variants` / `add_component_property` / `create_instance` / `batch`（整页一次原子写入）/ `set_variable_*` / `bind_variable_to_paint` / `token_map` / `component_map` / `icon_map` / `design_diff` / `get_screenshot` / `get_design_context` / `set_reactions` / `get_motion_context`。
- **读能力**（`framelink-mcp`）：`get_figma_data` 读 REST API 数据、`download_figma_images` 取图。

### 你缺的（差距所在）

| 业界做法 | 你的现状 |
|---|---|
| 生成后截图 → 独立裁判打分 → 修 → 再截 | 第 11 步「走查修正」是**人工核对**，模型自己说「已生成」 |
| 从真实参考提取品牌 | 从 16 张静态风格卡片里选，没有参考提取 |
| 优先复用现有组件库 | 第 10 步直接建页面，没查现有组件 |
| 用 `design_diff` 存基线 | 工具在，但 SKILL.md 里**一次都没提** |
| 自动检查（对比度/间距/绑定率/命名） | 门禁是打勾清单，没有可执行的自动检查 |
| 变体/状态用 `combine_as_variants` | 规范里写了，生成时通常只画 default |
| 变量绑定到代码 token（`codeSyntax`） | 规范说「Figma `/` 对应代码 `-`」，但没用工具落地 |

---

## 2. 业界方案地图（GitHub + 博客）

### A. 闭环自检 / 视觉裁判（最值得学）

| 项目 | 做法 | 链接 |
|---|---|---|
| **model-citizen/design-loop** | 独立 `design-judge` agent（干净上下文）按 rubric 打分，硬门禁（无障碍/令牌/运行时/资产许可）不过不许退出；停滞 2 轮就换结构性方案 | https://model-citizen.dev/skills/design-loop/ |
| **tathagat22/plumb-mcp** | `plumb_review` = 结构 diff + 确定性 rubric + 调用方自己的视觉评判，返回排序后的修复清单，迭代到过线 | https://github.com/tathagat22/plumb-mcp |
| **zengwei-code/GameUIAgent** | 六阶段：提示词 → Design Spec JSON → 后处理 → Figma 渲染 → **GPT-4o 五维评分** → 反思控制器迭代（带不回退保证） | https://github.com/zengwei-code/GameUIAgent |
| **codenameakshay/design-compare-mcp** | 参考 vs 实现打分（layout/color/content/typography/spacing + 动效），返回 punch-list，专治「hill-climb the clone」 | https://github.com/codenameakshay/design-compare-mcp |
| **amitpatole/agent-vision** | render → perceive → report → fix；用 DOM 几何 + 计算样式对比度 + OCR 词框，视觉 LLM 只做语义层 | https://github.com/amitpatole/agent-vision |
| **tonymfer/design-loop** | 分区截图（语义 landmark）→ 5 条反 slop 标准 1–5 分 → 修 top3 → 重复到 4/5+；有停滞检测 | https://github.com/tonymfer/design-loop |
| **Lumi-node/design-loop-ai** | 确定性指标库：WCAG 对比度（伽马校正亮度）、布局对称性、无障碍、响应式断点 | https://github.com/Lumi-node/design-loop-ai |
| **cgallic/design-os** | 规则目录（稳定 ID + 阈值 + `block`/`flag`/`advise`）+ 三镜头评判 + `waivers.yaml` 唯一合法破例 | https://github.com/cgallic/design-os |

### B. 参考研究 + 品牌提取

| 项目 | 做法 | 链接 |
|---|---|---|
| **tathagat22/plumb-mcp** | `plumb_brand`：实时截图最佳同类站 → 读计算 CSS → 合成色板 + 字阶 → 画成品牌板 | https://github.com/tathagat22/plumb-mcp |
| **mbeato/anti-slop-design** | 本地 ~281 个真实应用 corpus（Apple/Linear/Stripe/Whoop）→ 检索 3 个结构正交的竞品模式 → 用**你的** token 重建 → Playwright 渲染 → 6 维评分 → lessons store 越用越准 | https://github.com/mbeato/anti-slop-design |
| **NN/g「Prompt to Design Interfaces」** | 用精确视觉关键词（玻璃拟态/Y2K/Swiss）代替「简洁现代」；附轻量参考图（moodboard/截图） | https://www.nngroup.com/articles/vague-prototyping/ |

### C. 生成质量 / 反 AI 味

| 项目 | 做法 | 链接 |
|---|---|---|
| **zhuyansen/awesome-claude-design-skills** | 2026-10 实测 10 个 skill × 2 次同一落地页：**默认 AI 脸已变成「teal on cream + serif headline」**；2/10 个 skill 完全复现默认；7 项检查全过 | https://github.com/zhuyansen/awesome-claude-design-skills |
| **rohitg00/awesome-claude-design** | Claude Design 指纹清单：teal accent、blinking status dot、container soup、默认衬线标题、卡片左侧 accent bar、三列特性网格、Lucide 默认图标、生成式 hero 忽略 token | https://github.com/rohitg00/awesome-claude-design |
| **Dragoon0x/everything-design-taste** | 288 skills + 8 条常驻规则 + `taste-audit` 5 维加权评分（意图 30%/工艺 25%/AI slop 20%/文案 15%/无障碍 10%） | https://github.com/Dragoon0x/everything-design-taste |
| **Figma 官方 MCP 最佳实践** | 组件 + Code Connect + 变量 + 语义命名 + auto layout + 注释；小选择生成；`get_design_context` + `get_screenshot` 缺一不可 | https://help.figma.com/hc/en-us/articles/34855090823063-MCP-server-best-practices |
| **dev.to：Figma MCP design system prep** | 硬编码 fill/spacing 是失败首因；变量名要带**用途**（semantic）；auto layout 被 agent 当意图读 | https://dev.to/artemkireev/figma-mcp-design-system-prep-for-ai-agents-l61 |
| **MUI Recipes：AI prototyping** | 强提示词定义「谁在做什么任务」，不是「做个仪表盘」；参考图要说明「带什么、丢什么」 | https://recipes.mui.com/blog/ai-prototyping-buildable-uis |

### D. 组件复用 / 生成即组件库

| 项目 | 做法 | 链接 |
|---|---|---|
| **Figma `/figma-generate-design`** | 从**已批准组件库**构建新页面，不重新发明 | https://www.figma.com/resource-library/claude-skills-for-design/ |
| **miapre/mimic-ai** | 发现文件里的设计系统 → 匹配组件/token → 用真实组件实例 + 变量绑定 + auto layout 构建 | https://github.com/miapre/mimic-ai |
| **musepy/genable** | 把「受众/内容/层级/品牌/情绪/字体/密度/节奏」都写进 brief，逼模型做**构图决策**而不是卡片网格 | https://github.com/musepy/genable |
| **guptaprakhariitr/aiconnect-figma-mcp** | `batch_ops` 整页一次往返；`apply_brand` 从预设/颜色生成整套 token；`check_contrast` + 自动修复；20 万 Iconify 图标 | https://github.com/guptaprakhariitr/aiconnect-figma-mcp |

### E. 提示词工程（写 brief 的方法）

| 来源 | 要点 |
|---|---|
| **OpenAI 前端提示词指南** | 先定设计系统和约束；给视觉参考；先定叙事/内容策略；一个 H1、≤6 段、≤2 字体、1 个强调色 |
| **InterfaceKit 五段式** | Define → Map → Build → Test → Correct，每段一个提示词、一个可评审结果 |
| **NN/g** | 精确视觉关键词 > 冗长模糊描述；附轻量参考图；首版当草稿，指回同一张图纠偏 |
| **Layout UI** | 一次只改一处；引用「你看到的 UI」；贴截图要说明「带什么、忽略什么」 |

---

## 3. 关键差距分析

### 差距 1：没有闭环（最大问题）

**业界**：生成 → 截图 → 独立裁判 → 修 → 再截 → 过线才停。
**你**：第 10 步生成 → 第 11 步「逐项核对简报 vs 实际产出，列出偏差」（人工）。

**后果**：模型说「已生成」就等于完成；截图里的问题（溢出、截断、间距不齐、对比度不够、AI 味）全靠人事后发现。

### 差距 2：没有参考提取

**业界**：先看真实参考 → 提取色板/字阶/密度 → 再生成。
**你**：从 16 张风格卡片里选一张。

**后果**：风格卡片是「形容词」，参考提取是「数值」。你的技能自己写着「数值优先于形容词」，但流程里没有把参考转成数值的一步。

### 差距 3：组件不复用

**业界**：先盘点现有组件库，能复用就复用实例。
**你**：第 10 步直接建页面。

**后果**：每次生成都从零画，同一个按钮画 5 种；后续维护噩梦；和「设计系统」理念矛盾。

### 差距 4：2026 反 AI 味指纹没更新

**业界实测**：默认 AI 脸从「紫渐变」变成「teal on cream + serif headline」。
**你**：`anti-ai-slop.md` 主要写紫渐变/emoji/三卡片。

**后果**：你挡住了 2024 年的 AI 味，2026 年的 AI 味正大摇大摆走出去。

### 差距 5：门禁不可执行

**业界**：确定性检查（对比度算式、令牌覆盖率、auto layout 覆盖率）+ 硬门禁（block 不过不许走）+ 评分。
**你**：门禁是打勾清单，靠模型自觉。

**后果**：「颜色全部来自令牌」这种项，不实际扫一遍永远不知道过没过。

---

## 4. 优化方案（按优先级）

### P0-1 截图自检闭环（新增第 10.5 步）

**做什么**：在 SKILL.md 第 10 步（MCP 落地）和第 11 步（走查）之间，插入「第 10.5 步：截图自检闭环」。

**流程**：

1. **截**：对每个页面 `get_screenshot`（或 `save_screenshots` 存到 `_work/`）。
2. **判**：**不要模型自评**。开一个干净上下文的子 agent（本 harness 有 `visual-judge`；没有就用子 agent 只给它截图 + rubric），按 5 维打分：
   - 构图与层级 / 排版 / 色彩与对比度 / 视觉身份（是否像生成的）/ 一致性
   - 每维 1–5，总分 ≥8/10 才过。
3. **确定性检查**（能自动算的不要靠眼睛）：
   - 对比度：写个小脚本读截图或节点色值算 WCAG（参考 design-loop-ai 的伽马校正公式）。
   - 间距：检查是否 4 的倍数。
   - 令牌绑定：`token_map` 跑一遍，看有没有未映射的写死值。
   - auto layout 覆盖率：`search_nodes` 统计 Frame 中未用 auto layout 的比例。
   - 命名：`search_nodes` 找 `Frame \d+` / `Group \d+` 这类默认名。
4. **修**：按裁判的排序清单修，**先修最难的**，不要挑软的。
5. **再截**：回到第 1 步。
6. **退出条件**：
   - 总分 ≥8/10 且硬门禁（对比度/令牌/平台尺寸）全过 → 收工。
   - 连续 2 轮没涨分，或裁判连续 2 次说同一个问题 → **停**，找结构性原因（布局系统/色板/字阶错了），做一次大改，不是五次小改。
   - 默认上限 5 轮。

**为什么**：这是业界公认的**最大杠杆**。model-citizen 的原话：「建造它的 agent 不能给它打分，所以要有独立裁判。」

**来源**：model-citizen/design-loop、plumb-mcp、GameUIAgent、agent-vision、design-compare-mcp

---

### P0-2 参考研究 + 品牌提取（升级第 4 步）

**做什么**：第 4 步选风格后，增加一个**可选但推荐**的「参考提取」子步骤。

**流程**：

1. 问用户要 1–3 个参考（网址 / 截图 / 竞品）。
2. 提取：
   - 网址 → 用 `browser-use` skill 打开截图 + 读计算样式（色板/字阶/圆角/间距/密度）。
   - 截图 → 直接读图，提取主色/辅色/字体感觉/密度。
3. 产出一张「品牌提取板」：主色、辅色、字阶、圆角、间距、密度、情绪关键词，**带数值**。
4. 写进《设计规范简报》，作为后续生成的唯一真源。
5. 没有参考 → 用风格卡片的数值，并**明确告诉用户「没给参考，用的是风格卡默认值」**。

**为什么**：把「形容词」转成「数值」，正好落实你技能里「数值优先于形容词」的原则。plumb-mcp 的 `plumb_brand` 已经证明这条路可行。

**来源**：plumb-mcp、mbeato/anti-slop-design、NN/g

---

### P0-3 组件复用优先（改造第 10 步）

**做什么**：第 10 步改成三段：

1. **盘点**：`get_local_components` 列出目标文件已有组件；`component_map` 把要生成的页面映射到现有组件。
2. **复用**：能复用的用 `create_instance` 拉实例，用 `set_instance_properties` 覆写（Text/Boolean/Variant/Instance Swap）。
3. **新建**：只有映射不上的才新建，新建时顺手 `create_component` + `combine_as_variants`。

**门禁加一条**：页面中「组件实例 + 变体」覆盖率达到一定比例；纯手搭的 Frame 算技术债。

**为什么**：Figma 官方 `/figma-generate-design` 的核心就是「从已批准组件库构建」；mimic-ai 用真实组件实例 + 变量绑定。你的 figwright 有 `component_map` / `create_instance`，但 SKILL.md 里没要求用。

**来源**：Figma /figma-generate-design、mimic-ai、figwright component_map

---

### P1-1 用 design_diff 存基线（改造第 11 步）

**做什么**：

1. 第 10 步产出后，对每个页面 `design_diff` 存 baseline（首次调用返回 `baseline-created`）。
2. 第 11 步走查时，用 `design_diff` 替代肉眼比对——它返回 per-node、per-property 的变化（fills/layout/text/token 绑定），能精确说出「哪里偏了」。
3. 后续修改只动受影响节点，改完再 diff 确认。

**为什么**：`design_diff` 是你 figwright 里**已经存在但 SKILL.md 一次没提**的工具。它天生就是为「设计改了，只改受影响的部分」设计的。

**来源**：figwright-mcp `design_diff`

---

### P1-2 变量代码名 + 自动 token 映射

**做什么**：

1. 第 10 步建变量时，用 `set_variable_code_syntax` 声明 WEB 名（如 `--color-brand-primary`）。
2. 用 `token_map` 把 Figma 变量映射到**项目现有** token（CSS/Tailwind/SCSS）。
3. 门禁加两条硬检查：
   - 无写死 hex（`token_map` 返回的 unmapped 为空）。
   - 变量绑定覆盖率 100%（所有 fill/stroke/text 都绑变量）。

**为什么**：你的 `tokens-variables.md` 写了「Figma `/` 对应代码 `-`」，但这是**约定**，不是**机制**。工具能直接声明和映射，就别靠约定。

**来源**：figwright `set_variable_code_syntax` / `token_map`、Medium 生产级 DS 文章

---

### P1-3 语义命名 + auto layout 覆盖率自动检查

**做什么**：第 10 步后加一次自动扫描：

- `search_nodes` 搜 `Frame \d+`、`Group \d+`、`Rectangle \d+` 等默认名 → 批量 `batch_rename_nodes`。
- 统计 auto layout 帧占比，低于阈值就补。
- 门禁加两条：默认名 = 0；auto layout 覆盖率 ≥90%。

**为什么**：Figma 官方明确说「agent 把 auto layout 当意图读」；「Frame 234」对 agent 零信息。你的 naming.md 有规范，但生成时靠自觉，没有检查。

**来源**：Figma MCP 最佳实践、dev.to design system prep

---

### P1-4 状态/变体补全（combine_as_variants）

**做什么**：第 10 步对交互组件强制建 component set：

- 按钮：default / pressed / disabled / loading（至少 4 态）。
- 输入框：default / focus / error / disabled。
- 用 `combine_as_variants` 组集，用 `add_component_property` 暴露 Text/Boolean/Instance Swap。

**为什么**：你的门禁要求状态齐全，但生成时通常只画 default。工具能直接组变体，别让模型手画 4 份。

**来源**：Figma MCP 最佳实践、structure.md（已有规范，缺落地）

---

### P2-1 更新反 AI 味指纹库

**做什么**：更新 `quality/anti-ai-slop.md`，加入 2026 版指纹：

- **teal/cyan 强调色**（Claude Design 默认 `#16d5e6` 附近）
- **blinking status dot**（导航右上角动画绿点）
- **container soup**（pill 套 card 套 card，24/24/24 堆叠）
- **默认衬线标题**（Tiempos/Source Serif 类）
- **卡片左侧 4px accent bar**（无语义装饰）
- **三列特性网格**（landing 第二段几乎必是这个）
- **Lucide 默认图标**（要么单一家族，要么纯文字）
- **生成式 hero 忽略 token**（图片配色和 DESIGN.md 无关）

**同时保留**：你现有的「遮住 logo 测试」「换个人做会一样吗」——这两个是最好的直觉测试。

**为什么**：awesome-claude-design-skills 2026-10 实测发现默认 AI 脸已经变了；你的清单还停在 2024 版。

**来源**：awesome-claude-design-skills、rohitg00/awesome-claude-design

---

### P2-2 评分制门禁 + waiver 机制

**做什么**：把 `quality/review-gate.md` 从「打勾」升级成「打分 + 硬门禁」：

- 每维 1–5 打分（平台 25%/无障碍 20%/完整性 20%/一致性 15%/反 AI 味 10%/细节 10%）。
- **硬门禁（block）**：对比度、平台尺寸、令牌绑定、状态齐全——不过不许走。
- **waiver 机制**：唯一合法破例方式是写 waiver（作用域 + 理由 + 过期时间），参考 design-os。

**为什么**：打勾清单会被「我觉得过了」糊弄过去；评分 + block + waiver 是业界更成熟的做法。

**来源**：design-os、everything-design-taste、GameUIAgent

---

### P2-3 动效/原型连接

**做什么**：多页原型用 `set_reactions` 连 `ON_CLICK` 跳转；用 `get_motion_context` 盘点动效。

**为什么**：design-compare-mcp 专门做动效保真度；你的 motion.md 有规范，但第 10 步不要求连。

**来源**：design-compare-mcp、figwright reactions

---

## 5. 建议的落地改动清单

### SKILL.md 改动

| 位置 | 改动 |
|---|---|
| 第 4 步后 | 新增「第 4.5 步：参考提取（可选但推荐）」 |
| 第 10 步 | 改成「盘点 → 复用 → 新建」三段；加 component set / 变体要求 |
| 第 10 步后 | 新增「第 10.5 步：截图自检闭环」 |
| 第 11 步 | 用 `design_diff` 替代肉眼比对 |
| 评测表 | 加「生成后必须有截图 + 评分」的回归 prompt |

### references 新增/更新

| 文件 | 动作 |
|---|---|
| `quality/anti-ai-slop.md` | 更新 2026 指纹 |
| `quality/review-gate.md` | 打分 + block + waiver |
| `figma-delivery/verification-loop.md` | **新增**：截图自检闭环 rubric + 确定性检查脚本 + 退出条件 |
| `figma-delivery/reuse-first.md` | **新增**：组件盘点/映射/复用流程 |
| `figma-delivery/reference-extraction.md` | **新增**：参考提取流程 |
| `figma-delivery/variants-states.md` | **新增**：变体/状态补全规范 |
| `01-sources.md` | 补这次调研的来源 |

### 可选：脚本

在插件外加一个 `scripts/design-check.mjs`（放项目目录，不进插件）：
- 输入：截图 + 节点色值
- 输出：对比度、间距 4 的倍数、令牌覆盖率、auto layout 覆盖率、默认名数量
- 参考 `Lumi-node/design-loop-ai` 的确定性指标

---

## 6. 一句话总结

**你的知识库已经够厚，缺的是「闭环」和「执行机制」。**

- 闭环：生成 → 截图 → 独立裁判 → 修 → 再截（P0）
- 机制：复用优先、design_diff 基线、变量代码名、自动扫描、评分门禁（P1）
- 内容：2026 反 AI 味指纹（P1）

把这三块补上，生成质量会有质的变化——业界这一年的项目（plumb-mcp、design-loop、GameUIAgent、design-compare-mcp）都在往同一个方向收敛。

---

## 附：本次调研来源清单

- https://github.com/tathagat22/plumb-mcp
- https://model-citizen.dev/skills/design-loop/
- https://github.com/zengwei-code/GameUIAgent
- https://github.com/codenameakshay/design-compare-mcp
- https://github.com/amitpatole/agent-vision
- https://github.com/tonymfer/design-loop
- https://github.com/Lumi-node/design-loop-ai
- https://github.com/cgallic/design-os
- https://github.com/mbeato/anti-slop-design
- https://www.nngroup.com/articles/vague-prototyping/
- https://github.com/zhuyansen/awesome-claude-design-skills
- https://github.com/rohitg00/awesome-claude-design
- https://github.com/Dragoon0x/everything-design-taste
- https://help.figma.com/hc/en-us/articles/34855090823063-MCP-server-best-practices
- https://dev.to/artemkireev/figma-mcp-design-system-prep-for-ai-agents-l61
- https://recipes.mui.com/blog/ai-prototyping-buildable-uis
- https://www.figma.com/resource-library/claude-skills-for-design/
- https://github.com/miapre/mimic-ai
- https://github.com/musepy/genable
- https://github.com/guptaprakhariitr/aiconnect-figma-mcp
- https://developers.openai.com/api/docs/guides/frontend-prompt
- https://guides.interfacekit.io/prompt-ai-for-ui-design
- https://layoutui.com/docs/ai
