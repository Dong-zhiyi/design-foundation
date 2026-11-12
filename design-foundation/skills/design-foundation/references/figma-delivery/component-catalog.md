# 通用组件目录

**生成前先查这里。** 页面里每个区块，先从本目录找标准组件，再查文件内已有组件，都没有才新建。

> 来源：`tongsalangsingha/AI-prototyper-tool`（`my_components.js`，32 个组件，RAG 检索）、`nickciolpan/Rapid-Insert`（自然语言搜索 + 组合）、`figma/mcp-server-guide`（变体 30 组合上限）、`miapre/mimic-ai`（复用真实组件 + gap 检测）

---

## 一、为什么需要这个目录

没有目录时，模型每次生成都从零画：

- 同一个按钮画 5 种不同样式
- 同一个卡片在不同页面间距/圆角不同
- 后续改一个组件，要改 20 个地方

**目录 = 约束生成的边界。** 模型的自由度从「随便画」收窄到「从清单里选 + 按 API 组装」。

---

## 二、条目结构

每个组件是一个**可检索的 API**，不是外观描述：

```yaml
name: Button                      # 组件名（对应 Figma 组件 类别/名称）
layer: atom                       # atom | molecule | organism | pattern
description: 用于用户动作（提交/保存/取消/购买）。   # 意图式，供检索命中
props:                            # 可覆写项（对应组件属性）
  - { name: label, type: string, required: true }
  - { name: color, type: enum, options: [primary, secondary, ghost, danger] }
  - { name: icon, type: icon }                      # 引用 libraries/icons.md
  - { name: width, type: enum, options: [FULL, HUG], default: FULL }
variants:                         # 变体轴（乘积必须 ≤30）
  split: 按 color 拆 4 个组件集（Primary / Secondary / Ghost / Danger）
  axes: { Size: [Large, Medium, Small], State: [Default, Hover, Pressed, Disabled, Loading] }
  combinations: 3 × 5 = 15
tokens: [color/brand/primary, color/bg/surface, color/text/primary, space/4, radius/md]
states: [Default, Disabled, Loading]
recipe: component-recipes.md#button
```

**字段说明**：

| 字段 | 说明 |
|---|---|
| `description` | **意图式**写法：用于什么场景、什么时候用 / 不用。这是 RAG 检索命中的关键 |
| `props` | 可覆写项；`enum` 类型必须给 `options`，模型只能选，不能自由发挥 |
| `variants` | 变体轴；**所有轴乘积 ≤30**，超限按 `split` 拆（见 `variants-states.md` 第六节） |
| `tokens` | 依赖的变量（见 `tokens-variables.md`）；生成时必须绑定，不写死值 |
| `states` | 必须存在的状态（至少 default / disabled / loading） |

---

## 三、检索规则

1. **拆区块**：把页面拆成区块（导航 / 搜索 / 列表 / 卡片 / 表单 / 空态…）
2. **查目录**：每个区块在本目录找匹配组件 → 命中则用它的 API
3. **查文件**：未命中 → `get_local_components` + `component_map` 查文件内已有组件
4. **新建**：都没有 → 新建，并记入「组件缺口报告」

**检索示例**：

| 用户说 | 命中组件 |
|---|---|
| 「做个登录页」 | Top App Bar / Text Field ×2 / Button / Link Text / Social Button |
| 「做个商品列表」 | Search Bar / FilterBar(待补) / List Item ×N / Tag / EmptyState |
| 「做个设置页」 | Top App Bar / List Item ×N / Switch / Divider / Chevron |

---

## 四、组件清单

### 原子（Atom）

```yaml
name: Button
layer: atom
description: 用于用户动作（提交/保存/取消/购买）。
props:
  - { name: label, type: string, required: true }
  - { name: color, type: enum, options: [primary, secondary, ghost, danger] }
  - { name: icon, type: icon }
  - { name: width, type: enum, options: [FULL, HUG], default: FULL }
variants:
  split: 按 color 拆 4 个组件集（Primary / Secondary / Ghost / Danger）
  axes: { Size: [Large, Medium, Small], State: [Default, Hover, Pressed, Disabled, Loading] }
  combinations: 3 × 5 = 15
tokens: [color/brand/primary, color/bg/surface, color/text/primary, space/4, radius/md]
states: [Default, Disabled, Loading]
```

```yaml
name: Text Field
layer: atom
description: 用于用户输入文字（姓名/邮箱/密码/搜索词），含 label 与输入框。
props:
  - { name: label, type: string }
  - { name: placeholder, type: string }
  - { name: icon, type: icon }
  - { name: endIcon, type: icon }
  - { name: value, type: string }
variants:
  axes: { State: [Default, Focus, Error, Disabled] }
  combinations: 4
tokens: [color/text/secondary, color/border/default, color/feedback/error, space/4, radius/md]
states: [Default, Focus, Error, Disabled]
```

