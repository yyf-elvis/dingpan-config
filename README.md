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

## 插件内置兜底副本（重要）

远端三个源（raw / jsDelivr / jsDelivr-Fastly）在受限网络下可能**全部不可达**，
此时插件会回落到随扩展打包的内置副本。所以：

- **改完图文后跑一次同步**，把内容写进插件的内置副本：
  ```
  node 盯盘插件/tools/sync-about.js
  ```
- **打包发布前务必跑一次**（脚本会校验清单里引用的图片是否真的存在）
- 远端可达时仍以远端为主；内置只是兜底，页脚会标明内容来源（GitHub / jsDelivr / 内置内容 / 本地缓存）

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
| `version` | **单调递增的整数**。改内容必须 +1 —— 插件用它做新旧判定（远端版本低于内置副本就丢弃，防止 CDN 缓存把旧内容覆盖回来） |
| `updated` | 修改内容时改成新日期；用于图片缓存刷新 |
| `title` / `subtitle` | 页面顶部标题（可省略） |
| `blocks[].type` | `h2` 小节标题 / `h3` 小标题 / `p` 段落 / `list` 列表 / `img` 图片 / `divider` 分隔线 |
| `img.src` | 相对路径（相对 `about/`，如 `images/a.png`）或完整 `https://` 地址 |
| `img.caption` | 图片下方说明（可省略） |

## 图片建议

- 宽度 1200px 以内，单张建议 < 1MB
- 推荐 png / jpg / webp / gif（截图最稳）；svg 也可用（个别网络下插件会自动切镜像源重试）
- 插件里点击图片会打开原图
