# 跨库图标搜索与 HTML 预览

**用一个关键词，把 11 个推荐库里的候选图标全部列出来，看到形状再选。**

技术底座：Iconify API（`api.iconify.design`）。**2026-11-21 在用户机器实测**：API 可达、11 个库全覆盖、SVG 可下载。

> 配套：选完库/权重后，查 `icons.md` 的「风格 × 图标库匹配矩阵」确认与整体风格一致。

---

## 一、Iconify API 速查

### 搜索端点（跨库拿候选）

```
GET https://api.iconify.design/search?query={关键词}&limit=64
```

返回 JSON：`icons: ["lucide:ticket", "tabler:ticket", "ri:ticket-2-line", ...]` + `collections`（各库元数据）。

⚠️ **搜索索引不全**：phosphor、icon-park、fluent 对某些关键词不出现。搜索结果只当起点，**必须再用第 4 节的方法逐库补搜**。

### SVG 下载端点

```
GET https://api.iconify.design/{前缀}/{图标名}.svg?height=48&color=%23111111
```

- `height`：渲染高度 px
- `color`：填充色（URL 编码）。默认 `currentColor`（进 Figma 后用 `set_fills` 上色）

### 库元数据端点（验证覆盖、查变体）

```
GET https://api.iconify.design/collections?prefixes={前缀1},{前缀2}   ← 多库概览
GET https://api.iconify.design/collection?prefix={前缀}               ← 单库全量图标名列表
```

用途：① 确认库存在 ② 列出同一图标的所有变体（权重/线框/填充）③ 搜索索引不全时拉全量列表补搜关键词。

---

## 二、11 个库的前缀与命名规则

| 插件库 | Iconify 前缀 | 命名规则 | ticket 示例 | 实测 |
|---|---|---|---|---|
| Lucide | `lucide` | 小写 kebab | `lucide/ticket` | ✅ |
| Tabler | `tabler` | 小写 kebab，填充加 `-filled` | `tabler/ticket` | ✅ |
| **Phosphor** | **`ph`** ⚠️ | 小写+权重后缀（`-thin/-light/-bold/-fill/-duotone`，regular 省略） | `ph/ticket`、`ph/ticket-bold` | ✅ 6 权重全有 |
| Heroicons | `heroicons` | 小写 kebab | `heroicons/ticket` | ✅ |
| Bootstrap Icons | `bi` | 小写 kebab，名词可能不同 | `bi/ticket-perforated` | ✅ |
| Remix Icon | `ri` | 小写+风格后缀 `-line/-fill` | `ri/ticket-2-line` | ✅ |
| IconPark | `icon-park` | 小写 kebab | `icon-park/ticket` | ✅ |
| Octicons | `octicon` | 尺寸后缀 `-16/-24` | —— | ⚠️ 无 ticket |
| Fluent | `fluent` | 尺寸+风格后缀 `-24-regular/-24-filled` | `fluent/ticket-24-regular` | ✅ |
| Iconoir | `iconoir` | 小写 kebab | —— | ⚠️ 无 ticket |
| Radix Icons | `radix-icons` | 小写 kebab | —— | ⚠️ 集合仅 300+，缺图率高 |

### 命名三坑（必记）

1. **Phosphor 前缀是 `ph`，不是 `phosphor`**——`phosphor/ticket` 返回 Not found
2. **同一名词在各库名字不同**：ticket / ticket-2-line / ticket-perforated / ticket-24-regular。下载失败先怀疑名字，别怀疑库
3. **不是每个库都有每个图标**：octicon（200+）、radix-icons（300+）、iconoir 缺图常见。下载返回 `Not found` → 该库无此图标，跳过并在报告注明

### 调用注意

- 连续批量下载**加 `sleep 0.5`**，避免限流/断连（实测连续 9 个无间隔请求全部失败）
- API 走 Cloudflare，实测国内可达；不可达时备用域名：`api.simplesvg.com`、`api.unisvg.com`
- 本机 curl 的 `-w "%{http_code}"` 配 `$()` 有测量异常，验证存在性**以响应内容开头是否为 `<svg` 为准**

