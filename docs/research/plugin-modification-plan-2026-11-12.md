# 插件修改计划：通用组件生成优化

> 制定日期：2026-11-12
> 对象：`design-foundation` v0.4.0（`skills/design-foundation/` + `figwright-mcp@0.7.0` + `framelink-mcp`）
> 目标：让 AI 生成的 Figma 设计稿里，**通用组件**从「每次从零画」变成「检索目录 → 套配方 → 复用组件 → 绑变量 → 验证」。

---

## 0. 背景与依据

### 上一轮已经做完的（不要重复）

`research/figma-generation-optimization-2026-11-10.md` 的 P0 已落地：

- 截图自检闭环（SKILL.md 第 10.5 步 + `figma-delivery/verification-loop.md`）
- 参考提取（第 4.5 步 + `reference-extraction.md`）
- 复用优先（`reuse-first.md`）
- `design_diff` 基线（第 11 步）
- 变体状态、命名、变量令牌（`variants-states.md` / `naming.md` / `tokens-variables.md`）

### 本轮调研（2026-11-11）的结论

GitHub 与博客这一年的共识非常清楚：**通用组件生成的瓶颈不在「画得像」，在「复用」**。

| 来源 | 做法 | 对我们的意义 |
|---|---|---|
| `tongsalangsingha/AI-prototyper-tool` | 固定 `my_components.js`（32 个组件）+ prompt 分解 + RAG 检索，LLM 只能用检索到的组件 | **可检索的组件目录**是最核心的一块 |
| `figma/mcp-server-guide`（figma-generateLibraries） | 变体矩阵 30 组合上限 + 拆分策略 + scopes + codeSyntax + 分层依赖 | 变体与变量的**硬规则** |
| `miapre/mimic-ai` | 发现设计系统 → 匹配真实组件/token → 缺口检测 | 复用 + **gap 报告** |
| `Liljimbo/Figma-Forge` / `musepy/genable` | 把 Plugin API 封装成高层语义工具 | **组件配方**（语义操作层） |
| Figma 官方 structure-figma-file | 组件 + Code Connect + variables + 命名 + auto layout + annotations + dev resources | 文件结构即上下文 |

### 现有插件的 4 个缺口

| 文件 | 已有 | 缺 |
|---|---|---|
| `reuse-first.md` | 盘点**文件内**已有组件 → 映射 → 复用 | 没有「通用组件目录」可检索 |
| `variants-states.md` | 状态变体、4 个属性上限 | 30 组合上限、拆分策略、INSTANCE_SWAP 承担 icon 轴 |
| `tokens-variables.md` | 三层令牌、命名、绑定 | `scopes`（禁止 ALL_SCOPES）、`codeSyntax` 的 `var()` 包装 |
| `structure.md` | 命名、变体、描述、分区 | Code Connect / annotations / dev resources |

---

## 1. 改动总览

| # | 文件 | 动作 | 优先级 |
|---|---|---|---|
| 1 | `research/figma-common-component-generation-2026-11-11.md` | **新增**：本轮调研记录 | P0 |
| 2 | `references/figma-delivery/component-catalog.md` | **新增**：通用组件目录 | P0 |
| 3 | `references/figma-delivery/reuse-first.md` | **改**：加「检索通用组件目录」步骤 | P0 |
| 4 | `references/figma-delivery/variants-states.md` | **改**：加变体矩阵治理 | P0 |
| 5 | `SKILL.md` | **改**：第 10 步流程 + 评测表 | P0 |
| 6 | `references/figma-delivery/component-recipes.md` | **新增**：通用组件配方 | P1 |
| 7 | `references/figma-delivery/tokens-variables.md` | **改**：加 scopes + codeSyntax | P1 |
| 8 | `references/figma-delivery/structure.md` | **改**：加 Code Connect / annotations / dev resources | P2 |
| 9 | `references/figma-delivery/verification-loop.md` | **改**：加缺口报告 + 预览插入 | P2 |
| 10 | `references/00-index.md` / `01-sources.md` | **改**：更新索引与来源 | P0 |

---

## 2. 新增文件

### 2.1 `references/figma-delivery/component-catalog.md`（P0，最核心）

**作用**：给通用组件建一个**可检索的目录**，生成时先查目录，而不是凭空画。参考 `AI-prototyper-tool` 的 `my_components.js`（32 个组件，结构为 `{name, description, props[]}`）。

**每个组件的条目结构**：

```yaml
name: Button
description: 用于用户动作（提交/保存/取消）。意图式写法，供检索命中。
layer: atom            # atom / molecule / organism / pattern
props:
  - name: label
    type: string
    required: true
  - name: color
    type: enum
    options: [primary, secondary, destructive]
  - name: icon
    type: icon          # 引用 libraries/icons.md 的图标库
  - name: width
    type: enum
    options: [FULL, HUG]
    default: FULL
variants:               # 变体轴（乘积 ≤30）
  Type: [Primary, Secondary, Ghost, Danger]
  Size: [Large, Medium, Small]
  State: [Default, Hover, Pressed, Disabled, Loading]
tokens: [color/brand/primary, color/text/primary, space/4, radius/md]
states: [Default, Disabled, Loading]
```

