---
template: overrides/main.html
---

# 新建网站

在完成[Material for MkDocs的安装][1]后，可以使用`mkdocs`相关命令来启动文档。转到要放置项目的目录，然后输入：

```
mkdocs new .
```

如果你正在使用的是Docker中的Material for MkDocs，则使用以下命令：

=== "Unix"

    ```
    docker run --rm -it -v ${PWD}:/docs squidfunk/mkdocs-material new .
    ```

=== "Windows"

    ```
    docker run --rm -it -v "%cd%":/docs squidfunk/mkdocs-material new .
    ```

以上操作会新建以下结构的文件：

```
.
├─ docs/
│  └─ index.md
└─ mkdocs.yml
```

  [1]: getting-started.md

## 配置

### 最小配置

只需要简单的添加以下几行内容到`mkdocs.yml`即可启用主题。请注意，由于有几种不同的[安装][2]方法，因此配置可能会略有不同：

=== "pip, docker"

    ``` yaml
    theme:
      name: material
    ```

=== "git"

    ``` yaml
    theme:
      name: null
      custom_dir: mkdocs-material/material

      # 404 page
      static_templates:
        - 404.html

      # Necessary for search to work properly
      include_search_page: false
      search_index_only: true

      # Default values, taken from mkdocs_theme.yml
      language: en
      font:
        text: Roboto
        code: Roboto Mono
      favicon: assets/favicon.png
      icon:
        logo: logo
    ```

_如果是从GitHub克隆的MkDocs from GitHub，那么应当列出所有主题的默认项，因为[`mkdocs_theme.yml`][3]不会作为[官方的描述文件][4]被自动载入_

  [2]: getting-started.md/#_2
  [3]: https://github.com/squidfunk/mkdocs-material/blob/master/src/mkdocs_theme.yml
  [4]: https://www.mkdocs.org/user-guide/custom-themes/#creating-a-custom-theme

### 高级设置

Material for MkDocs包含许多可配置项，_设置_章节有如何设置或者自定义颜色、字体、图标等等的详细说明。

<div class="tx-columns" markdown="1">

- [修改颜色][5]
- [修改字体][6]
- [修改语言][7]
- [修改logo图片和icon图标][8]
- [设置导航][9]
- [设置站内搜索][10]
- [设置访问统计][11]
- [设置versioning][12]
- [设置头部(header)][13]
- [设置底部(footer)][14]
- [添加Github库(repository)][15]
- [添加评论系统][16]

</div>

  [5]: setup/changing-the-colors.md
  [6]: setup/changing-the-fonts.md
  [7]: setup/changing-the-language.md
  [8]: setup/changing-the-logo-and-icons.md
  [9]: setup/setting-up-navigation.md
  [10]: setup/setting-up-site-search.md
  [11]: setup/setting-up-site-analytics.md
  [12]: setup/setting-up-versioning.md
  [13]: setup/setting-up-the-header.md
  [14]: setup/setting-up-the-footer.md
  [15]: setup/adding-a-git-repository.md
  [16]: setup/adding-a-comment-system.md

## 预览

MkDocs包含一个试试预览的服务，所有可以在撰写文档的过程中进行实时预览。当文档修改保存后，这个服务会自动重建整个网站的文档。使用以下命令启动：

```
mkdocs serve
```

如果使用的是Docker中的Material for MkDocs，则使用以下命令：

=== "Unix"

    ```
    docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material
    ```

=== "Windows"

    ```
    docker run --rm -it -p 8000:8000 -v "%cd%":/docs squidfunk/mkdocs-material
    ```

浏览器打开[localhost:8000][17]，应该就能看到类似下图所示的内容：

[![Creating your site][18]][18]

  [17]: http://localhost:8000
  [18]: assets/screenshots/creating-your-site.png

## 生成网站

当文档编辑完成后，可以通过以下命令将所有的Markdown文件生成一个静态网站：

```
mkdocs build
```

该目录中的内容就是项目文档/网站。因为是完全独立的，所以不需要操作数据库或者服务器。生成的网站可以托管在[GitHub Pages][19]、[GitLab Pages][20]、CDN网络或者其它的web服务器上。

  [19]: publishing-your-site.md#github-pages
  [20]: publishing-your-site.md#gitlab-pages
