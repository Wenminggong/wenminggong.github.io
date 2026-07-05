# 学术主页维护指南

本仓库用于维护一个基于 Jekyll 和 GitHub Pages 的个人学术主页。

语言：[English README](../README.md) | 中文

当前主页采用“内容和展示分离”的结构。日常维护时，通常只需要修改下面几个文件：

- 全站配置和侧边栏个人信息：`_config.yml`
- 主页个人简介：`_pages/includes/intro.md`
- 论文数据：`_data/publications.yml`
- 教育和工作经历：`_pages/includes/others.md`
- 主页内容组合入口：`_pages/about.md`
- 论文卡片和访问地图样式：`assets/css/main.scss`

不要手动修改 `_site/` 目录。它是 Jekyll 构建生成的输出目录，重新构建时会被覆盖。

下文示例统一使用 `Alex Chen`、`Example University`、`example-user` 等化名或占位信息。维护真实主页时，请替换为你自己的实际信息。

## 本地预览

第一次在本机预览前，需要先安装 Ruby 和 Bundler。

在项目根目录运行：

```bash
bundle install
bundle exec jekyll serve --livereload --host 127.0.0.1 --port 4000
```

然后在浏览器打开：

```text
http://127.0.0.1:4000/
```

如果 `4000` 端口被占用，可以换一个端口：

```bash
bundle exec jekyll serve --livereload --host 127.0.0.1 --port 4001
```

然后打开：

```text
http://127.0.0.1:4001/
```

注意：

- 修改 `_config.yml` 后需要重启 Jekyll server。
- 修改 Markdown、YAML、SCSS 文件后，一般会自动刷新。
- 仓库里也有 `run_server.sh`，但显式使用上面的 `bundle exec jekyll serve --livereload ...` 命令更容易排查问题。

## 主页结构

主页入口文件是 `_pages/about.md`，它按顺序组合以下内容：

```liquid
{% include_relative includes/intro.md %}
{% include_relative includes/pub.md %}
{% include_relative includes/others.md %}
{% include visitor-map.html %}
```

一般情况下，不需要修改 `_pages/about.md`。只有当你想调整整个主页 section 的顺序，或者新增/删除一个大 section 时，才需要改这个文件。

## 修改个人信息和全站配置

全站配置位于 `_config.yml`。

示例：

```yml
title: "Alex Chen"
description: "Ph.D. Candidate at Example University."
repository: "example-user/example-user.github.io"

author:
  name: "Alex Chen"
  avatar: "images/profile.jpg"
  location: "Example City, Country"
  googlescholar: "https://scholar.google.com/citations?user=EXAMPLE_ID"
  email: "alex.chen@example.edu"
  github: "example-user"
  linkedin: "alex-chen-example"
  orcid: "https://orcid.org/0000-0000-0000-0000"
```

常见更新方式：

- 更换头像：把新图片放到 `images/`，然后修改 `author.avatar`。
- 修改邮箱、Google Scholar、GitHub、LinkedIn、ORCID：修改 `author` 下对应字段。
- 添加个人网站：填写 `author.uri`。
- 修改左侧栏短描述：修改 `description`。
- 修改网页标题：修改 `title`。

修改 `_config.yml` 后，记得重启本地 Jekyll server。

## 修改 About Me

个人简介位于 `_pages/includes/intro.md`。

这个文件适合放：

- 当前身份
- 导师和课题组
- 研究兴趣
- 2-4 个研究方向 bullet

示例：

```md
# About Me
Hello, I am Alex Chen. I am currently a Ph.D. candidate in the Department of Robotics at Example University.

My research interests lie in robotics, learning-based control, and human-robot interaction. I currently focus on:

* Human-Robot Collaboration
* Learning for Dynamics, Control, and Planning
```

建议保持简洁，让主页读起来更清爽。

## 修改教育和工作经历

教育和经历位于 `_pages/includes/others.md`。

