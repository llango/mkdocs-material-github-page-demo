---
template: overrides/main.html
---

# 自定义

网站与主题一样多种多样，使用Material for MkDocs使得MkDocs变得更漂亮。然而，有时候，可能需要对主题做些许改动才能符合我们自己的需求。

## 添加资源文件

[MkDocs][1]提供了多种方法来自定义一个主题。为了对Material for MkDocs进行一些调整，可以添加css和JavaScript文件到`docs`目录。

  [1]: https://www.mkdocs.org

### 添加CSS

如果想调整颜色或者一些元素间的距离等，可以通过单独添加CSS样式来实现。最简单的方法是在`docs`目录下添加CSS文件，类似与这种结构：

``` sh
.
├─ docs/
│  └─ stylesheets/
│     └─ extra.css
└─ mkdocs.yml
```

然后在`mkdocs.yml`中添加以下内容：

``` yaml
extra_css:
  - stylesheets/extra.css
```

运行[预览服务][2]，然后在添加的css文件做些许变动，稍后就能到改动的效果了。

  [2]: creating-your-site/#_5

### 添加JavaScript文件

与添加CSS文件类似。如果想使用其它的语法高亮或者自定义的一些脚本，在`docs`目录下新建JavaScript文件即可：

``` sh
.
├─ docs/
│  └─ javascripts/
│     └─ extra.js
└─ mkdocs.yml
```

然后，在`mkdocs.yml`文件中添加以下内容：

``` yaml
extra_javascript:
  - javascripts/extra.js
```

更多内容请查阅[MkDocs documentation][3]。

  [3]: https://www.mkdocs.org/user-guide/styling-your-docs/#customizing-a-theme

## 扩展主题

如果要更改最终生成的HTML文件的内容（比如：添加或删除部分内容），此时需要扩展主题。MkDocs支持[theme extension][4]，一种覆写部分Material for MkDocs的简易方法，而不需要分叉（forking）git库。此方法确保Material for MkDocs可以简易的升级到最新版。

  [4]: https://www.mkdocs.org/user-guide/styling-your-docs/#using-the-theme-custom_dir

### 设置

新建一个`overrides`目录，并在`custom_dir`文件中添加以下内容：

``` yaml
theme:
  name: material
  custom_dir: overrides
```

!!! warning "主题扩展先决条件 "

    由于`custom_dir`变量是由theme extension进程调用的，所有要求Material for MkDocs是通过`pip`方式完成安装的，且要求`mkdocs.yml`文件中的`name`完成设置。

`overrides`目录中的结构必须镜像原始主题的目录结构，因为`overrides`目录中的任何文件都将使用与原始主题相同的名称替换该文件。 此外，也可以将其他资源文件放置在`overrides`目录中。

主题的目录结构如下：

``` sh
.
├─ .icons/                             # 捆绑的图标
├─ assets/
│  ├─ images/                          # 图片和图标
│  ├─ javascripts/                     # JavaScript
│  └─ stylesheets/                     # CSS
├─ partials/
│  ├─ integrations/                    # 第三方内容
│  │  ├─ analytics.html                # - Google Analytics
│  │  └─ disqus.html                   # - Disqus
│  ├─ languages/                       # 本地化语言文件
│  ├─ footer.html                      # Footer bar
│  ├─ header.html                      # Header bar
│  ├─ language.html                    # 本地化标签
│  ├─ logo.html                        # Logo in header and sidebar
│  ├─ nav.html                         # Main navigation
│  ├─ nav-item.html                    # Main navigation item
│  ├─ palette.html                     # Color palette
│  ├─ search.html                      # Search box
│  ├─ social.html                      # Social links
│  ├─ source.html                      # Repository information
│  ├─ source-date.html                 # Last updated date
│  ├─ source-link.html                 # Link to source file
│  ├─ tabs.html                        # Tabs navigation
│  ├─ tabs-item.html                   # Tabs navigation item
│  ├─ toc.html                         # Table of contents
│  └─ toc-item.html                    # Table of contents item
├─ 404.html                            # 404 error page
├─ base.html                           # Base template
└─ main.html                           # Default page
```

