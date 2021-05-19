---
title: 使用VuePress搭建文档博客
date: 2021-05-17 12:51:33
index_img: /img/2021/05/build-vuepress-doc.jpg
categories: 博客搭建
excerpt: 搭建部署、添加actions、更改配置
tags: 
    - "vuepress"
---

# 博客地址

<a href="https://lins403.github.io/vuepress-doc/" alt="https://lins403.github.io/vuepress-doc/"
    target="_blank" >WebSite</a>

# 关于vuepress

> To Be Continued


# 搭建与部署

>  [官方指南](https://vuepress.vuejs.org/zh/guide/)
>
> [跟着这篇文章做完，你就会搭建个人博客了！](https://www.jianshu.com/p/6e8c608f24c8)

## 本地部署

```sh
# deploy.sh
#!/usr/bin/env sh
set -e
npm run build
cd docs/.vuepress/dist
git init
git add -A
git commit -m 'deploy'
git push -f git@github.com:lins403/vuepress-doc.git master:gh-pages
cd -
rm -rf docs/.vuepress/dist
```

## github actions

```yaml
# .github/workflows/deploy.yml
name: Build and Deploy
on: [push]
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout 🛎️
        uses: actions/checkout@v2 
        with:
          persist-credentials: false

      - name: Install and Build 🔧
        run: |
          npm install
          npm run build
        env:
          CI: false

      - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@releases/v3
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          BRANCH: gh-pages
          FOLDER: docs/.vuepress/dist
```





# 配置

> [默认主题配置](https://vuepress.vuejs.org/zh/theme/default-theme-config.html#%E9%A6%96%E9%A1%B5)

```js
// docs/.vuepress/config.js
module.exports = {
    base: '/vuepress-doc/',
    title: '小眯嘻的文档博客',
    description: 'Just playing around',
    head: [
        ['link', { rel: 'icon', href: '/assets/img/config/favicon.png' }]
      ],
    themeConfig: {
        repo: 'https://github.com/lins403/vuepress-doc',
        repoLabel: 'GitHub',
        nav: [
            { text: 'Home', link: '/' },
            { text: 'HelloWorld', link: '/blog/HelloWorld.md' }
        ],
        sidebar: 'auto',
        lastUpdated: 'Last Updated', // string | boolean
    }
  }
```