---

## 三、标准工作流：搜索 → HTML 预览 → 选定 → 落地

### 第 1 步：跨库搜索拿候选

1. `GET /search?query={关键词}&limit=64` 拿候选清单
2. 按 11 库前缀白名单过滤（`lucide tabler ph heroicons bi ri icon-park octicon fluent iconoir radix-icons`），忽略 mdi/coolicons 等未收录库
3. **补搜索引不全的库**：对 `ph`、`icon-park`、`fluent` 三个库调 `GET /collection?prefix={p}`，在返回的图标名列表里 grep 关键词（如 `icket`）

### 第 2 步：逐库点名验证 + 下载 SVG

对每个候选，用确切「前缀/图标名」下载到本地：

```bash
mkdir -p _work/icons && cd _work/icons
curl -s --max-time 15 "https://api.iconify.design/lucide/ticket.svg?height=48" -o lucide_ticket.svg
sleep 0.5
# 验证：文件开头是 <svg 即有效；是 Not found 即该库无此图标
```

同库多权重（如 Phosphor）把 2–3 个代表性权重（light/bold/fill）一起下载，方便对比。

### 第 3 步：在线预览页（每次选图标的默认方式）

**官方地址**：`https://dong-zhiyi.github.io/zcode-design-foundation-icon-preview/`（配置项 `icon_preview_url` 可覆盖为自部署副本）

用 **browser-use（内置浏览器）打开预览页，由用户在页面里选择**：

1. 打开预览页（已开着则复用标签页，不要重复开新页）
2. 在页面搜索关键词（中文英文都行：票 / ticket / 会员…），或直接用预置的 35 组跨库对比（SVG 已内嵌，零网络秒开）
3. **搜索结果卡片带 `#N` 编号**——用户回复「用 N 号」即可，也可点卡片看多尺寸大图核实形状
4. ZCode 读取编号映射拿到确切图标名：

```js
// browser-use 在预览页执行：
[...document.querySelectorAll("#searchGrid .card[data-num]")]
  .map(c => `#${c.dataset.num} = ${c.dataset.prefix}/${c.dataset.name}`)