示例：

```md
# Education
- *2024.09 - Present*, Ph.D., Department of Robotics, Example University.
- *2021.09 - 2024.06*, M.S., Department of Automation, Sample Institute of Technology.

# Experience
- *2024.03 - 2024.08*, Research Assistant, Example Robotics Lab.
- *2020.07 - 2021.08*, Software Engineer, Example Tech Inc.
```

维护建议：

- 正在进行的经历使用 `Present`。
- 时间格式保持一致，例如 `YYYY.MM - YYYY.MM`。
- 不要在每行末尾留下多余空格。

## 修改 Publications

论文内容由 `_data/publications.yml` 管理。

不要直接在 `_pages/includes/pub.md` 中手写论文列表。现在 `pub.md` 只负责渲染论文组件：

```md
# Publications

{% include publications.html %}
```

### 论文条目格式

每篇论文使用如下结构：

```yml
- id: example-paper
  visible: true
  category: journal
  title: "An Example Paper Title for Academic Homepage Maintenance"
  url: "https://doi.org/10.0000/example"
  authors: "<strong>Alex Chen</strong>, Taylor Lee, and Jordan Smith"
  venue: "Journal of Example Robotics"
  year: "2026"
  media:
    path: "/papers/figures/example_paper_framework.png"
    alt: "Framework overview for the example paper"
  links:
    - label: "Code"
      url: "https://github.com/example-user/example-paper"
    - label: "PDF"
      url: "/papers/example_paper.pdf"
```

字段说明：

- `id`：论文唯一标识，建议使用小写英文和连字符。
- `visible`：`true` 表示显示，`false` 表示保留数据但不显示。
- `category`：论文分类，可用 `journal`、`conference`、`preprint`。
- `title`：论文标题。
- `url`：论文主链接，例如 IEEE、ScienceDirect、arXiv、DOI 或项目主页。
- `authors`：作者列表。可以用 `<strong>Your Name</strong>` 加粗自己的名字。
- `venue`：期刊、会议、workshop 或 preprint 信息。
- `year`：年份。
- `media.path`：论文卡片展示图路径。
- `media.alt`：图片的简短描述，用于可访问性。
- `links`：显示在论文卡片下方的按钮链接。

### 新增一篇论文

1. 把 PDF 放入 `papers/`，例如：

   ```text
   papers/example_paper.pdf
   ```

2. 把展示图放入 `papers/figures/`，例如：

   ```text
   papers/figures/example_paper_framework.png
   ```

3. 在 `_data/publications.yml` 中新增一个论文条目。

4. 本地预览，检查图片、PDF、代码链接、视频链接是否正常。

### 暂时隐藏一篇论文

不要删除条目，直接设置：

```yml
visible: false
```

这样既不会在主页显示，也不会丢失论文数据。

### 论文展示图建议

推荐格式：

- `.png`：适合框架图、系统图、截图。
- `.jpg`：适合照片或复杂图像。
- 不建议直接使用 `.pdf` 作为论文卡片的展示图。

如果你有一个 PDF 图，例如：

```text
papers/figures/system_framework.pdf
```

建议先导出成：

```text
papers/figures/system_framework.png
```

然后在 `_data/publications.yml` 中引用 PNG：

```yml
media:
  path: "/papers/figures/system_framework.png"
```

## 配置 Visitor Map

访问地图使用可选的 Flag Counter 嵌入方式。默认关闭。

配置位于 `_config.yml`：

```yml
visitor_map:
  enabled: false
  provider: "flagcounter"
  embed_html: ""
```

启用方式：

1. 到 Flag Counter 生成你自己的嵌入 HTML。
2. 将生成的 HTML 原样填入 `embed_html`。
3. 设置 `enabled: true`。
4. 重启 Jekyll server。

示例：

