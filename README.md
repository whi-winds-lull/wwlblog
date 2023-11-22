# wwlblog
## 个人博客-使用Material for MkDocs构建
### Installation
```
git clone https://github.com/whi-winds-lull/wwlblog.git
```
安装主题及其依赖项
```
pip install -e mkdocs-material
```
### Publishing your site
GitHub Pages
GitHub上托管代码，GitHub Pages无疑是发布项目文档最方便的方式。它是免费的，而且很容易设置。
使用GitHub Actions，您可以自动部署项目文档。在仓库的根目录下，创建一个新的GitHub Actions工作流，例如： .github/workflows/ci.yml ，复制粘贴以下内容：
```yaml
name: ci
on:
  push:
    branches:
      - main # 根据实际的分支情况设置
permissions:
  contents: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: 3.x
      - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV
      - uses: actions/cache@v3
        with:
          key: mkdocs-material-${{ env.cache_id }}
          path: .cache
          restore-keys: |
            mkdocs-material-
      - run: pip install -r requirements.txt
      - run: mkdocs gh-deploy --force
```
当一个新的提交被推送到 master 或 main 分支时，静态站点会自动构建和部署。推送更改以查看工作流的运行情况。
如果GitHub Page在几分钟后没有显示，请转到存储库的设置，并确保GitHub Page的发布源分支设置为 gh-pages
您的网站将很快出现在 <username>.github.io/<repository>