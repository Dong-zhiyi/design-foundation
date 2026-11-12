# Taro

> 来源：[Taro 官方文档 - 设计稿及尺寸单位](https://docs.taro.zone/docs/size)

**一句话**：用 React 写一套代码，编译到小程序 / H5 / React Native。京东开源。

---

## 一、设计稿基准

| 项目 | 数值 |
|---|---|
| **默认基准** | **750px**（官方推荐） |
| 支持 | 750 / 640 / 828（需配置） |
| 自定义 | 支持任意（如 375），需配置 `deviceRatio` |
| **单位** | 源码写 **px**，编译时自动转换 |

### 换算规则
```
设计稿 1px  →  源码写 1px  →  小程序 rpx / H5 rem / RN pt
```

**默认 deviceRatio**：
```js
const DEVICE_RATIO = {
  640: 2.34 / 2,
  750: 1,
  828: 1.81 / 2
}
```

### 配置示例（750 基准）
```js
const config = {
  designWidth: 750,
  deviceRatio: { 750: 1 }
}
```

### 如果是 375 基准
```js
const DEVICE_RATIO = {
  640: 2.34 / 2,
  750: 1,
  828: 1.81 / 2,
  375: 2 / 1    // ← 需要自己加
}
```

---

## 二、单位转换规则

| 目标端 | 源码单位 | 编译后 |
|---|---|---|
| 小程序 | `px` | **rpx** |
| H5 | `px` | **rem** |
| React Native | `px` | **pt** |
| 小程序 | `rpx` | 原样保留 |
| H5 | `rpx` | rem |
| H5 | `rem` | 不转换 |
| RN | `rpx` | ❌ 不支持 |

**不想被转换的写法**：
```css
.avatar { width: 1Px; }   /* 大写 P，忽略转换 */
```

**运行时转换**：
```js
Taro.pxTransform(50)  // 无需指定单位
```

---

## 三、H5 端的坑（重要）

### 坑 1：baseFontSize 默认 20，容易和 Tailwind 冲突
Taro H5 默认 `baseFontSize: 20`，`rootValue = 40`；而 `weapp-tailwindcss` 默认 `1rem = 32px`（`baseFontSize: 16`）。

**同时用 Taro + Tailwind 时会导致 H5 与小程序尺寸差 25%。**

**解法**：统一 `baseFontSize`。
```js
h5: {
  postcss: {
    pxtransform: {
      config: {
        baseFontSize: 16,
        minRootSize: 16,
        maxRootSize: 40
      }
    }
  }
}
```

### 坑 2：minRootSize 会钳制小屏
`minRootSize` 默认 20，如果 `baseFontSize` 设成 16，375px 屏会被钳到 20px，反而偏大。
→ 设 `baseFontSize: 16` 时，`minRootSize` 也要设 16。

---

## 四、平台差异

| 平台 | 设计要点 |
|---|---|
| **小程序** | 有系统导航 + 胶囊，受小程序规范约束 |
| **H5** | 无系统导航，需自己画；基准 375px |
| **React Native** | 用 Flex；只支持类选择器；不支持组合器（`>`、`~`、`+`） |
| **鸿蒙** | 同 App |

**RN 端样式限制**（设计时要考虑）：
- 只支持 Flex 布局
- 只支持类选择器（`.btn`），不支持 `.btn .icon`、`div > span`
- 不支持 `position: fixed`（部分场景）

---

## 五、常用 UI 库

| 库 | 说明 |
|---|---|
| **Taro UI** | 官方（维护较少） |
| **NutUI React Taro** | 京东，推荐 |
| **Ant Design Mobile** | 蚂蚁，可适配 |
| **Taroify** | 轻量 |

**设计时要考虑**：和 uni-app 一样，**贴合项目使用的 UI 库规范**。

---

## 六、设计交付要点

- **基准说清**：750（默认）还是 375？
- **说清转换规则**：源码 px → 各端单位
- **H5 端配置**：`baseFontSize` 是多少？
- **RN 端限制**：是否要支持 RN？（影响样式方案）
- **说清哪些不需要转换**（大写 Px 的部分）

---

## 检查清单

- [ ] 设计稿基准已确认（推荐 750px）
- [ ] `designWidth` 配置已确认
- [ ] H5 端 `baseFontSize` 已确认
- [ ] 是否支持 RN 已确认（影响样式方案）
- [ ] 与项目 UI 库规范对齐
- [ ] 标注里写清源码单位（px）和目标单位