// → ["#1 = tabler/ticket", "#2 = ri/ticket-line", ...]
```

5. 搜索是远程请求（有 localStorage 跨会话缓存 + 弱网自愈：4 路并发 + 三域名轮换 + 熔断自动恢复）；搜索失败时改用第 2 步的逐库点名法，或给用户临时页

**备用：临时生成 HTML 预览页**（预览页打不开时的兜底）

把下载的 SVG **内嵌**进一个临时 HTML 网格页，写到项目 `_work/icon-preview.html`：

```html
<!DOCTYPE html>
<html lang="zh">
<head>
<meta charset="UTF-8">
<title>图标候选预览 — {关键词}</title>
<style>
  body { font-family: system-ui, sans-serif; background: #F5F5F4; margin: 24px; }
  h1 { font-size: 18px; color: #1C1917; }
  .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); gap: 12px; }
  .card { background: #FFF; border: 1px solid #E7E5E4; border-radius: 10px; padding: 16px 12px;
          text-align: center; cursor: pointer; transition: box-shadow .15s; }
  .card:hover { box-shadow: 0 2px 8px rgba(0,0,0,.12); }
  .card.selected { outline: 2px solid #5E6AD2; }
  .card svg { width: 48px; height: 48px; color: #111111; }
  .lib { display: inline-block; font-size: 11px; color: #78716C; background: #F5F5F4;
         border-radius: 4px; padding: 1px 6px; margin-top: 8px; }
  .name { font-size: 12px; color: #44403C; margin-top: 4px; word-break: break-all; }
</style>
</head>
<body>
<h1>图标候选：{关键词}（{N} 个，来自 {M} 个库）</h1>
<div class="grid" id="grid"></div>
<script>
// SVG 内容已内嵌（离线可用，双击即开，无 CORS 问题）
const icons = [
  { lib: "Lucide", name: "lucide/ticket", svg: `<svg xmlns="...">...</svg>` },
  { lib: "Tabler", name: "tabler/ticket", svg: `<svg ...>...</svg>` },
  { lib: "Phosphor", name: "ph/ticket-bold", svg: `<svg ...>...</svg>` },
  // ...
];
const grid = document.getElementById("grid");
icons.forEach(i => {
  const c = document.createElement("div");
  c.className = "card";
  c.innerHTML = i.svg + `<br><span class="lib">${i.lib}</span><div class="name">${i.name}</div>`;
  c.onclick = () => { c.classList.toggle("selected");
    console.log("selected:", i.name); };
  grid.appendChild(c);
});
</script>
</body>
</html>
```

**要点**：

- **SVG 内嵌而非 `<img src>` 外链** → 双击 file:// 直接打开、离线可用、无 CORS 问题
- 统一 `color: #111111`，浅灰底白卡，形状对比才公平
- 点击卡片切换选中态，`console.log` 记录选中项（browser-use 可读取）
- 文件放项目 `_work/`（临时产物，任务结束可清理）

### 第 4 步：用户选定 + 模型自查

**用户选定**：预览页在浏览器里开着，用户直接回复「用 N 号」。编号 → 图标名的映射用第 3 步第 4 条的 DOM 读取拿到。用户没说话只点了卡片时，读取页面详情弹层标题（`#N · prefix:name`）或问用户。

**模型自查**（必做——图标形状是否匹配风格是视觉判断，光看名字猜不准）：

1. 预览页路径：browser-use 已开着 → 对当前视口截图，模型亲自看候选形状
2. 临时页路径：项目根目录起服务 `python -m http.server 8901`（后台）→ browser-use 打开 `http://localhost:8901/_work/icon-preview.html` → 截图
3. 对每个候选做「风格 × 权重」匹配判断（对照 `icons.md` 匹配矩阵），在对话里给出推荐排序

也可以顺手给用户 Iconify 官网搜索链接做参考：`https://icon-sets.iconify.design/`。

### 第 5 步：选定 → 落地

1. 用户回复编号（或从页面 DOM 读映射），拿到确定的 `prefix/name`
2. SVG 获取：预置图标直接用页面内嵌的 SVG（详情弹层「复制 SVG」或读 `INLINE_SVG`）；非预置的从 Iconify API 下载
3. `import_svg` 把 SVG 画进目标 Figma 位置（32px 或页面所需尺寸）
4. `set_fills` 上色并**绑定语义色变量**（token 绑定率要求 100%）
5. 权重/风格按 `icons.md` 匹配矩阵统一（例如整体是 Phosphor Light，候选里选 ph/ticket-light）
6. 清理临时文件（`_work/` 下的临时 SVG/HTML，用户确认选完再清）

---

## 四、选型报告模板

搜索+预览完成后，在对话里给用户这张表：

| 库 | 图标名 | 形状特征（截图所见） | 许可 | 风格匹配 |
|---|---|---|---|---|
| Lucide | `lucide/ticket` | 线描 2px 圆角，简洁 | ISC | ✅ 适合现代浅色 |
| Phosphor | `ph/ticket-bold` | 填充式，分量重 | MIT | ⚠️ 与极简风冲突 |
| Fluent | `fluent/ticket-24-regular` | 线描，微软风 | MIT | ✅ 通用 |

并注明：哪些库缺此图标（octicon / iconoir 本次无 ticket）、哪些候选被风格矩阵排除。

---

## 五、检查清单

- [ ] 搜索候选 + 三个索引不全库（ph/icon-park/fluent）补搜都做了
- [ ] 每个候选都实际下载验证过（`<svg` 开头才算存在）
- [ ] HTML 预览页 SVG 已内嵌（双击可开），统一黑色填充
- [ ] 模型看过截图并给出风格匹配判断
- [ ] 用户已选定
- [ ] 落地时 fill 绑定了语义色变量
- [ ] 权重/风格与整体风格矩阵一致
- [ ] 临时文件已清理