**覆盖清单**（按插件的平台/技术栈适配，不限于 32 个）：

| 层 | 组件 |
|---|---|
| 原子 | Button、Social Button、Checkbox、RadioButton、Switch、Icon、Link Text、Slider、ColorSwatch、Avatar、Badge、Divider、Spinner、FAB、Progress Bar、Tag、Chip |
| 文字 | Header、Label、Body Text |
| 媒体 | Image、Placeholder |
| 输入 | Text Field、Dropdown、Search Bar |
| 容器 | Row、Column、Card、List Item |
| 导航 | Navigation Bar、Top App Bar、TabBar（小程序/H5）、SideBar |
| 浮层 | Snackbar、Modal、Toast、Tooltip |
| 模式 | EmptyState、Skeleton、FormField、PageHeader、FilterBar |

**检索规则**（写进文件开头）：

1. 页面拆成区块（导航 / 搜索 / 列表 / 卡片 / 表单 / 空态…）
2. 每个区块**先查本目录** → 命中则用目录的组件 API
3. 未命中 → 查文件内已有组件（`component_map`）
4. 都没有 → 新建，并记入「组件缺口报告」

**维护规则**：新增组件走 `02-how-to-add.md`，保持 description 意图式、prop 带枚举。

---

### 2.2 `references/figma-delivery/component-recipes.md`（P1）

**作用**：把常见通用组件写成「按什么顺序调哪些 figwright 工具 + `batch` 一次写入」的**配方**，相当于 Figma Forge 的语义工具层，但以文档形式落地。模型照配方拼，比自由发挥稳。

**每个配方包含**：名称 / 用途 / 组成（含依赖组件）/ 工具调用顺序 / `batch` 示例 / 注意事项。

**示例**：

```yaml
name: SearchBar
use: 顶部搜索，内容筛选与发现
compose:
  - create_frame(name: "SearchBar", layout: HORIZONTAL, padding: space/4, gap: space/2, cornerRadius: radius/full)
  - create_instance(component: Icon/search, size: 20)
  - create_text(characters: placeholder, style: Text/Body/MD, fill: color/text/secondary)
tools: [create_frame, create_instance, create_text, set_auto_layout, batch]
deps: [Icon]
tokens: [space/4, space/2, radius/full, color/text/secondary]
```

**配方清单**：SearchBar、ListItem、Card、FormField、NavBar、TabBar、EmptyState、Skeleton、Modal、Toast、FilterBar、PageHeader。

---

### 2.3 `research/figma-common-component-generation-2026-11-11.md`（P0）

本轮调研记录：来源清单 + 方案地图 + 与上一轮的差异。保持可追溯。

---

## 3. 修改文件

### 3.1 `SKILL.md`（P0）

**第 10 步改造**：在「先盘点，再动手」后加入检索目录：

```
**先盘点，再动手**（读 references/figma-delivery/reuse-first.md）：

0. get_local_components 列出目标文件已有组件
1. 读 references/figma-delivery/component-catalog.md，把页面拆成区块，逐个匹配通用组件
2. component_map 把要生成的页面映射到现有组件
3. 命中的用 create_instance 拉实例 + set_instance_properties 覆写；套配方（component-recipes.md）
4. 映射不上的才新建，新建顺手 create_component 转组件
5. 新建后输出「组件缺口报告」
```

**评测表加一条**：

| # | Prompt | 预期 |
|---|---|---|
| 10 | 「生成一个含通用组件的页面」 | 必须命中组件目录；变体组合 ≤30；token 绑定率 100%；输出组件缺口报告 |

**第 8 步门禁**加：变量绑定覆盖率 100%；页面区块对组件目录的命中率。

---

### 3.2 `references/figma-delivery/reuse-first.md`（P0）

在「第 1 步：盘点」和「第 2 步：映射」之间插入：

```markdown
### 第 1.5 步：检索通用组件目录

读 `references/figma-delivery/component-catalog.md`。把页面拆成区块
（导航 / 搜索 / 列表 / 卡片 / 表单 / 空态…），逐个区块匹配目录里的通用组件。

- 命中 → 用目录的组件 API（props / 变体 / 状态 / 依赖 token）建或复用
- 未命中 → 走第 2 步 `component_map` 映射文件内已有组件
- 都没有 → 新建，并记入「组件缺口报告」

**门禁**：页面区块对目录的命中率；未命中区块必须有说明。
```

---

### 3.3 `references/figma-delivery/variants-states.md`（P0）

新增「六、变体矩阵治理」：

- **30 组合上限**：所有变体轴（Size × Style × State × Icon…）的乘积**不得超过 30**。
- **拆分策略**（超限时按顺序选）：
  1. 按主风格拆成多个 component set（如 PrimaryButton / SecondaryButton）
  2. 把 icon 轴从变体矩阵移除，改用 **INSTANCE_SWAP** 属性承担
  3. 抽 **Building Blocks** 子组件（带独立状态轴的子元素，命名前缀 `Building Blocks/` 或 `.Building Blocks/`）
