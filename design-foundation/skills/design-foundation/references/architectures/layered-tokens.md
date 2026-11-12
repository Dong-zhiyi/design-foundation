# 分层令牌架构（Layered Tokens）

**这是所有项目的地基。** 不管选什么上层架构，令牌分层都要先做。

> 来源：DTCG 格式标准（`design-tokens/community-group`）、`tokens-studio/figma-plugin`、`style-dictionary/style-dictionary`、`primer/primitives`

---

## 为什么分层

不分层时：改一个主色，要全局搜 `#8C1D18` 替换——漏一个就是 bug。

分层后：改一个 primitive token，所有引用它的 semantic / component token 自动更新。

---

## 三层结构

### 第 1 层：Primitive（原始值）

**是什么**：设计系统里**最原始的值**，没有语义。

```
color/red/600    = #8C1D18
color/gold/500   = #C9A227
color/neutral/50 = #FAF7F2
space/4          = 16px
radius/md        = 12px
font/size/lg     = 18px
```

**要点**：
- 只描述「是什么值」，不描述「用在哪」
- 命名按**色相/刻度**，不按用途
- **不要在任何组件里直接引用 primitive**（除了 Foundation 层）

### 第 2 层：Semantic（语义值）

**是什么**：给 primitive **赋予用途**。

```
color/brand/primary   → {color/red/600}
color/brand/accent    → {color/gold/500}
color/bg/canvas       → {color/neutral/50}
color/bg/surface      → #FFFFFF
color/text/primary    → {color/neutral/900}
color/text/secondary  → {color/neutral/600}
```

**要点**：
- **所有组件只引用 semantic 层**
- 换肤/深色模式**只改这一层**
- 命名格式：`<类型>/<领域>/<角色>`

### 第 3 层：Component（组件值）

**是什么**：**某个具体组件**的专属值。

```
button/primary/bg        → {color/brand/primary}
button/primary/text      → #FFFFFF
button/primary/radius    → {radius/md}
input/bg                 → {color/bg/surface}
input/border             → {color/border/default}
```

**要点**：
- 只在该组件**与 semantic 层不一致时**才建
- 命名：`<组件>/<状态>/<属性>`

---

## 完整示例

```json
{
  "color": {
    "red": {
      "600": { "$value": "#8C1D18", "$type": "color" }
    },
    "gold": {
      "500": { "$value": "#C9A227", "$type": "color" }
    }
  },
  "brand": {
    "primary": { "$value": "{color.red.600}", "$type": "color" },
    "accent":  { "$value": "{color.gold.500}", "$type": "color" }
  },
  "bg": {
    "canvas":  { "$value": "#FAF7F2", "$type": "color" },
    "surface": { "$value": "#FFFFFF", "$type": "color" }
  },
  "text": {
    "primary":   { "$value": "#1A1A1A", "$type": "color" },
    "secondary": { "$value": "#6B6B6B", "$type": "color" }
  }
}
```

> 以上是 DTCG 格式（`$value` / `$type`），Tokens Studio 和 Style Dictionary 都支持。

---

## 什么进令牌，什么不进

| 进令牌 | 不进令牌 |
|---|---|
| 颜色（所有） | 具体像素坐标 |
| 字号 / 行高 / 字重 | 单个组件的布局 |
| 间距（4 的倍数档位） | 临时遮罩 |
| 圆角 | 一次性尺寸 |
| 阴影 | 动画关键帧细节 |
| 动效时长 / 缓动 | — |
| 层级（z-index） | — |

**判断标准**：**会被复用、或会整体调整的值**才进令牌。

---

## 与 Figma 的对应

| 令牌层 | Figma 里对应 |
|---|---|
| Primitive | 变量集合里的原始变量 |
| Semantic | 引用 primitive 的变量（alias） |
| Component | 组件专属变量 |

**Figma 落地见** `../figma-delivery/tokens-variables.md`。

---

## 检查清单

- [ ] 三层齐全（primitive / semantic / component）
- [ ] 组件只引用 semantic，不直接引用 primitive
- [ ] 命名语义化（不是 `gray-500`，而是 `color/text/secondary`）
- [ ] 深色模式只改 semantic 层
- [ ] 令牌有 DTCG 格式（`$value` / `$type`）
- [ ] 与设计工具双向同步（Tokens Studio）
