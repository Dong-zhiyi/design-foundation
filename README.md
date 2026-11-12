# Design Foundation 设计基座

> ZCode 插件 · 产品设计前的基座工作流

[![Version](https://img.shields.io/badge/version-0.4.0-blue)](CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-ZCode-orange)](https://zcode.app)

**一句话**：在动手画设计稿之前，先把平台、技术栈、风格、架构、字体、图标定下来，把需求聊清楚，过一遍质量门禁，确认 Figma MCP 连通性和目标文件，最后落地。

## ✨ 功能亮点

- **11 步工作流**：平台 → 技术栈 → 需求 → 风格 → 架构 → 字体 → 图标 → 质量门禁 → MCP 验证 → 落地 → 设计差异
- **截图自检闭环**：落地后自动截图 → 独立裁判打分 → 修 → 再截，≥8/10 且硬门禁全过才收工
- **自带 Figma MCP**：内含 `figwright-mcp` + `framelink-mcp`，装完即可画，不依赖全局 MCP 配置
- **知识库体系**：16 种设计风格 · 11 个图标库 · 5 个平台规范 · 7 种技术栈 · 6 种设计架构
- **复用优先**：盘点文件内已有组件 → 映射 → 复用，减少重复造轮子
- **变量令牌**：三层令牌体系（原语 / 语义 / 组件），命名规范 + 绑定规则

## 🚀 快速开始

### 安装

**方式一：插件市场安装**

1. 在 ZCode 中打开 **插件市场 → 添加 → 添加插件市场**
2. 粘贴本仓库的 URL（或克隆后粘贴本地路径）
3. 在 **个人** 里找到 **Design Foundation 设计基座** 点 **安装**

**方式二：手动安装**

```bash
# 克隆仓库
git clone https://github.com/Dong-zhiyi/design-foundation.git

# 在 ZCode 插件市场 → 添加 → 添加插件市场，选择仓库根目录
```

### 配置

安装后需要配置两项：

#### 1. Figma Access Token

`framelink-mcp` 需要 Figma Personal Access Token：

1. **设置 → 插件管理 → 已安装 → Design Foundation 设计基座 → 高级**
2. 填 `Figma Access Token`（`figd_` 开头）
3. 新开一个任务让 MCP 重新加载

生成 token：Figma → Settings → Security → Personal access tokens

> `figwright-mcp` 不需要 token，但要求 **Figma 桌面端已打开、Figwright 插件在运行**。

#### 2. 字体样张页（可选）

字体样张页是一个**独立的静态项目**，需要用户自行部署：

- 项目地址：[zcode-design-foundation-font-preview](https://github.com/Dong-zhiyi/zcode-design-foundation-font-preview)（独立仓库）
- 部署：上传到任意静态服务器，`index.html` 与 `fonts/` 必须同级
- 配置：在插件设置里填 `font_preview_url` 为部署后的地址

### 前置条件

| 条件 | 说明 |
|---|---|
| **Node.js / npm** | 两个 MCP 都用 `npx` 拉起，首次运行会自动下载 |
| **Figma 桌面端** | `figwright-mcp` 通过本地插件协议连 Figma，浏览器版不行 |
| **Figwright 插件** | 需在 Figma 桌面端运行 Figwright 插件 |

## 📖 工作流概览

```
┌─────────────────────────────────────────────────────────────┐
│                    Design Foundation 工作流                    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ① 定平台  →  ② 定技术栈  →  ③ 聊需求                       │
│       ↓                                                      │
│  ④ 选风格  →  ⑤ 定架构  →  ⑥ 选字体  →  ⑦ 选图标             │
│       ↓                                                      │
│  ⑧ 质量门禁  →  ⑨ 验证 MCP + 确认文件                        │
│       ↓                                                      │
│  ⑩ 落地设计稿  →  ⑪ 截图自检闭环（≥8/10 才收工）              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 📚 知识库结构

```
skills/design-foundation/references/
├── platforms/          # 5 个平台规范（微信/支付宝/抖音小程序、iOS、Android、Web、H5）
├── tech-stacks/        # 7 种技术栈（uni-app、Taro、React Native、Flutter、原生、H5、Web 框架）
├── styles/             # 16 种设计风格卡片（极简、新拟态、玻璃态、赛博朋克…）
├── architectures/      # 6 种设计架构（原子设计、领域分层、分层令牌…）
├── libraries/          # 字体库 + 11 个图标库文档
├── quality/            # 质量门禁（无障碍、反 AI 味、动效、UX 定律、评审门）
├── figma-delivery/     # Figma 交付规范（命名、结构、变量、复用、变体、截图闭环）
├── requirements/       # 需求简报模板
├── examples/           # 案例（剧院售票小程序等）
├── cross-platform.md   # 跨平台适配指南
├── 00-index.md         # 知识库总览
├── 01-sources.md       # 数据来源清单
└── 02-how-to-add.md    # 如何新增知识条目
```

## 🤝 贡献

欢迎提 Issue 和 PR！

- **Bug 报告**：请说明复现步骤、期望行为、实际行为
- **功能建议**：请说明使用场景和期望效果
- **知识库补充**：新增风格 / 图标库 / 平台规范，请参考 `references/02-how-to-add.md`

## 📄 许可证

MIT License — 详见 [LICENSE](LICENSE)

## 🔗 相关项目

- [zcode-design-foundation-font-preview](https://github.com/Dong-zhiyi/zcode-design-foundation-font-preview) — 字体样张页（独立静态项目）
