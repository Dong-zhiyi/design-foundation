# design-foundation（设计基座）

> ZCode Plugin · Product Design Foundation Workflow

[![Version](https://img.shields.io/badge/version-0.4.0-blue)](../CHANGELOG.md)
[![License](https://img.shields.io/badge/license-MIT-green)](../LICENSE)

把 `design-foundation` 技能打包成的 ZCode 插件：在动手画设计稿之前，先定平台与技术栈，再定风格、架构、字体库与图标库，把需求聊清楚、过一遍质量门禁，确认 Figma MCP 连通性和目标文件，最后落地。

**自带 Figma 操作能力**：插件内含 `figwright-mcp` 与 `framelink-mcp` 两个 MCP，装完即可画，不依赖全局 MCP 配置。

**生成不是终点**：落地后必须走「截图自检闭环」——截图 → 独立裁判打分 → 修 → 再截，≥8/10 且硬门禁全过才收工。

> Server names carry `-mcp` suffix to avoid conflicts with any global `figwright` / `Framelink-Figma` in `config.json`.

## Structure

```text
design-foundation/
├── .zcode-plugin/plugin.json      # Plugin manifest (name / version / skills / mcpServers / userConfig)
├── README.md
└── skills/design-foundation/
    ├── SKILL.md                   # 11-step workflow + 2 checkpoints + trigger boundaries
    └── references/                # Knowledge base (platforms / tech stacks / styles / architectures / fonts / icons / quality / Figma delivery)
        └── figma-delivery/        # Naming / structure / variables / delivery / reuse-first / variants / reference extraction / verification loop
```

> The font preview page is **not included in this plugin** — it's a separate static project (`index.html` + `fonts/`), deployed independently. See the [font preview project](https://github.com/Dong-zhiyi/zcode-design-foundation-font-preview) for details.

## Install

1. In ZCode: **Plugin Marketplace → Add → Add Marketplace**, paste the repository root (the directory containing `marketplace.json`)
2. Under **Personal**, find this plugin and click **Install**

## Configure Figma Token

`framelink-mcp` requires a Figma Personal Access Token, **filled in by each user**:

1. **Settings → Plugins → Installed → Design Foundation 设计基座 → Advanced**
2. Fill in `Figma Access Token` (starts with `figd_`)
3. Start a new task to reload MCP

Generate token: Figma → Settings → Security → Personal access tokens

> `figwright-mcp` doesn't need a token, but requires **Figma desktop app running with the Figwright plugin active**.
>
> ⚠️ **Security note**: ZCode currently has no secure credential storage. `userConfig` fields with `sensitive: true` can't be filled in the UI, so this uses a plain string field. The token is stored in plaintext under `~/.zcode/cli/config.json` in `plugins.options` — same exposure level as the previous global `mcp.servers` approach.

## Prerequisites

- **Node.js / npm**: Both MCPs are launched via `npx` and will auto-download on first run (`@figwright/mcp@0.7.0`, `figma-developer-mcp`)
- **Figma Desktop + Figwright Plugin**: `figwright-mcp` connects via local plugin protocol; browser version won't work

## Font Preview

The font preview page is a **separate static project** — not bundled with this plugin. Users deploy it to any static server and configure the URL in `font_preview_url`.

- Project: [zcode-design-foundation-font-preview](https://github.com/Dong-zhiyi/zcode-design-foundation-font-preview) (`index.html` + `fonts/`, ~62MB)
- Must be served over http(s): browsers block `file://` pages from loading local fonts (including same-directory fonts)
- For local preview: `python -m http.server 8731` in the project directory

## Update

After modifying source, increment `version` in `.zcode-plugin/plugin.json`, sync `../marketplace.json`, then **refresh the marketplace** in ZCode → **Personal** → **Update**.

## Contributing

See the [repository README](../README.md) for contribution guidelines.

## License

MIT — see [LICENSE](../LICENSE)