```yaml
name: Search Bar
layer: molecule
description: 顶部搜索，内容筛选与发现。
props:
  - { name: placeholder, type: string }
  - { name: icon, type: icon }
  - { name: hasFilter, type: boolean, default: false }
variants:
  axes: { State: [Default, Focus] }
  combinations: 2
tokens: [color/bg/surface, color/text/secondary, space/4, radius/full]
states: [Default, Focus]
```

```yaml
name: Dropdown
layer: atom
description: 下拉选择，用于表单里从预定义选项中选一个。
props:
  - { name: label, type: string }
  - { name: placeholder, type: string }
  - { name: selectedValue, type: string }
  - { name: endIcon, type: icon }
variants:
  axes: { State: [Default, Focus, Disabled] }
  combinations: 3
tokens: [color/text/secondary, color/border/default, space/4, radius/md]
states: [Default, Focus, Disabled]
```

```yaml
name: Checkbox
layer: atom
description: 用于多选或同意条款。
props:
  - { name: label, type: string, required: true }
  - { name: checked, type: boolean, default: false }
variants:
  axes: { State: [Default, Checked, Disabled], Size: [Small, Medium] }
  combinations: 6
tokens: [color/brand/primary, color/border/default, color/text/primary, space/3]
states: [Checked, Disabled]
```

```yaml
name: RadioButton
layer: atom
description: 用于从一组选项里选一个。
props:
  - { name: label, type: string, required: true }
  - { name: checked, type: boolean, default: false }
  - { name: groupName, type: string, required: true }
variants:
  axes: { State: [Default, Checked, Disabled], Size: [Small, Medium] }
  combinations: 6
tokens: [color/brand/primary, color/border/default, color/text/primary, space/3]
states: [Checked, Disabled]
```

```yaml
name: Switch
layer: atom
description: 用于开关型设置项。
props:
  - { name: label, type: string, required: true }
  - { name: checked, type: boolean, default: false }
variants:
  axes: { State: [On, Off, Disabled] }
  combinations: 3
tokens: [color/brand/primary, color/bg/surface, color/text/primary]
states: [On, Off, Disabled]
```

```yaml
name: Icon
layer: atom
description: 独立图标，引用 libraries/icons.md 的图标库。
props:
  - { name: icon, type: icon, required: true }
  - { name: size, type: enum, options: [xs, sm, md, lg], default: md }
variants:
  axes: { Size: [xs, sm, md, lg] }
  combinations: 4
tokens: [color/text/primary]
states: [Default]
```

```yaml
name: Image
layer: atom
description: 图片，用于照片/商品图/封面。
props:
  - { name: width, type: number }
  - { name: height, type: number }
  - { name: cornerRadius, type: enum, options: [none, sm, md, lg, full], default: md }
  - { name: fit, type: enum, options: [FILL, FIT, CROP], default: FILL }
variants:
  axes: { Shape: [circle, square], Size: [sm, md, lg] }
  combinations: 6
tokens: [radius/md, radius/full]
states: [Default]
```

```yaml
name: Placeholder
layer: atom
description: 灰色占位块，表示图片/媒体尚未加载。
props:
  - { name: width, type: number }
  - { name: height, type: number }
  - { name: label, type: string }
variants:
  axes: { Size: [sm, md, lg] }
  combinations: 3
tokens: [color/bg/surface]
states: [Default]
```

```yaml
name: Avatar
layer: atom
description: 圆形头像，显示首字母。
props:
  - { name: initials, type: string }
  - { name: size, type: enum, options: [sm, md, lg], default: md }
  - { name: color, type: enum, options: [blue, green, orange, purple, red, gray] }
variants:
  axes: { Size: [sm, md, lg], Color: [blue, green, orange, purple, red, gray] }
  combinations: 18
tokens: [color/bg/surface, color/text/primary]
states: [Default]
```

```yaml
name: Badge
layer: atom
description: 未读数/状态点，通常叠在图标右上角。
props:
  - { name: count, type: string }
  - { name: color, type: enum, options: [red, blue, green, orange], default: red }
variants:
  axes: { Color: [red, blue, green, orange], Size: [sm, md] }
  combinations: 8
tokens: [color/feedback/error, color/text/primary, space/2]
states: [Default]
```

```yaml
name: Tag
layer: atom
description: 分类/状态标签，常用于卡片、文章头、商品列表。
props:
  - { name: label, type: string, required: true }
  - { name: color, type: enum, options: [blue, green, red, orange, purple, gray] }
  - { name: size, type: enum, options: [small, medium], default: medium }
variants:
  axes: { Color: [blue, green, red, orange, purple, gray], Size: [small, medium] }
  combinations: 12
tokens: [color/bg/surface, color/text/secondary, space/3, radius/full]
states: [Default]
```

