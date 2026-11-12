# 通用组件生成优化调研（2026-11-11）

> 调研主题：怎么让 AI 生成 Figma 时，**通用组件**（按钮 / 卡片 / 输入 / 导航…）可复用、不重复造
> 对象：`design-foundation` v0.4.0
> 完整修改计划：`research/plugin-modification-plan-2026-11-12.md`
> 上一轮调研：`research/figma-generation-optimization-2026-11-10.md`（闭环 / 参考提取 / 反 AI 味）

---

## 0. 结论

上一轮的 P0（截图闭环、参考提取、复用优先、`design_diff`）已经落地。本轮的增量非常集中：

**通用组件生成的瓶颈不在「画得像」，在「复用」。** 先有可检索的组件清单和 token 体系，生成才从「凭空创作」变成「检索 → 组装 → 绑变量 → 验证」。

---

## 1. 方案地图（6 类）

### A. 组件库检索式生成（RAG）

| 项目 | 做法 |
|---|---|
| [`tongsalangsingha/AI-prototyper-tool`](https://github.com/tongsalangsingha/AI-prototyper-tool) | 固定 `my_components.js`（32 个组件）→ prompt 分解成 feature → RAG 检索组件 → LLM 只能用检索到的组件输出 JSON 组件树 → 沙箱脚本渲染成 Figma 原生节点 |
| [`nickciolpan/Rapid-Insert`](https://github.com/nickciolpan/Rapid-Insert) | 自然语言搜索设计系统组件 → 预览 → 组合（克隆现有组件 + 生成缺失元素）→ 插入 |

**可借鉴**：给通用组件建一个**可检索的目录**，生成前必须先检索、再组装，禁止模型凭空造组件。

### B. 设计系统感知 + 变量绑定

| 项目 | 做法 |
|---|---|
| [`figma/mcp-server-guide`](https://github.com/figma/mcp-server-guide) | `figma-generateLibraries`：所有视觉属性绑 variables；token 分 primitives / semantic / component 三层；每个变量带 scopes 与 codeSyntax |
| [`miapre/mimic-ai`](https://github.com/miapre/mimic-ai) | 发现文件里的设计系统 → 匹配真实组件 / token → 用真实实例 + 变量绑定构建；DS gap 检测 |
| [`robindicapua/figma-component-compose`](https://github.com/robindicapua/figma-component-compose) | 从代码组件提取 API → 生成 Figma 变体集（属性名与代码一致）→ 全部绑 DS 变量 |

**可借鉴**：生成前建/复用 token，组件全部绑变量；scopes 与 codeSyntax 是让 agent 正确引用 token 的元数据。

### C. 变体矩阵治理

| 来源 | 规则 |
|---|---|
| [`figma/mcp-server-guide`](https://github.com/figma/mcp-server-guide) → `component-creation.md` | 变体轴乘积 **≤30**；超限就拆（按主风格拆 component set / INSTANCE_SWAP 承担 icon 轴 / 抽 Building Blocks）；变体集网格排版 + 确定性命名 |

**可借鉴**：通用组件库按「轴」设计，主动控制组合爆炸。

### D. 语义工具层 / MCP 桥

| 项目 | 做法 |
|---|---|
| [`Liljimbo/Figma-Forge`](https://github.com/Liljimbo/Figma-Forge) | 把 Plugin API 封装成 26 个语义工具（`create_button` / `create_card` / `create_form`…），AI 只说「做什么」 |
| [`musepy/genable`](https://github.com/musepy/genable) | 设计 agent 循环：计划 → 建原生结构 → 读回 → 截图验证 → 继续精修 |
| [`mrpeterperez/tellfigma`](https://github.com/mrpeterperez/tellfigma) | MCP 双向读写：读项目 Tailwind/CSS/组件 → 用**你的** token 在 Figma 里生成 |

**可借鉴**：把常见组件写成「配方」（按什么顺序调哪些工具 + `batch` 一次写入），比让模型自由发挥稳。

### E. 从真实代码 / HTML 反推

| 项目 | 做法 |
|---|---|
| [`miapre/mimic-ai`](https://github.com/miapre/mimic-ai) | 从 HTML 构建，只用你的设计系统，安全回退 |
| [`minha-eveningglow/design-something`](https://github.com/minha-eveningglow/design-something) | Figma / Penpot 抽象层，统一成 W3C DTCG token |
| [`e-cesar9/prompt-to-figma`](https://github.com/e-cesar9/prompt-to-figma) | 从文本描述生成完整设计系统 |

**可借鉴**：让生成结果复用项目已有 token 与组件（figwright 已有 `token_map` / `component_map` / `icon_map`）。

### F. 文件结构即上下文（官方）

| 来源 | 要点 |
|---|---|
| [Figma 官方 structure-figma-file](https://developers.figma.com/docs/figma-mcp-server/structure-figma-file/) | 组件 + Code Connect + variables + 语义命名 + auto layout + annotations + dev resources |

**可借鉴**：Code Connect 是让 agent 复用真实组件最有效的手段；annotations 解释非视觉信息。

---

## 2. AI-prototyper 的 `my_components.js` 结构（目录模板）

已抓取到本地 `_work/my_components.js`（404 行，32 个组件）。

**结构**：

```js
export const myComponents = [
  {
    name: "Button",
    description: "For user actions like submit, save, cancel.",
    props: [
      { name: "label", type: "string", required: true },
      { name: "color", type: "string", options: ["primary","secondary","destructive"] },
      { name: "icon", type: "string" },
      { name: "width", type: "string", options: ["FULL","HUG"], default: "FULL" }
    ]
  },
  // ...共 32 个
]
```

**两个值得抄的设计**：

1. **description 是「意图式」写法**——如 Button = *"For user actions like submit, save, cancel"*，Link Text = *"...secondary navigation (e.g., Forgot password?, Sign up)"*。这是 RAG 能检索准的关键。
2. **prop 带枚举约束**——`color[primary|secondary|destructive]`、`width[FULL|HUG]`。模型只能选，不能自由发挥。

伴生文件：`my_icons.js`（可用图标名）+ `my_icons_svg.js`（SVG path）。

---

## 3. 现有插件的 4 个缺口

| 文件 | 已有 | 缺 |
|---|---|---|
| `reuse-first.md` | 盘点**文件内**已有组件 → 映射 → 复用 | 没有「通用组件目录」可检索 |
| `variants-states.md` | 状态变体、4 个属性上限 | 30 组合上限、拆分策略、INSTANCE_SWAP 承担 icon 轴 |
| `tokens-variables.md` | 三层令牌、命名、绑定 | `scopes`（禁止 ALL_SCOPES）、`codeSyntax` 的 `var()` 包装 |
| `structure.md` | 命名、变体、描述、分区 | Code Connect / annotations / dev resources |

---

## 4. 关键来源清单

- https://github.com/tongsalangsingha/AI-prototyper-tool
- https://github.com/nickciolpan/Rapid-Insert
- https://github.com/figma/mcp-server-guide
- https://github.com/miapre/mimic-ai
- https://github.com/robindicapua/figma-component-compose
- https://github.com/Liljimbo/Figma-Forge
- https://github.com/musepy/genable
- https://github.com/mrpeterperez/tellfigma
- https://developers.figma.com/docs/figma-mcp-server/structure-figma-file/
- https://github.com/minha-eveningglow/design-something
- https://github.com/e-cesar9/prompt-to-figma
