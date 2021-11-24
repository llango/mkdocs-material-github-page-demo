---
template: overrides/main.html
---

# 发布网站

将网站托管在`git`库中的最大好处是能够在推送新更改时自动部署它。MkDocs使得这一操作更加简单。

## GitHub Pages 

如果已经在GitHub上托管代码，那么使用[GitHub Pages][1]来发布网站再方便不过了。

  [1]: https://pages.github.com/

### 使用GitHub Actions

使用[GitHub Actions][2]可以自动部署网站。在库的根目录下新建一个GitHub Actions workflow，比如：`.github/workflows/ci.yml`，并粘贴入以下内容：

=== "Material for MkDocs"

    ``` yaml
    name: ci
    on:
      push:
        branches:
          - master
          - main
    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v2
          - uses: actions/setup-python@v2
            with:
              python-version: 3.x
          - run: pip install mkdocs-material
          - run: mkdocs gh-deploy --force
    ```

=== "内测版本"

    ``` yaml
    name: ci
    on:
      push:
        branches:
          - master
          - main
    jobs:
      deploy:
        runs-on: ubuntu-latest
        if: github.event.pull_request.head.repo.fork == false
        steps:
          - uses: actions/checkout@v2
          - uses: actions/setup-python@v2
            with:
              python-version: 3.x
          - run: pip install git+https://${GH_TOKEN}@github.com/squidfunk/mkdocs-material-insiders.git
          - run: mkdocs gh-deploy --force
    env:
      GH_TOKEN: ${{ secrets.GH_TOKEN }}
    ```

此时，当一个新的提交推送到`master`或`main`时，我们的静态网站的内容将自动生成并完成部署。可以尝试推送一个提交来查看GitHub Actions的工作状况。

网站将在不久后部署到`<username>.github.io/<repository>`。

_注意，如果是部署的[内测版本][4]，记得在[personal access token][3]中添加`GH_TOKEN`的环境变量值,可以使用[secrets][5]完成操作。_

  [2]: https://github.com/features/actions
  [3]: https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token
  [4]: insiders.md
  [5]: https://docs.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets

### 使用MkDocs

如果是倾向于手动部署网站，请在包含`mkdocs.yml`文件的目录中运行以下命令：

```
mkdocs gh-deploy --force
```

## GitLab Pages

如果是将代码托管在GitLab，可以使用[GitLab CI][7]来部署到[GitLab Pages][6]。在库的根目录中，创建一个名为“ .gitlab-ci.yml”的任务定义，然后复制并粘贴以下内容：

=== "Material for MkDocs"

    ``` yaml
    image: python:latest
    pages:
      stage: deploy
      only:
        - master
      script:
        - pip install mkdocs-material
        - mkdocs build --site-dir public
      artifacts:
        paths:
          - public
    ```

=== "内测版本"

    ``` yaml
    image: python:latest
    pages:
      stage: deploy
      only:
        - master
      script:
        - pip install git+https://${GH_TOKEN}@github.com/squidfunk/mkdocs-material-insiders.git
        - mkdocs build --site-dir public
      artifacts:
        paths:
          - public
    ```

此时，当一个新的提交推送到`master`时，静态网站的内容将自动生成并完成部署。提交并推送文件到库就能看到效果了。

网站将在不久后部署到`<username>.gitlab.io/<repository>`。

_注意，如果是部署的[内测版本][4]，记得在[personal access token][3]中添加`GH_TOKEN`的环境变量值,可以使用[masked custom variables][8]完成操作。_

  [6]: https://gitlab.com/pages
  [7]: https://docs.gitlab.com/ee/ci/
  [8]: https://docs.gitlab.com/ee/ci/variables/#create-a-custom-variable-in-the-ui