```yaml
name: Chip
layer: atom
description: 筛选 pill / 标签组 / 可切换选项。
props:
  - { name: label, type: string, required: true }
  - { name: icon, type: icon }
  - { name: selected, type: boolean, default: false }
  - { name: variant, type: enum, options: [filled, outlined], default: filled }
variants:
  axes: { Variant: [filled, outlined], State: [Default, Selected, Disabled] }
  combinations: 6
tokens: [color/brand/primary, color/bg/surface, color/text/primary, space/3, radius/full]
states: [Selected, Disabled]
```

```yaml
name: Divider
layer: atom
description: 分割线，分隔区块或列表项。
props:
  - { name: spacing, type: enum, options: [sm, md, lg], default: md }
  - { name: inset, type: number, default: 0 }
variants:
  axes: { Orientation: [horizontal, vertical] }
  combinations: 2
tokens: [color/border/default, space/4]
states: [Default]
```

```yaml
name: Spinner
layer: atom
description: 加载指示器，异步操作与不确定加载态。
props:
  - { name: size, type: enum, options: [sm, md, lg], default: md }
  - { name: color, type: enum, options: [primary, white, gray], default: primary }
variants:
  axes: { Size: [sm, md, lg], Color: [primary, white, gray] }
  combinations: 9
tokens: [color/brand/primary]
states: [Default]
```

```yaml
name: Progress Bar
layer: atom
description: 线性进度，用于上传/步骤/等级。
props:
  - { name: value, type: number, required: true }
  - { name: label, type: string }
  - { name: color, type: enum, options: [primary, success, warning, error], default: primary }
variants:
  axes: { Color: [primary, success, warning, error], Size: [sm, md] }
  combinations: 8
tokens: [color/brand/primary, color/bg/surface]
states: [Default]
```

```yaml
name: Header
layer: atom
description: 屏幕标题或分区标题。
props:
  - { name: text, type: string, required: true }
  - { name: textAlign, type: enum, options: [LEFT, CENTER, RIGHT], default: CENTER }
variants:
  axes: { Align: [LEFT, CENTER, RIGHT] }
  combinations: 3
tokens: [font/size/xl, font/weight/bold, color/text/primary]
states: [Default]
```

```yaml
name: Body Text
layer: atom
description: 段落/说明/副文本，用于可读内容块。
props:
  - { name: text, type: string, required: true }
  - { name: size, type: enum, options: [small, medium, large], default: medium }
  - { name: color, type: enum, options: [primary, secondary, disabled], default: primary }
  - { name: textAlign, type: enum, options: [LEFT, CENTER, RIGHT], default: LEFT }
variants:
  axes: { Size: [small, medium, large], Color: [primary, secondary, disabled], Align: [LEFT, CENTER, RIGHT] }
  combinations: 27
tokens: [font/size/md, color/text/primary, color/text/secondary]
states: [Default]
```

```yaml
name: Label
layer: atom
description: 非交互文字显示。
props:
  - { name: text, type: string, required: true }
  - { name: textAlign, type: enum, options: [LEFT, CENTER, RIGHT], default: LEFT }
variants:
  axes: { Align: [LEFT, CENTER, RIGHT] }
  combinations: 3
tokens: [font/size/sm, color/text/secondary]
states: [Default]
```

### 容器（Container）

```yaml
name: Row
layer: atom
description: 横向排列容器，用于并排控件/条目。
props:
  - { name: spacing, type: enum, options: [sm, md, lg], default: md }
  - { name: alignment, type: enum, options: [TOP, CENTER, BOTTOM], default: CENTER }
  - { name: justify, type: enum, options: [MIN, CENTER, MAX, SPACE_BETWEEN], default: MIN }
variants:
  axes: { Alignment: [TOP, CENTER, BOTTOM], Justify: [MIN, CENTER, MAX, SPACE_BETWEEN] }
  combinations: 12
tokens: [space/4]
states: [Default]
```

```yaml
name: Column
layer: atom
description: 纵向排列容器，用于卡片内分组。
props:
  - { name: spacing, type: enum, options: [sm, md, lg], default: md }
  - { name: alignment, type: enum, options: [MIN, CENTER, MAX], default: MIN }
variants:
  axes: { Alignment: [MIN, CENTER, MAX] }
  combinations: 3
tokens: [space/4]
states: [Default]
```

```yaml
name: Card
layer: molecule
description: 分组相关内容的容器（padding + 背景 + 边框 + 阴影）。
props:
  - { name: padding, type: enum, options: [sm, md, lg], default: md }
  - { name: spacing, type: enum, options: [sm, md, lg], default: md }
  - { name: backgroundColor, type: enum, options: [white, light, dark], default: white }
  - { name: hasBorder, type: boolean, default: true }
  - { name: hasShadow, type: boolean, default: true }
variants:
  axes: { Background: [white, light, dark], Shadow: [with, without] }
  combinations: 6
tokens: [color/bg/surface, color/border/default, shadow/sm, space/4, radius/lg]
states: [Default, Hover, Pressed]
```

