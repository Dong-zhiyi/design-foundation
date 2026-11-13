# uni-app

> 来源：
> - [uni-app x 官方文档 - 长度单位](https://doc.dcloud.net.cn/uni-app-x/css/common/length.html)（**新一代，推荐 1x 390px**）
> - [uni-app 官方文档 - 页面样式与布局](https://uniapp.dcloud.net.cn/tutorial/syntax-css.html)（经典版，rpx 750 基准）
> - [uni-app 官方文档 - pages.json](https://uniapp.dcloud.net.cn/collocation/pages.html)（导航栏 44px / tabBar 50px）
> - [DCloud 问答：设计稿基准](https://ask.dcloud.net.cn/question/221454)

**一句话**：用 Vue 写一套代码，编译到 iOS / Android / 鸿蒙 / H5 / 各平台小程序。国内跨端项目最常用。

---

## ⚠️ 先分清：你用的是哪个 uni-app？

| 版本 | 设计稿基准 | 推荐单位 | 说明 |
|---|---|---|---|
| **uni-app x**（新一代） | **390px（1x）** | **px** | 官方当前推荐 |
| **uni-app（经典版，Vue 2/3）** | 750px（rpx 1:1）或 375px | rpx | 老项目常见 |

**这是最容易搞错的地方。** 问用户一句：「用的是 uni-app x 还是经典 uni-app？」

---

## 一、uni-app x（新一代，官方推荐）

### 1.1 设计稿基准

| 项目 | 数值 |
|---|---|
| **画布** | **1x**（逻辑像素，不是 2x 物理像素） |
| **竖屏基准** | **390px** |
| 宽屏 | 834px 或 1280px |
| **推荐单位** | **px** |

> 官方原文：「推荐设计师在设计软件中使用 1x 画布，按 390px 出普通竖屏图、按 834px 或 1280px 出宽屏图。」

### 1.2 单位使用规则（重要）

| 场景 | 用什么 | 说明 |
|---|---|---|
| 字号 | **px** | 系统默认 16px，各端一致 |
| 高度 | **px** | 固定值 |
| 间距 / 内边距 / 外边距 | **px** | 固定值 |
| 圆角 / 边框 / 阴影 | **px** | 固定值 |
| **需要随屏宽变化的宽度** | **rpx** | 仅此场景 |
| 占满剩余空间 | **`flex: 1`** | 不要靠单位 |

**官方明确不推荐**：
- ❌ 在 font-size / height / padding / margin / 边框 / 阴影上大量使用 rpx
- ❌ 把 rpx 当成「万能自适应」单位
- ❌ 期待「所有屏幕都按设计稿等比放大」

### 1.3 设计稿要标什么

1. **弹性容器**：哪块区域在屏幕变宽后向右填充（用 `flex: 1`）
2. **限宽小组件**：如按钮，直接标 px（设计 60px 就写 `width: 60px`）
3. **字体规范**：固定几个字号档位（大/较大/小/较小），不要给无限种字号
4. **间距、圆角规范**：固定枚举值，全站统一

---

## 二、经典 uni-app（Vue 2/3）

### 2.1 设计稿基准

| 项目 | 数值 |
|---|---|
| 常用基准 | **750px**（此时 1px = 1rpx，最省心） |
| 也可用 | 375px、640px |
| 单位 | **rpx**（响应式像素） |
| **换算公式** | `750 × 元素在设计稿中的宽度 / 设计稿基准宽度` |

### 2.2 换算示例

| 设计稿基准 | 元素宽度 | rpx 值 |
|---|---|---|
| 750px | 100px | `750×100/750` = **100rpx** |
| 375px | 200px | `750×200/375` = **400rpx** |
| 640px | 100px | `750×100/640` = **117rpx** |

> 官方 Tips：「设计师可以用 iPhone6 作为视觉稿的标准」（iPhone 6 逻辑宽 375pt）

### 2.3 rpx 的边界

| 配置项 | 默认值 | 说明 |
|---|---|---|
| `rpxCalcMaxDeviceWidth` | **960** | 超过此宽度按此基准计算 |
| `rpxCalcBaseDeviceWidth` | **375** | 超出最大宽度时的基准 |
| `dynamicRpx` | false | 动态 rpx（nvue） |

**行为**：
- 屏宽 ≤ 960px：rpx 按实际屏宽等比缩放
- 屏宽 > 960px：按 375px 基准计算（防止平板/PC 上元素过大）

**配置示例**（`pages.json`）：
```json
{
  "globalStyle": {
    "rpxCalcMaxDeviceWidth": 1024,
    "rpxCalcBaseDeviceWidth": 375,
    "rpxCalcIncludeWidth": 750
  }
}
```

---

## 三、关键尺寸（两个版本都一样）

| 组件 | 高度 | 说明 |
|---|---|---|
| **NavigationBar（导航栏）** | **44px** | **不含状态栏**，固定不可改 |
| **TabBar（底部选项卡）** | **50px** | **不含安全区**（HBuilderX 2.3.4 起与 H5 统一，可自主更改） |

> ⚠️ **和小程序完全不同**：原生小程序导航是 **128rpx**、TabBar **98rpx**。uni-app 的导航和 TabBar **小得多**。

**其他**：
- `pages.json` 里 `titleNView` / plus api 的单位**只支持 px**，不支持 rpx
- CSS 变量 `--window-top` / `--window-bottom` 可拿到导航和 tabBar 高度

---

## 四、支持的单位

| 单位 | 支持情况 |
|---|---|
| **px** | ✅ 逻辑像素（uni-app x 推荐） |
| **rpx** | ✅ 响应式（经典方案） |
| rem / vh / vw | ✅ 仅 vue 页面（nvue 不支持） |
| 百分比 | ❌ nvue 不支持 |
| 纯数字 | App 端视为 px；Web 端无效 |

---

## 五、平台差异

| 平台 | 设计要点 |
|---|---|
| **微信小程序** | 有系统导航 + 胶囊（编译后仍受小程序规范约束） |
| **App（iOS/Android）** | 导航栏 44px、TabBar 50px；注意 Safe Area |
| **H5** | 无系统导航，需自己画；**基准 390px** |
| **鸿蒙** | 同 App |

---

## 六、常用 UI 库

| 库 | 说明 |
|---|---|
| **uView UI** | 最流行，组件全 |
| **uni-ui** | DCloud 官方 |
| **NutUI** | 京东，Vue3 |
| **Wot Design Uni** | 轻量、现代 |

**设计时要考虑**：如果项目用了某个 UI 库，设计稿要**贴合该库的组件规范**。

---

## 六.5、原生组件与 API 清单

> 来源：
> - [uni-app 组件文档](https://uniapp.dcloud.net.cn/component/)
> - [uni-app x 组件文档](https://doc.dcloud.net.cn/uni-app-x/component/)
> - [uni-app API 文档](https://uniapp.dcloud.net.cn/api/)
> - [uni-app x 概述](https://doc.dcloud.net.cn/uni-app-x/)

设计时要知道框架**自带哪些原生组件**，避免设计了实现不了的效果，也避免重复造轮子。

### 经典 uni-app 原生组件

**视图容器**
| 组件 | 说明 |
|---|---|
| `view` | 视图容器，类似 div |
| `scroll-view` | 可滚动视图容器 |
| `swiper` | 滑块视图容器（轮播 banner） |
| `match-media` | 屏幕动态适配 |
| `movable-area` | 可拖动区域 |
| `movable-view` | 可移动视图（必须在 movable-area 内） |
| `cover-view` | 覆盖在原生组件上的文本 |
| `cover-image` | 覆盖在原生组件上的图片 |

**基础内容**
| 组件 | 说明 |
|---|---|
| `icon` | 图标 |
| `text` | 文字 |
| `rich-text` | 富文本 |
| `progress` | 进度条 |

**表单组件**
| 组件 | 说明 |
|---|---|
| `button` | 按钮 |
| `checkbox` | 多项选择器 |
| `editor` | 富文本输入框 |
| `form` | 表单 |
| `input` | 输入框 |
| `label` | 标签 |
| `picker` | 弹出式列表选择器 |
| `picker-view` | 窗体内嵌式列表选择器 |
| `radio` | 单项选择器 |
| `slider` | 滑动选择器 |
| `switch` | 开关选择器 |
| `textarea` | 多行文本输入框 |

**导航**
| 组件 | 说明 |
|---|---|
| `navigator` | 页面链接，类似 a 标签 |

**媒体组件**
| 组件 | 说明 |
|---|---|
| `audio` | 音频 |
| `camera` | 相机 |
| `image` | 图片 |
| `video` | 视频 |
| `live-player` | 直播播放 |
| `live-pusher` | 直播推流 |

**其他**
| 组件 | 说明 |
|---|---|
| `map` | 地图 |
| `canvas` | 画布 |
| `web-view` | 内嵌网页 |
| `ad` / `ad-draw` | 广告组件 |
| `custom-tab-bar` | 自定义底部 TabBar |
| `navigation-bar` | 页面顶部导航 |
| `page-meta` | 页面属性配置 |
| `unicloud-db` | uniCloud 数据库组件 |

### uni-app x 原生组件（新增/差异）

uni-app x 在经典版基础上**新增**了以下组件：

| 组件 | 说明 | 备注 |
|---|---|---|
| `glass-effect-view` | 玻璃效果视图容器 | **新增**，毛玻璃背景 |
| `list-view` | 列表容器 | **新增**，高性能长列表 |
| `waterflow` | 瀑布流容器 | **新增** |
| `nested-scroll` | 嵌套滚动 | **新增** |
| `sticky` | 粘性布局组件 | **新增**，吸顶效果 |
| `page-container` | 弹层容器 | **新增** |
| `share-element` | 共享元素视图容器 | **新增**，转场动画 |
| `movable` | 可拖动组件 | 替代 classic 的 movable-area + movable-view |
| `cover` | 覆盖组件 | 替代 classic 的 cover-view/cover-image |

**uni-app x 移除/不支持**：
- ❌ 不支持小程序 WXML 组件（微信小程序除外）
- ❌ 不支持 `plus` API 和 `weex` API
- ❌ nvue 不支持百分比单位

**uni-app x 特有**：
- ✅ 支持 DOM API（App 端）
- ✅ 支持 `uvue` 自定义组件（前端）和 `uts` 原生插件（native）
- ✅ Steam Mode 渲染引擎（官方称快于原生）

### 原生 API 分类（经典 uni-app 与 uni-app x 通用）

| 分类 | 主要 API |
|---|---|
| **基础** | `uni.base64ToArrayBuffer`、`uni.getLaunchOptionsSync`、`canIUse` |
| **网络** | `uni.request`、`uni.uploadFile`、`uni.downloadFile`、`uni.connectSocket` |
| **路由** | `uni.navigateTo`、`uni.redirectTo`、`uni.reLaunch`、`uni.switchTab`、`uni.navigateBack` |
| **数据缓存** | `uni.getStorage`、`uni.setStorage`、`uni.removeStorage`、`uni.clearStorage` |
| **位置** | `uni.getLocation`、`uni.chooseLocation`、`uni.openLocation` |
| **媒体** | `uni.chooseImage`、`uni.previewImage`、`uni.chooseVideo`、`uni.chooseMedia`、`uni.getRecorderManager` |
| **设备** | `uni.getSystemInfo`、`uni.getNetworkType`、`uni.scanCode`、`uni.setClipboardData`、`uni.vibrateShort` |
| **界面** | `uni.showToast`、`uni.showLoading`、`uni.showModal`、`uni.showActionSheet`、`uni.setNavigationBarTitle`、`uni.setTabBarBadge` |
| **文件** | `uni.saveFile`、`uni.openDocument`、`uni.getFileSystemManager` |
| **画布** | `uni.createCanvasContext`、`uni.canvasToTempFilePath` |
| **第三方服务** | `uni.login`、`uni.getUserInfo`、`uni.share`、`uni.requestPayment` |
| **其他** | `uni.authorize`、`uni.openSetting`、`uni.chooseAddress`、`uni.navigateToMiniProgram` |

> ⚠️ **设计影响**：知道框架有什么原生组件，设计时就可以**直接映射**——比如需要轮播就用 `swiper`，需要长列表就用 `list-view`（x）或 `scroll-view`（经典），需要吸顶就用 `sticky`（x）或自己实现（经典）。

---

## 七、设计交付要点

- **说清是 uni-app x 还是经典版**
- **说清基准**：390px（x）还是 750px（经典）
- **说清单位**：px 还是 rpx
- **导航栏 44px（不含状态栏）、TabBar 50px（不含安全区）**
- **哪些是弹性区域**（flex:1）、哪些是固定尺寸
- **与项目 UI 库规范对齐**

---

## 检查清单

- [ ] 已确认 uni-app x 还是经典版
- [ ] 设计稿基准已确认（x: 390px / 经典: 750px）
- [ ] 单位已确认（x: px 为主 / 经典: rpx）
- [ ] 弹性区域已标注
- [ ] 导航栏 44px、TabBar 50px 已确认
- [ ] 与项目 UI 库规范对齐
- [ ] H5 端基准已确认（390px）
- [ ] Safe Area（App 端）已考虑
