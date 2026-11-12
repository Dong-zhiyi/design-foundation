# 原生开发（iOS / Android）

**追求最佳体验和平台特性时的选择。** 设计规范最严格，也最能体现平台气质。

---

# iOS（SwiftUI / UIKit）

> 详见 [platforms/ios-app.md](../platforms/ios-app.md)

## 设计基准

| 项目 | 数值 |
|---|---|
| 画板 | **390×844pt**（iPhone 15）或 375×812pt |
| 单位 | **pt**（逻辑点） |
| 最小触控 | **44×44 pt** |
| 导航栏 | 44pt |
| Tab Bar | 49pt |
| Home Indicator | 34pt |

## 技术栈

| 技术 | 说明 |
|---|---|
| **SwiftUI** | 苹果声明式 UI，新项目的首选 |
| **UIKit** | 传统命令式，老项目 |
| **Swift** | 语言 |

## 设计影响

- 遵循 **HIG**（Human Interface Guidelines）
- 支持 **Dynamic Type**（字号缩放）
- 支持 **深色模式**（语义化颜色）
- 毛玻璃材质（Material）
- 边缘手势（右滑返回）

---

# Android（Kotlin / Jetpack Compose）

> 详见 [platforms/android-app.md](../platforms/android-app.md)

## 设计基准

| 项目 | 数值 |
|---|---|
| 画板 | **360×800dp** 起 |
| 单位 | **dp**（尺寸）/ **sp**（字体） |
| 最小触控 | **48×48 dp** |
| Top App Bar | 64dp |
| Bottom Nav | 80dp |

## 技术栈

| 技术 | 说明 |
|---|---|
| **Jetpack Compose** | 声明式 UI，新项目首选 |
| **XML + View** | 传统方式，老项目 |
| **Kotlin** | 语言 |

## 设计影响

- 遵循 **Material Design 3**
- 支持**动态取色**（Material You）
- 支持深色模式（Tonal Palette）
- 字号用 **sp**（随系统缩放）
- 系统返回键 / 手势

---

## 原生 vs 跨端的取舍

| 维度 | 原生 | 跨端（uni-app/Taro） |
|---|---|---|
| 体验 | 最佳 | 接近原生 |
| 性能 | 最好 | 较好 |
| 开发成本 | 高（双端各一套） | 低（一套代码） |
| 平台特性 | 完整 | 受限 |
| 设计还原度 | 高 | 中 |
| 适合 | 强体验产品、平台特性依赖 | 效率优先、内容型产品 |

---

## 设计交付要点

**iOS**：
- 基准 390pt
- 遵循 HIG
- 支持 Dynamic Type
- 深色模式用语义色

**Android**：
- 基准 360dp
- 遵循 Material 3
- 字号用 sp
- 支持动态取色

**共同**：
- 明确安全区
- 明确系统导航高度
- 与平台规范对齐（不要「创新」核心交互）

---

## 检查清单

- [ ] 平台已确认（iOS / Android / 双端）
- [ ] 基准已确认（390pt / 360dp）
- [ ] 触控目标达标
- [ ] 安全区已适配
- [ ] 系统导航高度已确认
- [ ] 深色模式已适配
- [ ] 字号缩放支持（Dynamic Type / sp）
- [ ] 与平台规范对齐