- **网格排版**：列 = State（Default/Hover/Pressed/Disabled），行 = Size×Style（Size 变化最快），gap 20，padding 40。
- **确定性命名**：`Button/Type=Primary,Size=Large,State=Default`，与幂等清理和续跑配套。
- 来源：`figma/mcp-server-guide` → `component-creation.md`。

保留现有「变体属性不超过 4 个」，补上「乘积不超 30」。

---

### 3.4 `references/figma-delivery/tokens-variables.md`（P1）

新增「八、变量作用域与代码名」：

- **scopes**：**禁止 `ALL_SCOPES`**。按用途收窄：
  - 背景：`FRAME_FILL, SHAPE_FILL`
  - 文字：`TEXT_FILL`
  - 描边：`STROKE_COLOR`
  - 间距：`GAP`
  - 圆角：`CORNER_RADIUS`
  - 原语：`[]`（隐藏）
- **codeSyntax**：WEB **必须**用 `var()` 包装，如 `var(--color-brand-primary)`，不用裸 `--color-brand-primary`；ANDROID / iOS 不用 wrapper。
- **工具**：`create_variable(scopes)` + `set_variable_code_syntax`。
- **硬门禁**：变量绑定覆盖率 100%（所有 fill / stroke / text / padding / radius / gap 都绑变量）。
- 来源：`figma/mcp-server-guide` → `token-creation.md`。

---

### 3.5 `references/figma-delivery/structure.md`（P2）

新增「九、让 agent 读懂你的文件」：

- **Code Connect**：把 Figma 组件连到真实代码组件——这是让 agent 复用真实组件**最有效**的手段。批量组件用 batch files（`.figma.batch.ts` + `.figma.batch.json`）。
- **annotations**：解释非视觉信息（行为 / 对齐 / 响应式规则）。
- **dev resources**：Dev Mode 里给图层挂文档 / 代码 / 工单链接。
- 来源：Figma 官方 `structure-figma-file` 指南。

---

### 3.6 `references/figma-delivery/verification-loop.md`（P2）

新增两节：

- **组件缺口报告**：每轮生成后输出「未命中目录 / 文件组件的区块清单 + 建议补充的组件」，参考 `mimic-ai` 的 DS gap detection。
- **预览后插入**：复杂布局先出预览（`get_screenshot`）确认，再 commit，参考 `Rapid Insert`。
- 确定性检查表加一条：**目录命中率**（用 `get_local_components` + 目录清单比对）。

---

### 3.7 `references/00-index.md` / `01-sources.md`（P0）

- `00-index.md` 目录地图加 `component-catalog.md` / `component-recipes.md`。
- `01-sources.md` 补本轮来源（AI-prototyper、figma/mcp-server-guide、mimic-ai、Figma Forge、Genable、Rapid Insert、structure-figma-file）。

---

## 4. 实施顺序

| 阶段 | 内容 | 产出 |
|---|---|---|
| 1 | 调研记录 + 组件目录 | `research/figma-common-component-generation-2026-11-11.md`、`component-catalog.md` |
| 2 | 复用流程改造 | `reuse-first.md`、`variants-states.md`、`SKILL.md` |
| 3 | **实测回归** | 跑评测 prompt 10，看通用组件是否不再各画各的 |
| 4 | 配方与变量规则 | `component-recipes.md`、`tokens-variables.md` |
| 5 | 结构补强 | `structure.md`、`verification-loop.md` |
| 6 | 索引与来源 | `00-index.md`、`01-sources.md` |

**阶段 3 是关卡**：实测不通过就停下来调目录和流程，别继续往下堆。

---

## 5. 验收标准

- [ ] 评测 prompt 10 通过：命中目录、变体 ≤30 组合、token 绑定率 100%、输出缺口报告
- [ ] 同一页面里同类组件（按钮/卡片/输入）只有 1 个主组件 + 变体
- [ ] `token_map` 返回的 `unmapped` 为空
- [ ] 变体集网格排版、确定性命名
- [ ] 连续 2 轮生成，同类组件不重复造

---

## 6. 风险与回滚

- **纯文档改动，无代码风险**：不涉及 `plugin.json` / MCP 配置。
- **每步独立可回滚**：新增文件不影响现有流程，直到 `SKILL.md` 引用它们。
- **目录过大的风险**：`component-catalog.md` 别一次写满 — 先写高频 15–20 个，实测后再补。
- **配方与工具脱节的风险**：配方里的工具名以 `figwright-mcp@0.7.0` 实际工具为准，写前先核对工具列表。
- **回滚方式**：改动前用 `git`（若可用）或复制一份 `skills/` 目录；本插件目录非 git 仓库，改前先备份。

---

## 7. 一句话总结

**知识库已经够厚，缺的是「通用组件的检索与复用机制」。** 先做组件目录 + 检索步骤 + 变体治理（P0），实测通过后再补配方、变量规则和结构补强（P1/P2）。
