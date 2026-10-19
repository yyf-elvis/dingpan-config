# dingpan-config

「盯盘之王」插件 —— **关于**页面的图文内容仓库（在线更新，无需给插件发版）。

## 目录结构

- `about/index.json` — 图文清单，插件读取的入口文件
- `about/images/` — 图片文件（统一 WebP）

## 更新流程

1. 修改 `about/index.json`，或替换 / 新增 `about/images/` 里的图片
2. 把 `index.json` 的 `version` +1，并把顶层 `updated` 字段改成新日期
   （`updated` 会作为图片 URL 的版本号变化，避免旧图缓存）
3. 提交并推送：
   ```
   git add -A
   git commit -m "update about"
   git push
   ```
4. 打开插件 → 左下角「关于」即可看到最新图文
   （raw.githubusercontent.com 有几分钟 CDN 缓存，刚推送后如未见更新，稍等重开一次）

## 内容源与离线行为

插件**不内置**图文副本，内容只来自本仓库，按 `raw.githubusercontent` →
`jsDelivr` → `jsDelivr(fastly)` 依次拉取；三个源全部不可达时，
显示上次成功拉取的本地缓存（没有缓存才提示加载失败）。

## index.json 结构

```json
{
  "version": 4,
  "updated": "2026-10-19",
  "title": "标题",
  "subtitle": "副标题",
  "blocks": [
    { "type": "h2", "text": "小节标题" },
    { "type": "h3", "text": "小标题" },
    { "type": "p", "text": "段落文字\n支持换行" },
    { "type": "list", "items": ["条目一", "条目二"] },
    { "type": "img", "src": "images/example.webp", "caption": "图片说明" },
    { "type": "divider" }
  ]
}
```

| 字段 | 说明 |
|---|---|
| `version` | **单调递增的整数**。改内容必须 +1 —— 插件以本地缓存版本为下限，远端版本更低则保留缓存，防止 CDN 缓存把旧内容覆盖回来 |
| `updated` | 修改内容时改成新日期；用于图片缓存刷新 |
| `title` / `subtitle` | 页面顶部标题（可省略） |
| `blocks[].type` | `h2` 小节标题 / `h3` 小标题 / `p` 段落 / `list` 列表 / `img` 图片 / `divider` 分隔线 |
| `img.src` | 相对路径（相对 `about/`，如 `images/a.webp`）或完整 `https://` 地址 |
| `img.caption` | 图片下方说明（可省略） |

## 图片建议

- 统一用 **WebP**（体积最小，插件现有图片已全部为 WebP）
- 宽度 1200px 以内，单张建议 < 1MB
- 页面中连续两个 `img` 会拼成一组上下排列（如「关注我」的抖音 / 公众号二维码）
- 出于隐私考虑，插件内图片不可点击放大 / 另存
