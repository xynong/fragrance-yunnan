# 香芬云南 XYONG · YUNNAN 网站源码 v2.0（跨境电商 B2B 改版）

纯 HTML/CSS/JavaScript 静态网站，无需构建步骤，可直接部署到 GitHub Pages。

- 域名：`xyong.ccwu.cc`（见 `CNAME` 文件——**如果你的实际域名/仓库名是 `xynong` 而不是 `xyong`，上传前请先修改 CNAME 文件内容，并同步修改本文件与文档中的域名，否则自定义域名会绑定失败**）
- 中文站（面向国内/零售）：根目录 `index.html` 等
- 英文B2B站（面向跨境批发/OEM买家）：`en/` 目录 `index.html` 等
- 数据：`data/products.json`（产品，含中英文字段与批发信息）、`data/batches.json`（批次检测数据，含中英文字段）
- 图片/视频素材说明：见 `assets/img/README-图片素材指南.md`、`assets/video/README-视频素材指南.md`
- 部署步骤：见方案包中 `docs/05-网站部署指南-Cloudflare-GitHub.md`（原有步骤不变）
- 本次改版详细说明：见方案包中 `docs/07-国际化跨境电商B2B改版方案.md`

## 本地预览

```bash
cd website
python3 -m http.server 8000
```
浏览器访问 `http://localhost:8000`（中文站）或 `http://localhost:8000/en/`（英文B2B站）。
不要直接双击 index.html 用 file:// 方式打开，会导致 fetch() 无法读取本地 JSON。

## 更新批次数据

编辑 `data/batches.json`，参考 `factory-data/batch_schema.json` 字段格式追加新批次对象
（记得同时补充 `plant_en` / `origin_town_en` / `release_status_en` 等英文字段，供 `/en/quality.html` 使用），
提交(commit & push)到GitHub即可，网站在1-2分钟内自动更新。

## 新增功能一览（v2.0）

1. 英文 `/en/` 子目录：面向国际批发/OEM买家的完整B2B站点（首页/产品/批发条款/质检数据/关于/询盘表单）
2. 顶部工具条：中英文切换 + 跨境合作入口
3. 询盘表单升级：`contact.html`（中文，零售+批发分流）、`en/request-quote.html`（英文RFQ表单，已预留Formspree接入位）
4. 批发/私人定制专页 `en/wholesale.html`：MOQ表、OEM/ODM说明、认证清单、贸易条款、下单流程、FAQ
5. 媒体占位符系统：图片/视频未上传时自动显示设计感占位符，上传同名文件后自动生效，无需改代码
6. 原创SVG图标：五种植物线条图标、认证图标，避免使用未授权的第三方图片
7. SEO基础：`robots.txt`、`sitemap.xml`、Open Graph标签