```yaml
name: List Item
layer: molecule
description: 列表行：主文字 + 可选副文字 + 可选前导图标 + 可选尾部元素。
props:
  - { name: primaryText, type: string, required: true }
  - { name: secondaryText, type: string }
  - { name: leadingIcon, type: icon }
  - { name: trailingIcon, type: icon }
  - { name: trailingText, type: string }
  - { name: hasDivider, type: boolean, default: true }
variants:
  axes: { Size: [sm, md], Divider: [with, without] }
  combinations: 4
tokens: [color/text/primary, color/text/secondary, color/border/default, space/4]
states: [Default, Selected, Disabled]
```

### 导航 / 浮层（Organism）

```yaml
name: Navigation Bar
layer: organism
description: 底部主导航，3–5 个 tab，每项含图标 + 文字。
props:
  - { name: items, type: string, required: true }
  - { name: activeIndex, type: number, default: 0 }
variants:
  axes: { Items: [3, 4, 5] }
  combinations: 3
tokens: [color/bg/surface, color/text/secondary, color/brand/primary, space/2]
states: [Default]
```

```yaml
name: Top App Bar
layer: organism
description: 顶部标题栏：标题 + 左侧返回/菜单 + 右侧动作图标。
props:
  - { name: title, type: string, required: true }
  - { name: navigationIcon, type: icon }
  - { name: actionIcon1, type: icon }
  - { name: actionIcon2, type: icon }
variants:
  axes: { Back: [with, without], Actions: [0, 1, 2] }
  combinations: 6
tokens: [color/bg/surface, color/text/primary, space/4]
states: [Default]
```

```yaml
name: Snackbar
layer: organism
description: 底部反馈条，可带动作按钮（确认/错误/撤销）。
props:
  - { name: message, type: string, required: true }
  - { name: actionLabel, type: string }
  - { name: variant, type: enum, options: [default, success, error, warning], default: default }
variants:
  axes: { Variant: [default, success, error, warning] }
  combinations: 4
tokens: [color/text/primary, color/feedback/error, color/feedback/success, space/4, radius/md]
states: [Default]
```

### 模式（Pattern）

```yaml
name: EmptyState
layer: pattern
description: 空态：第一次进入 / 无数据时看到的内容（插画 + 说明 + 动作）。
props:
  - { name: title, type: string, required: true }
  - { name: description, type: string }
  - { name: actionLabel, type: string }
  - { name: layout, type: enum, options: [illustration, icon], default: illustration }
variants:
  axes: { Layout: [illustration, icon] }
  combinations: 2
tokens: [color/text/primary, color/text/secondary, color/brand/primary, space/6]
states: [Default]
```

```yaml
name: Skeleton
layer: pattern
description: 骨架屏，加载态占位（优先于转圈）。
props:
  - { name: rows, type: number, default: 3 }
  - { name: size, type: enum, options: [sm, md, lg], default: md }
variants:
  axes: { Size: [sm, md, lg] }
  combinations: 3
tokens: [color/bg/surface, radius/md]
states: [Default]
```

---

## 五、待补清单（实测后按需补全）

以下组件在 AI-prototyper 的 32 个清单里，但优先级低于上面 27 个，先列出来，实测有需要再补：

- **输入类**：Social Button、ColorSwatch、Slider
- **文字类**：Link Text
- **浮层类**：Modal、Toast、Tooltip
- **模式类**：FormField、FilterBar、PageHeader
- **导航类**：TabBar（小程序 / H5）、SideBar
- **其他**：FAB

补全时按 `02-how-to-add.md` 走，保持 description 意图式、prop 带枚举、变体乘积 ≤30。

---

## 六、维护规则

1. **新增组件**：走 `02-how-to-add.md`，先写 description（意图式）和 props（带枚举），再补 variants / tokens。
2. **description 决定检索命中**：写「用于什么场景」，不写「长什么样」。
3. **变体乘积 ≤30**：超了按 `variants-states.md` 第六节拆，别硬塞。
4. **tokens 必须绑变量**：不写死 hex，见 `tokens-variables.md`。
5. **实测后补全**：本目录先覆盖高频组件，别一次写满。

---

## 检查清单

- [ ] 页面每个区块都查过本目录
- [ ] 命中的组件用了它的 API（props / 变体 / tokens）
- [ ] 未命中区块走了文件内组件映射
- [ ] 新建的组件记入了缺口报告
- [ ] 没有「同一个按钮画 5 种」
