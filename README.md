# Translation · JX15 的翻译集

- 站点地址：<https://jx15.github.io/translation>
- 存放我翻译的英文文章。基于 Jekyll，托管在 GitHub Pages（项目站点），推送到 `main` 分支后由 GitHub Actions 自动构建部署。
- 结构与主题沿用 [JX15.github.io](https://github.com/JX15/JX15.github.io)，主题来自 [Tw93](https://tw93.fun/) 的 [cosy-jekyll-theme](https://github.com/tw93/tw93.github.io)，MIT License，保留原作者署名。

## 放什么

我觉得有价值的英文文章。

## 版权

译文仅供学习交流，原文版权归原作者所有。如原作者不希望被翻译，请提 issue，我会第一时间下线。

## 新增一篇译文

在 `_posts/` 下新建 `YYYY-MM-DD-中文标题.md`，front matter 示例：

```yaml
---
layout: post
title: 中文标题
category: 分类            # 如：软件工程 / 产品 / 随笔
summary: 一句话摘要
source_title: Original English Title
source_url: https://example.com/original-article
source_author: Author Name
source_date: 2024-01-01   # 可选，原文发表日期
# 可选：写了 poem，文章页顶部就显示这句话（大字今楷），不写则显示标题
poem: 原文里最打动你的一句话
---
```

文章页会在正文上方自动根据 `source_title` / `source_url` / `source_author` 生成「译自 …」出处行。

正文按这个顺序：

1. **译文正文**：尽量按原文段落一一对应，不合并、不删减。
2. **译者注**（可选）：放在文末，说明术语取舍、有意译的地方、以及和原文不同的补充。
3. **原文**：注明原文版权归属，给出原文标题和链接，不整篇转载。

图片放到 `images/<文章英文短名>/`，按出现顺序编号命名（如 `01-xxx.png`），正文里这样引用：

```markdown
![图注]({{ '/images/<文章英文短名>/01-xxx.png' | relative_url }})
```

## 本地预览

```bash
bundle install
bundle exec jekyll serve
# 打开 http://127.0.0.1:4000/translation/
```

## 与主博客的区别

- 这是**项目站点**，URL 带 `/translation` 前缀，`_config.yml` 里配置了 `baseurl: /translation`，模板里的链接统一走 `relative_url` / `absolute_url`。
- GitHub Actions 里 `configure-pages` 会自动注入 `--baseurl`，无需手动改。
