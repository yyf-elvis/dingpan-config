# dingpan-config

「A股盯盘助手」插件 —— **关于**页面的图文内容仓库（在线更新，无需给插件发版）。

## 目录结构

- `about/index.json` — 图文清单，插件读取的入口文件
- `about/images/` — 图片文件（截图、示意图等）

## 更新流程

1. 修改 `about/index.json`，或替换 / 新增 `about/images/` 里的图片
2. 把 `index.json` 顶层 `updated` 字段改成新日期（会带动图片 URL 版本号变化，避免旧图缓存）
3. 提交并推送：
   ```
   git add -A
   git commit -m "update about"
   git push
   ```
4. 打开插件 → 左下角「关于」→ 点右上角「刷新」即可看到最新图文
   （raw.githubusercontent.com 有几分钟 CDN 缓存，刚推送后如未见更新，稍等再点一次）

## index.json 结构

```json
{
  "version": 1,
  "updated": "2026-10-17",
  "title": "标题",
  "subtitle": "副标题",
  "blocks": [
    { "type": "h2", "text": "小节标题" },
    { "type": "h3", "text": "小标题" },
    { "type": "p", "text": "段落文字\n支持换行" },
    { "type": "list", "items": ["条目一", "条目二"] },
    { "type": "img", "src": "images/example.svg", "caption": "图片说明" },
    { "type": "divider" }
  ]
}
```

| 字段 | 说明 |
|---|---|
| `updated` | 修改内容时改成新日期；用于图片缓存刷新 |
| `title` / `subtitle` | 页面顶部标题（可省略） |
| `blocks[].type` | `h2` 小节标题 / `h3` 小标题 / `p` 段落 / `list` 列表 / `img` 图片 / `divider` 分隔线 |
| `img.src` | 相对路径（相对 `about/`，如 `images/a.png`）或完整 `https://` 地址 |
| `img.caption` | 图片下方说明（可省略） |

## 图片建议

- 宽度 1200px 以内，单张建议 < 1MB
- 推荐 png / jpg / webp / gif（截图最稳）；svg 也可用（个别网络下插件会自动切镜像源重试）
- 插件里点击图片会打开原图