```yml
visitor_map:
  enabled: true
  provider: "flagcounter"
  embed_html: >
    <a href="https://info.flagcounter.com/example"><img src="https://s01.flagcounter.com/example/bg_FFFFFF/txt_000000/border_CCCCCC/columns_4/maxflags_12/viewers_0/labels_0/pageviews_0/flags_0/percent_0/" alt="Flag Counter"></a>
```

不要手动编造 Flag Counter ID。必须使用 Flag Counter 官方生成的代码，这样访问统计才会进入你的专属 counter。

## 可选功能开关

以下配置都在 `_config.yml` 中，默认保持关闭或空值。

### MathJax

如果页面需要显示 LaTeX 公式，可以开启：

```yml
mathjax: true
```

如果主页没有公式，建议保持：

```yml
mathjax: false
```

### Google Analytics

如果你有 Google Analytics measurement ID，可以填写：

```yml
google_analytics_id: "G-XXXXXXXXXX"
```

如果该字段为空，页面不会加载 Google Analytics 脚本。

### Google Scholar Citation Stats

原模板支持自动抓取 Google Scholar 引用统计，但当前默认关闭：

```yml
google_scholar_stats_enabled: false
google_scholar_stats_use_cdn: true
```

只有当你确认 crawler workflow 和 `google-scholar-stats` 分支都已经正确配置后，再开启这个功能。

## 修改导航栏

顶部导航配置位于 `_data/navigation.yml`。

当前锚点：

```yml
main:
  - title: "About Me"
    url: "/#about-me"
  - title: "Publications"
    url: "/#publications"
  - title: "Education"
    url: "/#education"
  - title: "Experience"
    url: "/#experience"
```

如果修改了 section 标题，需要同步检查这里的锚点是否仍然正确。

## 修改样式

当前自定义样式主要位于 `assets/css/main.scss`。

论文卡片相关 class：

- `.publication-list`
- `.publication-card`
- `.publication-card__media`
- `.publication-card__body`
- `.publication-card__title`
- `.publication-card__links`

访问地图相关 class：

- `.visitor-map`
- `.visitor-map__embed`

建议只做小范围、明确目的的样式修改，避免大面积改动主题文件。

## 发布前检查

每次准备提交或发布前，建议运行：

```bash
git status
git diff --check
bundle exec jekyll build
```

同时手动检查：

- 主页可以本地打开。
- 论文图片正常显示，没有变形或缺失。
- PDF、代码、视频链接可用。
- 顶部导航可以跳转到正确 section。
- Visitor Map 在未启用时不会显示。
- 修改 `_config.yml` 后已经重启 Jekyll server。

## 提交和部署

检查无误后：

```bash
git add .
git commit -m "update homepage"
git push
```

GitHub Pages 会根据仓库的 Pages 设置自动部署。

## 常见问题

### `bundle: command not found`

说明本机没有安装 Ruby/Bundler。安装后重新运行：

```bash
bundle install
```

### 修改 `_config.yml` 后页面没有变化

停止当前 Jekyll server，然后重新启动。

### 新增论文图片不显示

检查：

- 文件是否真实存在。
- 路径是否以 `/` 开头，例如 `/papers/figures/example.png`。
- 文件是否已经加入 git。
- 文件名大小写是否完全一致。

### 新增论文没有显示

检查：

- `visible: true`
- `category` 是否为 `journal`、`conference` 或 `preprint`
- YAML 缩进是否使用空格而不是 tab
- `_data/publications.yml` 是否是合法 YAML

### Visitor Map 不显示

检查：

- `visitor_map.enabled: true`
- `visitor_map.embed_html` 不是空字符串
- 粘贴的 HTML 是有效的 Flag Counter 嵌入代码
- 修改 `_config.yml` 后已经重启 Jekyll server

## Acknowledgements

本主页基于 AcadHomepage 模板改造而来。AcadHomepage 参考了 Minimal Mistakes 和 Academic Pages，并使用 Font Awesome 与 Academicons 提供图标支持。
