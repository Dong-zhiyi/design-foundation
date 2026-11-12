# Figma 文件与组件结构

**文件组织决定协作效率。** 结构乱的文件，多人协作会变成灾难。

---

## 一、页面分区

```
01_Foundation
├── 色板（所有颜色，用变量）
├── 字阶（所有文字样式）
├── 间距与圆角
└── 阴影与层级

02_Components
├── Atoms（原子组件）
├── Molecules（分子组件）
├── Organisms（有机体）
└── Patterns（模式）

03_Templates
└── 页面骨架

04_Pages
├── 首页/小程序
├── 列表页/小程序
└── 详情页/小程序

05_Archive
└── 旧版本、废弃稿
```

---

## 二、组件库组织

### 按层级
```
Components/
├── Atoms/
│   ├── Button
│   ├── Input
│   ├── Tag
│   └── Icon
├── Molecules/
│   ├── SearchBar
│   ├── FormField
│   └── ListItem
├── Organisms/
│   ├── NavBar
│   ├── ShowCard
│   ├── FilterBar
│   └── TabBar
└── Patterns/
    ├── PageHeader
    ├── EmptyState
    └── LoadingSkeleton
```

### 按业务（如果是多业务）
```
Components/
├── Common/         跨业务通用
├── Business/      业务专属
├── Shop/           商城专属
└── Membership/     会员专属
```

---

## 三、组件内部结构

**一个好的组件，图层结构应该能自解释**：

```
ShowCard（组件根）
├── ├── Bg/Card（背景）
├── ├── Poster（海报）
│   ├── Image/Poster
│   └── Text/PosterTitle
└── ├── Info（信息列）
    ├── Text/Title
    ├── Tags
    │   ├── Tag/类别
    │   └── Tag/Hot
    ├── Text/Meta
    └── Bottom
        ├── Text/Price
        └── Button/Buy
```

**要点**：
- 根节点是 **Frame + 自动布局**
- 分组清晰（背景、内容、操作）
- 命名用 `类别/名称`

---

## 四、变体（Variants）管理

**原则**：**同一组件的不同状态/尺寸/类型，用 Variants 而不是复制。**

```
Button（组件集）
├── Type: Primary | Secondary | Ghost | Danger
├── Size: Large | Medium | Small
├── State: Default | Hover | Active | Disabled | Loading
└── Icon: With | Without
```

**命名规范**：
```
组件名/变体属性=值

例：
Button/Primary/Large/Default/NoIcon
```

**注意**：变体属性**不超过 4 个**，否则组合爆炸。

---

## 五、组件描述（Description）

**给每个组件写描述**，说明：
- 用途
- 什么时候用 / 什么时候不用
- 相关组件

```
Card/内容卡
用途：展示一条内容（图片、名称、描述、价格）
用于：列表页、搜索结果、推荐位
不用于：详情页（详情页用大图布局）
相关：Card/横向版
```

**为什么重要**：协作者不用问就知道怎么用。

---

## 六、组件属性（Component Properties）

**暴露常用覆写项**，避免每次进组件内部改：

| 属性类型 | 用途 | 例子 |
|---|---|---|
| **Text** | 可编辑文字 | 标题、价格、标签 |
| **Boolean** | 显示/隐藏 | 是否显示标签、是否显示按钮 |
| **Instance Swap** | 可替换 | 图标、头像 |
| **Variant** | 切换变体 | 状态、尺寸 |

**例子**：
```
ShowCard 暴露：
- Text: 标题
- Text: 价格
- Text: 元信息
- Boolean: 显示标签
- Instance Swap: 海报
```

---

## 七、多平台文件策略

### 方案 A：一个文件多页面（推荐小团队）
```
设计文件.fig
├── 04_Pages/
│   ├── 首页/小程序
│   ├── 首页/iOS
│   └── 首页/Web
```
**优点**：组件复用方便
**缺点**：文件会变大

### 方案 B：按平台拆文件（推荐多团队）
```
设计文件-小程序.fig
设计文件-iOS.fig
设计文件-Web.fig
```
**优点**：各自独立
**缺点**：组件同步麻烦（用 Team Library 解决）

### 方案 C：组件库 + 页面文件（推荐）
```
设计系统库.fig（发布为 Team Library）
├── 01_Foundation
└── 02_Components

小程序页面.fig（引用组件库）
├── 03_Patterns
└── 04_Pages
```
**优点**：组件单点维护，多端引用
**缺点**：需要团队有版本管理意识

---

## 八、定期清理

**每月做一次**：
- 删除 `05_Archive` 里的过期稿
- 合并重复组件
- 检查未使用的样式和变量
- 更新组件描述

**指标**：
- 组件使用率（多少组件被真正用了）
- 文件体积（过大说明有冗余）

---

## 检查清单

- [ ] 页面按 `01_` ~ `05_` 分区
- [ ] 组件按层级或业务分组
- [ ] 组件内部结构清晰、命名规范
- [ ] 变体用 Variants 管理
- [ ] 每个组件有描述（用途/禁忌）
- [ ] 常用覆写项暴露为组件属性
- [ ] 多平台策略明确
- [ ] 有定期清理机制
