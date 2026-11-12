# iOS App（Apple HIG）

> 来源：Apple Human Interface Guidelines（developer.apple.com/design/human-interface-guidelines）

---

## 一、画板基准

| 设备 | 逻辑尺寸 | 倍图 |
|---|---|---|
| iPhone SE | 375×667 pt | @2x |
| iPhone 15 / 14 | 390×844 pt | @3x |
| iPhone 15 Pro Max | 430×932 pt | @3x |
| iPad（竖屏） | 820×1180 pt | @2x |

**设计稿基准建议：390×844pt**（当前主流），兼容 375 宽。

---

## 二、布局核心：Size Class，不是设备型号

Apple 明确要求：**按 Size Class 决策布局，而不是按设备型号或方向**。

| Size Class | 宽度 | 典型设备 |
|---|---|---|
| Compact | 窄 | iPhone 竖屏 |
| Regular | 宽 | iPad、iPhone 横屏（大屏机型） |

**为什么重要**：同一台 iPad 分屏时 Size Class 会变。按设备写死布局，分屏就崩。

---

## 三、关键尺寸

| 元素 | 数值 |
|---|---|
| **最小触控目标** | **44×44 pt** |
| 导航栏（Nav Bar） | 44pt 高 |
| 大标题导航栏（Large Title） | 约 96pt |
| 标签栏（Tab Bar） | 49pt 高 |
| Home Indicator 区 | 34pt 高 |
| 状态栏 | 由 Safe Area 决定（刘海/Dynamic Island 机型更高） |
| 标准边距 | 16pt（紧凑）/ 20pt（常规） |

**触控目标按位置不同**（iOS 26 HIG）：
- 顶部工具栏图标按钮：44×44
- 底部工具栏图标按钮：48×48

---

## 四、Safe Area（安全区）

**永远不要硬编码状态栏/导航栏高度**（20、44、64、88、34、49、83 这些数字都别写死），用 `safeAreaInsets` / `safeAreaLayoutGuide`。

规则：
- 内容控件 → 约束到 Safe Area
- 背景图/滚动容器 → 可以延伸到边缘（underlap），但要设 `contentInset`
- **顶部和底部 inset 相互独立**，不要互相推导
- **左右 inset 也不对称**（横屏时只有一边有刘海/Dynamic Island）

---

## 五、字体

- 系统字体：**SF Pro**（中文回退 PingFang SC）
- 正文基准：**17pt**
- 支持 **Dynamic Type**（用户可调字号），布局必须能随字号伸缩
- 字阶参考：11 / 12 / 13 / 15 / 17 / 20 / 22 / 28 / 34

---

## 六、色彩与材质

- 支持**深色模式**（必须适配）
- 使用**语义化颜色**（`.systemBackground`、`.label`、`.secondaryLabel`），不要写死 hex
- **材质（Material）**：导航栏、工具栏有半透明模糊背景，内容滚动时可见
- iOS 26 引入 **Liquid Glass**，有专门的光学参数（折射/色散/光角/磨砂强度）

---

## 七、手势

| 手势 | 行为 |
|---|---|
| 边缘右滑 | 返回上一级 |
| 底部上滑 | 回主屏幕 |
| 底部上滑悬停 | 多任务 |
| 下拉 | 通知中心 |
| 右上角下滑 | 控制中心 |

**设计约束**：不要占用边缘手势区域做自定义交互。

---

## 八、平台特有规范

- **Tab Bar**：3–5 个，超过 5 个用「更多」
- **工具栏按钮**：图标 + 可选文字
- **表单**：优先用选择器（Picker）而非键盘输入
- **搜索**：Search Bar 可放导航栏下方
- **上下文菜单**：长按触发（ContextMenu）

---

## 九、设计交付检查

- [ ] 画板 390×844pt
- [ ] 热区 ≥44×44pt
- [ ] 内容避开 Safe Area（不写死状态栏高度）
- [ ] 深色模式已适配
- [ ] 语义化颜色（不写死 hex）
- [ ] 支持 Dynamic Type
- [ ] 边缘手势区域未被占用
- [ ] Tab Bar 3–5 项
