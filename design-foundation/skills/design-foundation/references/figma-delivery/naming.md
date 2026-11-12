# Figma 命名规范

**命名乱 = 开发找不到图层 = 还原靠猜。** 这套规范让每个图层都能被准确描述。

---

## 一、页面命名

```
01_Foundation     基础：色板、字阶、间距、组件
02_Components     组件库（可复用）
03_Patterns       模式/模板
04_Pages          具体页面
05_Archive        归档（旧版本、废弃稿）
```

**要点**：
- 数字前缀保证排序
- 用下划线分隔，不用空格
- `05_Archive` 定期清理，别让废稿占视野

---

## 二、画板（Frame）命名

```
<页面名>/<平台>

例：
首页/小程序
详情页/iOS
列表页/Web
```

**要点**：
- 多平台时，平台名放后面
- 同一页面多状态：`首页/小程序/深色`、`首页/小程序/空态`

---

## 三、组件命名

```
<类别>/<名称>[/<变体>]

例：
ShowCard/图兰朵
Button/Primary
Button/Secondary
Tab/首页
Icon/search
Tag/Hot
```

**类别参考**：
- `Button` / `Input` / `Select` / `Tag` / `Badge`
- `Card` / `List` / `Grid`
- `Nav` / `Tab` / `Header` / `Footer` / `Sidebar`
- `Modal` / `Sheet` / `Toast` / `Tooltip`
- `Icon` / `Image` / `Avatar`
- `Text` / `Title` / `Heading`

**变体用 Figma 的 Variants 管理**，不要建 `按钮-大`、`按钮-中`、`按钮-小` 三个组件。

---

## 四、图层命名

```
<元素类型>/<语义名>

例：
Icon/search
Image/Poster
Text/Title
Text/Meta
Bg/Card
Bg/Overlay
```

**元素类型前缀**：
| 前缀 | 用途 |
|---|---|
| `Icon/` | 图标 |
| `Image/` | 图片 |
| `Text/` | 文本 |
| `Bg/` | 背景 |
| `Bg/` + `Overlay/` | 遮罩 |
| `Line/` | 分割线 |
| `Avatar/` | 头像 |
| `Thumb/` | 缩略图 |

**禁止**：
- ❌ `Rectangle 123`（Figma 默认名）
- ❌ `Group 45`
- ❌ `Frame 7`
- ❌ `矩形 1`
- ❌ `组 2`

---

## 五、变量与样式命名

### 变量（Variables）
```
<类型>/<类别>/<角色>

例：
color/brand/primary
color/bg/canvas
color/text/secondary
space/4
radius/base
font/size/lg
```

### 样式（Styles）
```
<类型>/<名称>/<状态>

例：
Text/Heading/LG
Text/Body/MD
Text/Caption
Fill/Brand/Primary
Effect/Card/Shadow
```

---

## 六、变体和属性

**用 Variants 而不是复制组件**：

```
Button
├── Property: Type = Primary / Secondary / Ghost
├── Property: Size = Large / Medium / Small
├── Property: State = Default / Hover / Disabled / Loading
└── Property: Icon = With / Without
```

**要点**：
- 变体属性不超过 4 个（多了开发看不懂）
- 状态用 Figma 的 Variant，不要用单独组件
- 组件内图层命名保持一致（这样覆写才能对应）

---

## 七、实例覆写（Instance Overrides）

**覆写要规范**：

| ✅ 可以覆写 | ❌ 不要覆写 |
|---|---|
| 文字内容 | 删除组件内图层 |
| 图标替换（同尺寸） | 改变图层结构 |
| 显示/隐藏（用 Variants 或布尔属性） | 改组件的自动布局参数 |
| 颜色（如果暴露了变量） | 改组件内元素的命名 |

**要点**：覆写应该「换内容」，不是「改结构」。

---

## 八、自动布局（Auto Layout）规范

- **一律用自动布局**，不要绝对定位（除非是装饰元素）
- 间距值用 4 的倍数
- 明确设置 **Sizing**：
  - `Hug` — 内容撑开（按钮、标签）
  - `Fill` — 填满父容器（列表项、输入框）
  - `Fixed` — 固定尺寸（图标、头像）
- 用 `Space between` 时确保行为符合预期

---

## 九、检查清单

- [ ] 页面按 `01_` ~ `05_` 分区
- [ ] 画板命名含平台
- [ ] 组件命名 `类别/名称`
- [ ] 没有 `Rectangle 123` 这种默认名
- [ ] 变体用 Variants 管理（不是复制组件）
- [ ] 变量命名语义化
- [ ] 自动布局参数规范（Sizing 正确）
- [ ] 实例覆写只换内容不改结构
