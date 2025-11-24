# 将 mdbook 项目部署到 Pages 中

- 参考：[Automated Deployment: GitHub Pages (Deploy from branch)](https://github.com/rust-lang/mdBook/wiki/Automated-Deployment%3A-GitHub-Pages-%28Deploy-from-branch%29)

首次部署：

```bash
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

```bash
mdbook build
cp -r book/* gh-pages
cd gh-pages
git add -A
git commit -m 'deploy new book'
git push origin +gh-pages
```

## shell 脚本

> [!IMPORTANT]
> 此脚本需要在有登陆授权的 git 终端中使用


```bash
#!/bin/sh
# 本脚本用于将 mdbook 部署到 Pages

work_dir=/home/poplar/Others/git/whiteboard
# 默认的工作文件夹
branch_name=gh-pages

cd work_dir

if git branch --list | grep -q "$branch_name"; then
    echo 'Find gh-pages'
    mdbook build
    cp -r book/* gh-pages
    cd gh-pages
    git add -A
    git commit -m 'deploy new book'
    git push origin +gh-pages
else
    echo 'No find gh-pages'
    mdbook build
    git worktree add --orphan -B gh-pages gh-pages
    cp -r book/* gh-pages
    git config user.name "Deploy from CI"
    git config user.email ""
    cd gh-pages
    git add -A
    git commit -m 'deploy new book'
    git push origin +gh-pages
fi
```