### 覆写部分

为了覆盖一个部分，可以在`overrides`目录中用相同名称和位置的文件替换它。 例如，要替换原始的`footer.html`，在`overrides/partials`目录中创建一个`footer.html`文件即可：

``` sh
.
├─ overrides/
│  └─ partials/
│     └─ footer.html
└─ mkdocs.yml
```
MkDocs将在渲染主题时使用新的局部文件。可以使用任何文件来完成。

### 覆盖块

除了覆盖部分，还可以覆盖（和扩展）在模板内部定义或者包装特定功能的_template blocks_。 要覆盖一个块，在`overrides`目录中创建一个`main.html`文件：

``` sh
.
├─ overrides/
│  └─ main.html
└─ mkdocs.yml
```

举例说明，如果要覆写页面的标题，则在`main.html`中添加以下内容：

``` html
{% extends "base.html" %}

{% block htmltitle %}
  <title>Lorem ipsum dolor sit amet</title>
{% endblock %}
```

Material for MkDocs提供了以下的template blocks：

| Block name   | Wrapped contents                                |
| ------------ | ----------------------------------------------- |
| `analytics`  | Wraps the Google Analytics integration          |
| `announce`   | Wraps the announcement bar                      |
| `config`     | Wraps the JavaScript application config         |
| `content`    | Wraps the main content                          |
| `disqus`     | Wraps the Disqus integration                    |
| `extrahead`  | Empty block to add custom meta tags             |
| `fonts`      | Wraps the font definitions                      |
| `footer`     | Wraps the footer with navigation and copyright  |
| `header`     | Wraps the fixed header bar                      |
| `hero`       | Wraps the hero teaser (if available)            |
| `htmltitle`  | Wraps the `<title>` tag                         |
| `libs`       | Wraps the JavaScript libraries (header)         |
| `scripts`    | Wraps the JavaScript application (footer)       |
| `source`     | Wraps the linked source files                   |
| `site_meta`  | Wraps the meta tags in the document head        |
| `site_nav`   | Wraps the site navigation and table of contents |
| `styles`     | Wraps the stylesheets (also extra sources)      |
| `tabs`       | Wraps the tabs navigation (if available)        |

更多内容请参考[MkDocs documentation][5]。

  [5]: https://www.mkdocs.org/user-guide/styling-your-docs/#overriding-template-blocks

## 开发扩展

Material for MkDocs使用[Webpack][6]作为构建工具来利用[TypeScript][7]和[SASS][8]等现代Web技术。如果要进行更根本的更改，则需要直接在主题源码中进行调整并重新编译。

  [6]: https://webpack.js.org/
  [7]: https://www.typescriptlang.org/
  [8]: https://sass-lang.com

### 开发环境设置

Material for MkDocs的开发，要求[Node.js][9]的版本至少是ver12。首先，克隆GitHub库：

```
git clone https://github.com/squidfunk/mkdocs-material
```

接着，安装所有依赖：

```
cd mkdocs-material
pip install -r requirements.txt
pip install mkdocs-minify-plugin
pip install mkdocs-redirects
npm install
```

  [9]: https://nodejs.org

### 开发模式

启动Webpack的看门狗：

```
npm start
```

接着，等待几秒后，启动MkDocs预览服务：

```
mkdocs serve
```

浏览器访问[localhost:8000][10]，查看预览效果。

!!! warning "Automatically generated files 自动生成的文件"

    切勿在`material`目录中进行任何更改，因为该目录的内容是从`src`目录自动生成的，且在构建主题时会被覆盖。

  [10]: http://localhost:8000

### 构建主题

完成更改后，使用以下命令来构建主题：

```
npm run build
```

This triggers the production-level compilation and minification of all
stylesheets and JavaScript sources. When the command exits, the final files are
located in the `material` directory. Add the `theme_dir` variable pointing to
the aforementioned directory in the original `mkdocs.yml`.

Now you can run `mkdocs build` and you should see your documentation with your
changes to the original theme.
