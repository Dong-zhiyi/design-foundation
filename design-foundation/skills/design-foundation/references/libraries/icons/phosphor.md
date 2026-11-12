# Phosphor Icons

| 项目 | 内容 |
|---|---|
| 许可 | MIT |
| 商用 | ✅ 免费可商用 |
| 数量 | 9000+ |
| 风格 | 统一家族、几何感 |
| 权重 | **6 种**（thin / light / regular / bold / fill / duotone） |
| 字体图标 | ✅ `@phosphor-icons/web` |
| Figma 插件 | ✅ 有 |
| 仓库 | `phosphor-icons/core` |

**安装**：`npm i @phosphor-icons/react`（或 `-web` / `-vue` / `-flutter`）

**特点**
- 一套图标 6 种权重，能覆盖从优雅标题到清晰操作的多种语境
- duotone（双色调）变体很有表现力
- 权重统一很重要，混用会制造意外的层级

**适合**：需要多种粗细、品牌感强、剧院/高端调性

**来源**：https://phosphoricons.com

---

## 按需下载（图标会持续增删时看这里）

`@phosphor-icons/web` 是**全量包**：6 个权重合计约 **3 MB**；单权重全量 woff2 约 130–160 KB、ttf 约 440–555 KB。图标用得少时应该做**子集化**。

### 方案 A：Phosphor Pack（官方，一次性定稿适用）

**https://pack.phosphoricons.com/** —— Phosphor 官方的字体裁剪工具，四步：

1. **选图标**——搜索 + 勾选，可按权重筛选（Regular / Thin / Light / Bold / Fill / Duotone）
2. **配置字体**——格式选 `woff2` / `ttf` / `woff`；输出选 `CSS and external font(s)`
3. **Review**——看 glyph 列表、估算体积与节省比例
4. **Generate**——下载

> ⚠️ **它不保存选择**（纯前端工具，无 localStorage / URL 参数 / 导出导入）。**下次再加图标得重新勾选**。适合「一次性定稿、后续基本不动」。

### 方案 B：iconfont.cn 项目（增量增删最省心，推荐）

1. 在 **iconfont.cn** 建项目（如「剧院售票」）
2. 去 **phosphoricons.com** 搜图标 → 选权重 → **SVG RAW** 下载
3. 上传到 iconfont.cn 项目
4. 生成字体 → 下载 woff2/ttf + CSS
5. **下次加图标**：往同一项目再加一个 SVG → 重新生成 → 下载

> 服务端持久化，随时增删，还能协作。**国内小程序的标准工作流。**

### 方案 C：本地清单 + 构建脚本（有工程化时最自动化）

- 项目里维护一份图标名清单（如 `icons.txt`，一行一个名字）
- 用 `@phosphor-icons/core` 取原始 SVG，或用 fonttools 对 `@phosphor-icons/web` 的 TTF 做子集化：

```bash
pyftsubset Phosphor.woff2 --text-file=icons.txt \
  --output-file=phosphor-subset.woff2 --flavor=woff2
```

> 加图标 = 改清单 + 跑脚本。

### 网络字体 vs 包内字体（小程序）

把字体文件放服务器、用网络地址加载**可以**，但要满足 4 个硬条件：

| # | 条件 | 说明 |
|---|---|---|
| 1 | **必须 https** | iOS 不支持 http |
| 2 | **域名配到小程序后台** | `downloadFile 合法域名`（必须），建议也加 `request 合法域名` |
| 3 | **服务器开 CORS** | 响应头必须 `Access-Control-Allow-Origin: *`（只给 `servicewechat.com` 安卓会挂） |
| 4 | **Content-Type 正确** | `font/woff` / `font/ttf` / `font/woff2`（配成 text/html 会解析失败） |

nginx 示例：

```nginx
location ~ \.(eot|otf|ttf|woff|woff2|svg)$ {
    add_header Access-Control-Allow-Origin *;
}
```

推荐用 `wx.loadFontFace` / `uni.loadFontFace`（有失败回调便于排查）：

```js
wx.loadFontFace({
  global: true,
  family: 'Phosphor',
  source: 'url("https://your.domain/fonts/phosphor-regular.woff2")',
  scopes: ['webview'],
  success() {}, fail(err) { console.warn(err) }
})
```

> ⚠️ **WOFF2 在低版本 iOS 不兼容**，官方建议 TTF / WOFF。**开发者工具成功 ≠ 真机成功**，以真机为准。
> 💡 **折中做法**：包内放**子集字体**（几十 KB，主包 2MB 限制内），稳定且零延迟；完整字体放服务器反而没必要。
