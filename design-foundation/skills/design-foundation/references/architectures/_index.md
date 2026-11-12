# 设计架构库索引

「架构」在这里指**设计系统怎么分层、怎么命名、怎么组织**，不是画图的技巧。

---

## 选型决策树

```
项目规模？
├─ 单产品 / 小团队
│    ├─ 只做一两个页面 ─────────────→ 分层令牌（够用就好）
│    └─ 有 5+ 页面，要长期维护 ─────→ 分层令牌 + 组件库
├─ 中大型产品 / 多页面
│    ├─ 组件复用度高 ───────────────→ 原子设计
│    └─ 业务领域复杂 ───────────────→ 领域分层
└─ 多产品线 / 多团队
     └─────────────────────────────→ 单包 vs 多包 决策 + 完整设计系统
```

---

## 四个架构

**⚠️ 必须列全让用户选**，不要只推荐 1 个。用户的判断你不该替他做。

| # | 架构 | 一句话 | 适合 | 详见 |
|---|---|---|---|---|
| 1 | **分层令牌** | primitive → semantic → component，三层 token | 所有项目的**底座**，必选 | [layered-tokens.md](./layered-tokens.md) |
| 2 | **原子设计** | atom → molecule → organism → template → page | 组件复用度高的产品 | [atomic-design.md](./atomic-design.md) |
| 3 | **领域分层** | Foundation / Patterns / Features | 业务领域复杂的产品 | [domain-layered.md](./domain-layered.md) |
| 4 | **单包 vs 多包** | 一个仓库还是多个包 | 多团队协作时 | [package-strategy.md](./package-strategy.md) |

> **1 是地基，2/3/4 是上层选择，可以叠加。** 可以附一句推荐（如「你的项目建议 1+2」），但**必须列全**。

---

## 分层模型（通用）

不管选哪个架构，分层逻辑都长这样：

```
Foundation（基础层）
  色彩 / 字体 / 间距 / 圆角 / 阴影 / 动效
        ↓
Tokens（令牌层）
  primitive → semantic → component
        ↓
Primitives（原语层）
  按钮 / 输入框 / 标签 / 图标 / 选择器
        ↓
Components（组件层）
  卡片 / 导航 / 列表项 / 表单
        ↓
Patterns（模式层）
  列表页 / 详情页 / 表单流 / 空状态
        ↓
Pages（页面层）
  具体页面
```

**依赖只能向下，不能反向。** 这是架构设计的唯一硬性规则。

---

## 命名规范（跨架构通用）

### 令牌命名
```
<类型>/<类别>/<属性>

例：
color/brand/primary
color/bg/canvas
color/text/secondary
space/4
radius/base
font/size/lg
```

**要点**：
- 用 `/` 分层，不用 `-` 连写
- **语义化命名**（`color/bg/canvas`），不要外观命名（`color/gray-500`）
- 代码侧映射：`color/brand/primary` ↔ `--color-brand-primary`

### 组件命名
```
<类别>/<名称>

例：
ShowCard/图兰朵
Button/Primary
Tab/首页
Icon/search
```

### 图层命名
```
<元素类型>/<语义名>

例：
Icon/search
Text/Title
Image/Poster
```

---

## 数据来源

| 仓库 | 用途 | Star |
|---|---|---|
| [`design-tokens/community-group`](https://github.com/design-tokens/community-group) | DTCG 格式标准（W3C 社区组） | 2.2k |
| [`tokens-studio/figma-plugin`](https://github.com/tokens-studio/figma-plugin) | Figma 令牌管理 | 1.6k |
| [`style-dictionary/style-dictionary`](https://github.com/style-dictionary/style-dictionary) | 令牌 → 多端代码 | 4.7k |
| [`carbon-design-system/carbon`](https://github.com/carbon-design-system/carbon) | 架构教科书级参考 | 9.5k |
| [`primer/primitives`](https://github.com/primer/primitives) | GitHub 的令牌分层实践 | 415 |
| [`arco-design/arco-design`](https://github.com/arco-design/arco-design) | 字节，含 Figma 库 | 5.7k |

---

## 常见错误

| 错误 | 后果 | 正确做法 |
|---|---|---|
| 令牌直接写死 hex | 换肤要改几十个文件 | 用语义化 token |
| 分层只有两层（primitive + semantic） | 组件级覆写困难 | 加 component 层 |
| 架构过度设计 | 小项目背着大架构 | 从分层令牌起步 |
| 命名按外观（`gray-500`） | 换主题时语义错乱 | 按语义（`color/text/secondary`） |
| 依赖反向（基础层引用组件） | 循环依赖，改一处崩一片 | 单向依赖 |
