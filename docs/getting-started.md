---
template: overrides/main.html
title: 入门
---

# 入门

Material for MkDocs是[MkDocs][1]的一个主题，[MkDocs][1]是一个用于生成静态页面的程序。如果熟悉Python的话，可以使用[`pip`][2]这个Python的包管理器来安装Material for MkDocs。如果不熟悉的话，推荐使用[`docker`][3]来安装。

安装过程中遇到问题，请查阅[故障排除][4]部分。

  [1]: https://www.mkdocs.org
  [2]: #pip
  [3]: #docker
  [4]: troubleshooting.md

## 安装

### 使用pip

Material for MkDocs 可以使用`pip`来完成安装:

=== "Material for MkDocs"

    ```
    pip install mkdocs-material
    ```

=== "内测版本"

    ``` sh
    pip install git+https://${GH_TOKEN}@github.com/squidfunk/mkdocs-material-insiders.git
    ```

这将自动安装所有依赖包的兼容版本：[MkDocs][1]，[Markdown][5]，[Pygments][6]和[pymdown-extensions][7]。Material for MkDocs始终致力于支持最新版本，因此无需单独安装这些软件包。

_注意，要安装[内测版本][8]，您需要[成为赞助商][9]，创建个人访问令牌[^1]，并设置_`GH_TOKEN`_值的环境变量到token's value。

  [^1]:
    为了使用`pip`通过HTTPS从私有存储库进行安装，[个人访问令牌][14]需要[`repo`][15]范围。 创作并且只有在安装Insiders时才需要使用访问令牌通过HTTPS，这是从CI / CD进行构建时的推荐方法工作流程，例如，使用[GitHub页面][16]或[GitLab页面][17]。
    

  [5]: https://python-markdown.github.io/
  [6]: https://pygments.org/
  [7]: https://facelessuser.github.io/pymdown-extensions/
  [8]: insiders.md
  [9]: insiders.md#how-to-become-a-sponsor

### 使用docker

推荐使用官方的[Docker镜像][10]来快速安装，因为其已经预装了所有依赖。使用以下命令来`拉取`最新版本：


=== "Material for MkDocs"

    ```
    docker pull squidfunk/mkdocs-material
    ```

=== "内测版本"

    ```
    docker login -u ${GH_USERNAME} -p ${GH_TOKEN} ghcr.io
    docker pull ghcr.io/squidfunk/mkdocs-material-insiders
    ```

`mkdocs`可执行文件作为入口点,且`serve`是默认的命令。如果不熟悉Docker也无需担心，后面的章节都会做详细的介绍。

以下插件与Docker镜像捆绑在一起：

- [mkdocs-minify-plugin][11]
- [mkdocs-redirects][12]

_注意，要安装[内测版本][8]，您需要[成为赞助商][9]，创建个人访问令牌[^2]，并设置_`GH_TOKEN`_值的环境变量到token's value。

  [^2]:
    如果是使用`docker`从[GitHub Container Registry][18]拉取私有的Docker镜像，[personal access token][14]需要[`read:packages`][15]范围。需要注意的是，在拉取前需要完成登录。例如，参考workflow的[`publish`][19]流程。同时也需要在账户中启用"[Improved Container Support][20]"

  [10]: https://hub.docker.com/r/squidfunk/mkdocs-material/
  [11]: https://github.com/byrnereese/mkdocs-minify-plugin
  [12]: https://github.com/datarobot/mkdocs-redirects

??? question "如何向docker镜像中添加插件？"

    Material for MkDocs捆绑了有用和常用的插件，同时尽量不扩大官方镜像的大小。如果没有包含要使用的插件，请创建一个新的`Dockerfile`并使用自定义安装例程扩展官方的Docker映像：

    ``` Dockerfile
    FROM squidfunk/mkdocs-material
    RUN pip install ...
    ```

    接下来，使用以下命令构建镜像

    ```
    docker build -t squidfunk/mkdocs-material .
    ```

    可以像使用官方的镜像一样使用新镜像。

### 使用git

通过将存储库克隆到项目根目录的子文件夹中，可以直接从[GitHub] [13]使用Material for MkDocs，如果是想使用最最新的版本，可以尝试此种方法：

=== "Material for MkDocs"

    ```
    git clone https://github.com/squidfunk/mkdocs-material.git
    ```

=== "内测版本"

    ```
    git clone git@github.com:squidfunk/mkdocs-material-insiders.git mkdocs-material
    ```

主题位于文件夹`mkdocs-material/material`中。 从`git`克隆时，必须自己安装所有必需的依赖包：

```
pip install -r mkdocs-material/requirements.txt
```

_注意，要安装[内测版本][8]，您需要[成为赞助商][9]_

  [13]: https://github.com/squidfunk/mkdocs-material

  [14]: https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token
  [15]: https://docs.github.com/en/developers/apps/scopes-for-oauth-apps#available-scopes
  [16]: publishing-your-site.md#github-pages
  [17]: publishing-your-site.md#gitlab-pages
  [18]: https://docs.github.com/en/free-pro-team@latest/packages/getting-started-with-github-container-registry/about-github-container-registry
  [19]: https://github.com/squidfunk/mkdocs-material/blob/master/.github/workflows/publish.yml
  [20]: https://docs.github.com/en/free-pro-team@latest/packages/guides/enabling-improved-container-support
