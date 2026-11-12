# Figma 变量与令牌映射

**目标**：设计稿里的变量名和代码里的 token 名**一一对应**，开发不用猜。

> 来源：DTCG 格式（`design-tokens/community-group`）、`tokens-studio/figma-plugin`、`style-dictionary/style-dictionary`

---

## 一、Figma 变量命名

### 颜色
```
color/<类别>/<角色>

例：
color/brand/primary
color/brand/accent
color/bg/canvas
color/bg/surface
color/bg/surface/hover
color/text/primary
color/text/secondary
color/text/disabled
color/border/default
color/border/strong
color/feedback/success
color/feedback/warning
color/feedback/error
```

### 间距
```
space/<档位>

例：
space/1 = 4px
space/2 = 8px
space/3 = 12px
space/4 = 16px
space/6 = 24px
space/8 = 32px
```

### 圆角
```
radius/<档位>

例：
radius/sm = 4px
radius/md = 8px
radius/lg = 12px
radius/xl = 16px
radius/full = 9999px
```

### 字号
```
font/size/<档位>
font/weight/<角色>
font/lineHeight/<档位>

例：
font/size/sm = 12px
font/size/md = 14px
font/size/lg = 16px
font/size/xl = 20px
font/weight/regular = 400
font/weight/medium = 500
font/weight/bold = 700
```

### 阴影
```
shadow/<层级>

例：
shadow/sm
shadow/md
shadow/lg
```

---

## 二、与代码 token 的映射

| Figma 变量 | CSS 变量 | 说明 |
|---|---|---|
| `color/brand/primary` | `--color-brand-primary` | 直接对应 |
| `color/bg/canvas` | `--color-bg-canvas` | |
| `space/4` | `--space-4` | |
| `radius/md` | `--radius-md` | |
| `font/size/lg` | `--font-size-lg` | |

**命名规则**：Figma 用 `/`，代码用 `-`。一一对应，不要搞例外。

**导出**：用 Tokens Studio 插件可以直接导出 DTCG JSON，再用 Style Dictionary 转成 CSS/SCSS/Swift/Kotlin。

---

## 三、变量集合（Collections）

**按主题/模式组织**：

```
Design Tokens（默认集合）
├── 所有颜色变量
├── 所有间距变量
├── 所有字体变量
└── 所有圆角变量

Theme-Dark（可选，或用 Mode 实现）
└── 深色模式下的不同值
```

**推荐做法**：**一个集合 + 多个 Mode**（Light / Dark），而不是建多个集合。

```
集合：Design Tokens
├── Mode 1: Light
└── Mode 2: Dark
```

---

## 四、三层令牌在 Figma 里的实现

### Primitive（原始值）
```
color/red/600   = #8C1D18
color/gold/500  = #C9A227
color/neutral/50 = #FAF7F2
```

### Semantic（语义值）—— 引用 primitive
```
color/brand/primary = alias(color/red/600)
color/bg/canvas     = alias(color/neutral/50)
```

### Component（组件值）—— 引用 semantic
```
button/primary/bg = alias(color/brand/primary)
input/bg          = alias(color/bg/surface)
```

**关键**：组件只绑定 semantic 层。换肤时只改 semantic，组件自动更新。

---

## 五、绑定到节点

**正确做法**：
- 填充色 → 绑定 `color/*` 变量
- 文字颜色 → 绑定 `color/text/*` 变量
- 间距/圆角 → 绑定 `space/*`、`radius/*` 变量
- 字号/字重 → 绑定 `font/*` 变量

**错误做法**：
- ❌ 直接写 hex
- ❌ 用 Style 而不是 Variable（Style 不能换肤）
- ❌ 只在部分元素上绑定（有的绑有的不绑）

---

## 六、深色模式

**做法**：同一批变量，在 Dark Mode 下取不同的值。

```
color/bg/canvas:
  Light Mode: #FAF7F2
  Dark Mode:  #0D0D0F

color/text/primary:
  Light Mode: #1A1A1A
  Dark Mode:  #E8E8EC
```

**注意**：
- 深色模式**不是简单反色**
- 深色下阴影不可见 → 改用描边或色差
- 深色下降低饱和度（纯白刺眼）

---

## 七、从设计到代码的完整链路

```
Figma 变量
    ↓ (Tokens Studio 插件导出)
DTCG JSON
    ↓ (Style Dictionary)
CSS 变量 / SCSS / JS / Swift / Kotlin
    ↓
开发使用
```

**要点**：
- 一份源头（DTCG JSON），多端产物
- 设计改 token → 导出 → 代码更新
- 不要设计和代码各维护一套

---

## 检查清单

- [ ] 变量命名与代码 token 一一对应
- [ ] 三层结构（primitive / semantic / component）
- [ ] 组件只绑定 semantic 层
- [ ] 深色模式用 Mode 实现（不是复制集合）
- [ ] 没有写死的 hex
- [ ] 有导出流程（Tokens Studio → DTCG JSON）
- [ ] 开发知道怎么取用
