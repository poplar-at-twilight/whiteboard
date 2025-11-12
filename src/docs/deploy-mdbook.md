# 将 mdbook 项目部署到 Pages 中

- 参考：[Automated Deployment: GitHub Pages (Deploy from branch)](https://github.com/rust-lang/mdBook/wiki/Automated-Deployment%3A-GitHub-Pages-%28Deploy-from-branch%29)

首次部署：

```shell
mdbook build
git worktree add --orphan -B gh-pages gh-pages
cp -r book/* gh-pages
git config user.name "Deploy from CI"
git config user.email ""
cd gh-pages
git add -A
git commit -m 'deploy new book'
git push origin +gh-pages
```

再次部署：

```shell
mdbook build
cp -r book/* gh-pages
cd gh-pages
git add -A
git commit -m 'deploy new book'
git push origin +gh-pages
```