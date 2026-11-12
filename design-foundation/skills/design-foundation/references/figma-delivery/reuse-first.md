# 组件复用优先

**先盘点，再动手。** 能复用现有组件就复用实例，缺的才新建。

> 来源：Figma 官方 `/figma-generate-design`（从已批准组件库构建）、`miapre/mimic-ai`（发现设计系统 → 匹配组件/token → 用真实实例构建）、`guptaprakhariitr/aiconnect-figma-mcp`（`batch_ops` 整页一次往返）

---

## 为什么

每次生成都从零画，后果：

- 同一个按钮画 5 种不同样式
- 页面之间间距/圆角/字号各不相同
- 后续改一个按钮，要改 20 个地方
- 和「设计系统」理念完全矛盾

**设计稿的价值不在「画出来」，在「可维护」。**

---

## 三步

### 第 1 步：盘点

```
get_local_components(nodeId: 目标文件/页面)   // 列出已有组件和组件集
```

看什么：
- 有没有现成的 Button / Input / Card / Nav / Tag
- 有没有组件集（带变体）
- 命名是否规范（`类别/名称`）

### 第 1.5 步：检索通用组件目录

读 `references/figma-delivery/component-catalog.md`。把页面拆成区块（导航 / 搜索 / 列表 / 卡片 / 表单 / 空态…），逐个区块匹配目录里的通用组件。

- **命中** → 用目录的组件 API（props / 变体 / 状态 / 依赖 token）建或复用
- **未命中** → 走第 2 步 `component_map` 映射文件内已有组件
- **都没有** → 新建，并记入「组件缺口报告」

**为什么先查目录**：目录里的组件是**跨文件通用**的（Button / Card / Input…），文件内组件是**项目专属**的。先通用后专属，才能保证不同项目里同一个按钮长一样。

### 第 2 步：映射

```
component_map(nodeId: 页面根节点)   // 把页面映射到现有代码/组件
```

`component_map` 会返回：
- `mappings`：匹配到的组件 + 置信度 + matchedProps / unmatchedProps
- `unmapped`：没匹配上的

**处理**：
- 高置信度匹配 → 直接用
- 低置信度 → 看截图确认，别硬套
- `unmatchedProps` → 组件缺的变体轴，记下来，考虑扩展组件

### 第 3 步：复用 / 新建

**能复用的**：

```
create_instance(componentId: ...)            // 拉实例
set_instance_properties(instanceId: ..., properties: {...})   // 覆写
```

**覆写规则**（见 `naming.md` 第七节）：
- ✅ 换文字、换图标（同尺寸）、显示/隐藏、切变体
- ❌ 删图层、改结构、改自动布局参数

**映射不上的才新建**，新建时：
- 顺手 `create_component` 转成组件
- 交互组件用 `combine_as_variants` 组变体集
- 暴露组件属性（见 `variants-states.md`）

---

## 整页一次写入

页面结构复杂时，用 `batch` 一次原子写入，别一个节点一个节点调：

```
batch(ops: [
  { tool: "create_frame", params: {...} },
  { tool: "set_auto_layout", params: {...} },
  { tool: "create_instance", params: {...} },
  ...
])
```

**好处**：一次往返、要么全成要么全不回、省 token。

**注意**：`batch` 不能放删除类操作（`delete_*` / `ungroup_nodes` / `detach_instance`）。

---

## 组件缺口报告

每轮生成后输出，追加到 `_work/component-gaps.md`：

| 页面 | 区块 | 未命中目录 | 未命中文件组件 | 建议新增 | 理由 |
|---|---|---|---|---|---|
| 首页/小程序 | 筛选栏 | FilterBar | 无 | FilterBar（横向 Chip 组） | 列表页高频，建议入库 |

**用途**：缺口积累到一定量，就按 `02-how-to-add.md` 补进 `component-catalog.md`。

---

## 门禁

- [ ] 页面中能用实例的地方用了实例（不是手搭 Frame）
- [ ] 同一个组件全文件只有 1 个主组件（带变体）
- [ ] 新建组件都转成了 component，不是散 Frame
- [ ] 覆写只换内容，没改结构
- [ ] 页面区块对通用组件目录的命中率达标，未命中区块有说明
- [ ] 输出了「组件缺口报告」

---

## 检查清单

- [ ] 动手前 `get_local_components` 盘点过
- [ ] 查过通用组件目录（`component-catalog.md`）
- [ ] `component_map` 映射过
- [ ] 复用优先，新建次之
- [ ] 新建的转成了组件
- [ ] 整页用 `batch` 一次写入
- [ ] 输出组件缺口报告
