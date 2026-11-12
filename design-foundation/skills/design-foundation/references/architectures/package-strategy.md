# 单包 vs 多包（Monorepo 策略）

**调性**：当设计系统要服务多个产品时，**怎么切包、怎么发布、怎么版本化**。

> 来源：`carbon-design-system/carbon`（IBM，多包）、`primer/*`（GitHub，多仓库）、`arco-design/arco-design`（字节，单包 + 独立设计资产）、`microsoft/fluentui`（259 个 workspace 包）

---

## 三种形态

### 1. 单包（Single Package）

```
@company/design-system
├── tokens
├── components
└── icons
```

✅ 优点：简单、版本统一、上手快
❌ 缺点：无法按需引入，体积大；多产品共用时改动影响面大

**适合**：单一产品、小团队、设计系统初期

### 2. 多包（Multi-Package）

```
@company/ds-tokens
@company/ds-react
@company/ds-icons
@company/ds-theme
```

✅ 优点：按需引入、独立版本、多框架适配
❌ 缺点：维护成本高、版本对齐麻烦

**适合**：多产品线、需要支持多框架（React/Vue/原生）

### 3. Monorepo（单仓库多包）

```
design-system/
├── packages/
│   ├── tokens/
│   ├── react/
│   ├── vue/
│   └── icons/
└── apps/
    └── docs/
```

✅ 优点：一次改多处生效、统一版本、便于重构
❌ 缺点：需要工具链（pnpm workspace / Turborepo / Nx）

**适合**：中大型团队

---

## 设计资产怎么切

**关键原则**：**令牌和组件分离**。

```
design-system/
├── tokens/          颜色、字体、间距（与框架无关）
│   └── 输出：CSS 变量 / SCSS / JS / Swift / Kotlin
├── components/      组件实现
│   ├── react/
│   ├── vue/
│   └── miniprogram/  小程序组件
├── icons/           图标（独立，体量大）
└── theme/           主题（品牌 A / 品牌 B）
```

**为什么令牌要独立**：
- 设计与代码共用同一份源（DTCG JSON）
- 换肤时只改 theme 包
- 多端（Web / iOS / Android / 小程序）共享

---

## 版本策略

| 策略 | 说明 | 风险 |
|---|---|---|
| **统一版本** | 所有包一起发版 | 简单但容易产生无意义版本号 |
| **独立版本** | 各包独立 | 版本对齐复杂 |
| **固定版本（Locked）** | 所有包版本号强制一致 | 适合强一致要求的场景 |

**推荐**：独立版本 + 变更日志（CHANGELOG）+ 语义化版本（SemVer）。

---

## 主题与多品牌

```
tokens/
├── base/          原始值（primitive）
├── themes/
│   ├── brand-a/   品牌 A 主题（如红金）
│   ├── brand-b/   品牌 B 主题（如蓝紫）
│   └── minimal/   极简主题（黑白）
└── semantic/      语义层（按主题输出）
```

**要点**：
- primitive 层**全局唯一**
- semantic 层**按主题生成**
- 组件层**不写死颜色**，只引用 semantic

---

## 决策参考

| 场景 | 推荐 |
|---|---|
| 单一产品，1–3 人 | 单包 |
| 多产品，共享组件 | 多包 或 Monorepo |
| 需要支持多框架 | 多包（tokens 独立） |
| 多品牌 / 多主题 | tokens + theme 分层 |
| 设计系统初期 | 先单包，跑通再拆 |

---

## 反模式

- ❌ **组件里写死颜色**（换肤噩梦）
- ❌ **图标和组件耦合**（图标体量大，更新频繁）
- ❌ **令牌与组件同版本**（令牌稳定、组件频繁变）
- ❌ **没有 CHANGELOG**（使用者不知道改了什么）
- ❌ **过度拆分**（5 个组件拆成 5 个包）

---

## 检查清单

- [ ] 令牌与组件分离
- [ ] 有明确的版本策略
- [ ] 有 CHANGELOG
- [ ] 有主题机制（即使现在只有一个主题）
- [ ] 组件不写死颜色/尺寸
- [ ] 有贡献指南（怎么加新组件）
