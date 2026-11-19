---
name: translate-post
description: 把一篇英文文章（网页链接或本地文件）翻译成中文，核对后发布到本站。
disable-model-invocation: true
argument-hint: <网页链接或文件路径>
---

把 `$ARGUMENTS` 指向的英文文章翻译成中文，放进本仓库（Jekyll，GitHub Pages，推送 `main` 即部署），核对无误后发布。

翻译标准见 [STANDARDS.md](STANDARDS.md)，第 4、5 步都以它为准。仓库约定（front matter 字段、正文顺序、图片目录和引用写法）以 [README.md](../../../README.md) 为准。

## 1. 取原文

- **网页链接**：`curl -sL -A "Mozilla/5.0"` 下载到 `/tmp/translate-<slug>/`，提取 `<article>` 正文（标题、段落、列表、代码、链接、图片地址和 alt、图注、脚注、作者、发表日期）。抓不到正文（JS 渲染、被拦）就用浏览器读页面文本。
- **本地文件**：直接读；PDF、Word 用对应的 skill 读。
- `<slug>` 取原文 URL 的最后一段，或英文标题的短横线写法，后面图片目录也用它。

完成标准：手里有完整原文，按段落列出，图片和视频按出现顺序编号，作者和日期都知道。

## 2. 看本站风格

读 `_posts/` 里最近两篇译文，再按 STANDARDS.md 的「术语」一节查已有译法。

## 3. 下载图片

- 每张图下载到 `images/<slug>/`，按出现顺序命名 `01-短名.png`、`02-短名.png`……（去掉 CDN 的 `fm=webp` 之类参数，拿原图）。
- 下载完逐张打开看，确认编号和原文顺序、图注对得上。批量下载很容易错位一格。
- 视频不下载，用 `<video controls muted playsinline src="原链接"></video>` 嵌入，下面加一行斜体说明和"直接打开"链接，写法照 `_posts/` 里已有的视频。

## 4. 翻译

写到 `_posts/YYYY-MM-DD-中文标题.md`，日期用今天（翻译日期，不是原文日期），文件名里的中文标题去掉空格和标点。

- front matter 按 README 填：`summary` 一句话说清文章讲什么；`poem` 选原文里最有分量的一句英文原话；`source_date` 填原文发表日期。
- 正文按 STANDARDS.md 翻译，段落和原文一一对应。
- 图片写成 `![中文图注]({{ '/images/<slug>/01-xxx.png' | relative_url }})`，原文有图注的在下一行用 `_图注_` 保留。
- 文末依次是：致谢或作者说明（原文有才写）→ 脚注 → `## 译者注` → `## 原文`（版权归属、原文标题和链接、作者和日期，不整篇转载），格式照最近一篇译文。

## 5. 核对

派一个子代理做独立核对，不要把你的判断告诉它。只给它这些：原文文本路径、译文路径、STANDARDS.md 路径、图片目录，让它：

- 逐段对照原文和译文，列出漏译、错译、多译、数字或链接不一致的地方；
- 按 STANDARDS.md 逐条挑出翻译腔、风格走样、术语前后不一致的句子，给出改法；
- 核对每个图片引用都指向存在的文件，且顺序和原文一致。

拿到结果后逐条处理：认同的就改；不认同的写下理由，最后在回报里交代。

完成标准：原文每一段在译文里都有对应，图片、链接、脚注的数量和原文相同，子代理提的问题都已改掉或给了理由。

## 6. 本地构建

```bash
bundle exec jekyll build -d /tmp/translate-<slug>/site
```

检查生成的文章页：能生成；`<img src>` 全部指向 `/translation/images/<slug>/` 下存在的文件；页面里没有残留的 `{{`；顶部有「译自」出处行。

## 7. 发布

推送前先给用户看：中文标题、summary、文件清单、核对时没采纳的意见。等用户明确说「发」「推送」这类话再继续。

```bash
git add _posts/<文件> images/<slug>
git commit -m "Add translation: <英文原标题>"
git push origin main
```

提交信息末尾带上会话要求的 attribution 行。

## 8. 确认上线

- `gh run watch <run-id> -R JX15/translation --exit-status` 等部署跑完。失败就看 `gh run view <run-id> --log-failed`，查明原因，修好后同样先问用户再推送。
- 部署成功后用 `curl` 确认线上文章页、首页列表里的这一篇、至少一张图片都返回 200。站点地址见 README。

完成标准：部署结论是 success，线上文章页和图片都能打开。最后把线上链接发给用户。
