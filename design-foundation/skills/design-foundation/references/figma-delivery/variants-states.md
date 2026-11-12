# 变体与状态补全

**交互组件只画 default 态 = 交付不完整。** 用变体（Variants）组状态，不要手画 4 份。

> 来源：Figma 官方 MCP 最佳实践（components + variants）、`madebysan/claude-figma-skills`（组件文档 + 变体意图）、`naming.md` 第六节

---

## 一、什么时候必须建变体

| 组件 | 最低变体 |
|---|---|
| 按钮 | Default / Pressed / Disabled / Loading |
| 输入框 | Default / Focus / Error / Disabled |
| 标签页 | Selected / Unselected |
| 卡片 | Default / Hover / Pressed |
| 列表项 | Default / Selected / Disabled |
| 开关 | On / Off |
| 图标按钮 | Default / Disabled |

**原则**：**用户能遇到的每个状态都要有。** 不是「画一个好看的 default」。

---

## 二、用工具组变体

### 1. 建各个状态的组件

每个状态先建成独立的 COMPONENT（命名用变体语法）：

```
Button/Type=Primary,Size=Large,State=Default
Button/Type=Primary,Size=Large,State=Disabled
```

### 2. 合成变体集

```
combine_as_variants(nodeIds: [各个状态的组件])
```

Figma 会按名字里的 `属性=值` 自动推导变体轴。

### 3. 暴露组件属性

```
add_component_property(componentId: 变体集, name: "State", type: "VARIANT", ...)
add_component_property(componentId: 变体集, name: "显示图标", type: "BOOLEAN", ...)
add_component_property(componentId: 变体集, name: "标题", type: "TEXT", ...)
```

| 属性类型 | 用途 | 例子 |
|---|---|---|
| VARIANT | 切换变体 | 状态、尺寸、类型 |
| TEXT | 可编辑文字 | 按钮文字、卡片标题、价格 |
| BOOLEAN | 显示/隐藏 | 是否显示标签、是否显示图标 |
| INSTANCE_SWAP | 可替换 | 图标、头像 |

**绑定到图层**：

```
bind_component_property(nodeId: 子图层, field: "characters", propertyId: ...)   // TEXT → 文字
bind_component_property(nodeId: 子图层, field: "visible", propertyId: ...)      // BOOLEAN → 可见性
bind_component_property(nodeId: 子图层, field: "mainComponent", propertyId: ...) // INSTANCE_SWAP
```

**要点**：变体属性**不超过 4 个**（多了开发看不懂）。

---

## 三、页面里怎么使用

**用实例，不要进组件内部改**：

```
create_instance(componentId: 变体集)
set_instance_properties(instanceId: ..., properties: {
  "State": "Disabled",
  "标题": "免费试听一节课",
  "显示图标": true
})
```

---

## 四、状态设计规范

### 每个状态要有明确差异

| 状态 | 差异 |
|---|---|
| Default | 正常 |
| Hover | 背景加深/变浅、光标 pointer |
| Pressed | 背景再深、轻微缩放 |
| Disabled | 降透明度/灰化、不可点 |
| Loading | 转圈或骨架、文字保留或隐藏 |
| Focus | 焦点环（不要只用浏览器默认 outline） |
| Error | 红边 + 错误提示文字 |

### 深色模式

**不是简单反色**：
- 深色下阴影不可见 → 改用描边或色差
- 降低饱和度（纯白刺眼）
- 用 Mode 实现，不是复制一套（见 `tokens-variables.md`）

---

## 五、空态 / 加载态 / 错误态

**每个页面都要有**：

| 状态 | 画什么 |
|---|---|
| 空态 | 第一次进来看到什么（插画 + 一句说明 + 一个动作） |
| 加载态 | 骨架屏优先（不要只有一个转圈） |
| 错误态 | 说清哪里错了 + 怎么办（重试按钮） |
| 极限态 | 超长文字、100 条数据、图片加载失败 |

**骨架屏 > 转圈**：骨架屏让用户知道「要出什么」，转圈只让用户等。

---

## 六、变体矩阵治理

**所有变体轴的乘积 ≤30。** 超了 Figma 会变慢、开发看不懂、维护爆炸。

### 1. 先算乘积

```
Button: Size(3) × Type(4) × State(5) = 60   → 超限，必须拆
Button: Size(3) × State(5) = 15             → 可以
```

### 2. 超限就拆（按顺序选）

1. **按主风格拆 component set**：`PrimaryButton` / `SecondaryButton` / `GhostButton` / `DangerButton`，每个 3 × 5 = 15
2. **用 INSTANCE_SWAP 承担 icon 轴**：图标不参与变体矩阵，改成可替换属性
3. **抽 Building Blocks**：带独立状态轴的子元素抽成子组件集（命名前缀 `Building Blocks/` 或 `.Building Blocks/`）

### 3. 网格排版

- **列 = State**（Default / Hover / Pressed / Disabled）
- **行 = Size × Style**（Size 变化最快）
- gap 20，padding 40
- 排完 `resizeWithoutConstraints` 让组件集框贴合内容

### 4. 确定性命名

```
Button/Type=Primary,Size=Large,State=Default
Button/Type=Primary,Size=Large,State=Disabled
```

命名规则让清理和续跑能按名找节点，**不要用 `Component 1` 这种默认名**。

### 5. 验证

`get_metadata` 读组件集，确认：

- `variantGroupProperties` 轴数和值正确
- 子组件数 = 预期组合数
- 没有子组件叫 `Component 1`

> 来源：`figma/mcp-server-guide` → `component-creation.md`

---

## 检查清单

- [ ] 交互组件有完整状态变体（至少 default/disabled/loading）
- [ ] 变体用 `combine_as_variants` 组集（不是复制组件）
- [ ] 变体轴乘积 ≤30（超限已拆分）
- [ ] 变体集网格排版、确定性命名
- [ ] 暴露了 TEXT / BOOLEAN / INSTANCE_SWAP 属性
- [ ] 页面里用实例 + 覆写，没进组件内部改
- [ ] 每个页面有 empty / loading / error 态
- [ ] 深色模式用 Mode 实现
