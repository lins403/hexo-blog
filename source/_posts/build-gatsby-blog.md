---
title: 使用Gatsby搭建文档博客
date: 2021-05-19 13:47:15
index_img: /img/2021/05/build-gatsby-blog.jpg
categories: 博客搭建
excerpt: 搭建部署、添加actions、添加主题、博客建站总结
tags: 
    - "gatsby"
---

# 博客地址

<a href="https://lins403.github.io" alt="https://lins403.github.io"
    target="_blank" >WebSite</a>

# 关于Gatsby

> [作者在ReactiveConf 2019的路演](https://www.bilibili.com/video/BV1kK411772f/?spm_id_from=333.788.recommend_more_video.8)
>
> [Gatsby - Full Tutorial for Beginners](https://www.youtube.com/watch?v=mHFAM0CXviE)
>
> [How to Build a Developer Blog with Gatsby and MDX](https://www.sitepoint.com/gatsby-mdx-blog/)



# 搭建与部署

> [Gatsby.js 中文教程](https://www.gatsbyjs.cn/tutorial/)

```bash
npm install -g gatsby-cli
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
cd hello-world
gatsby develop #热更新的开发环境
```



## 本地部署

> [How Gatsby Works with GitHub Pages](https://www.gatsbyjs.com/docs/how-to/previews-deploys-hosting/how-gatsby-works-with-github-pages/)

```bash
npm install gh-pages

# gatsby-config.js
module.exports = {
  pathPrefix: '/',
}

# package.json
"scripts": {
	"deploy": "gatsby build --prefix-paths && gh-pages -d public -b master"
},

npm run deploy
```

如果不使用gh-pages分支，就要另外建一个分支，比如 `source` 来存放源码，而将 `master` 分支仅仅用于存放打包后的静态资源



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
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} #环境变量

      - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@releases/v3
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          BRANCH: master
          FOLDER: public
```





# 使用主题

> [Themes—官方指南](https://www.gatsbyjs.com/docs/themes/#gatsby-skip-here)

我是直接重新搭建了一个demo，也可以参照官方的 [How to Use a Theme in an Existing Site](https://www.gatsbyjs.com/docs/how-to/plugins-and-themes/using-a-gatsby-theme/) 

安装theme或者构建时遇到的问题：[mozjpeg pre-build test failed](https://www.jianshu.com/p/b1e196d5956a)

```bash
# 问题解决方法
brew install nasm
npm cache clean -f
# 使用这个主题构建网站
gatsby new lins403-blog LekoArts/gatsby-starter-minimal-blog
```

## shadowing

> [shadowing—官方指南](https://www.gatsbyjs.com/docs/how-to/plugins-and-themes/shadowing/)

1. 设置外链的target为blank

   - 新建本地的  `src/@lekoarts/gatsby-theme-minimal-blog/components/header-external-links.tsx`
   - 将 `https://github.com/LekoArts/gatsby-themes/blob/master/themes/gatsby-theme-minimal-blog/src/components/header-external-links.tsx` 的内容copy进来
   - 修改 `<TLink key={link.url} href={link.url} target={`_blank`}>`

2. 引用（不修改）

   - `import Title from "@lekoarts/gatsby-theme-minimal-blog/src/components/title"`

## 使用github API

> [Show off Github repos in your Gatsby site using Github GraphQL API](https://dev.to/lennythedev/show-off-your-github-repos-in-your-gatsby-site-using-graphql-421l)
>
> [gatsby-source-graphql](https://github.com/gatsbyjs/gatsby/tree/master/packages/gatsby-source-graphql)
>
> [gatsby-theme-portfolio](https://github.com/smakosh/gatsby-theme-portfolio)

```
npm install gatsby-source-graphql
npm i gatsby-source-github-api
npm install dotenv
```

<!--好像明文提交了github token，这个就会丢失掉-->

## 再部署

使用主域名（不使用gh-pages分支），使用github actions，如上

```bash
# 解除原来的绑定关系
git remote remove origin

# 在新的项目下初始化git
git init

# 添加resource分支
git switch -c source

# 添加github actions
>添加文件 .github/workflows/deploy.yml

# 绑定至远端
git remote add origin https://github.com/lins403/lins403.github.io.git
git add --all
git commit -m '使用gatsby-theme-minimal-blog主题后的初始化'

# 这里会和远程的source冲突，所以先把远端的source分支删了
> 我觉得手动去github上删除比较好

# 然后再push提交
git push -u origin source
```

！github actions 运行时遇到一个error：

在 `gatsby build` 时，会产生 `github action Error:Source GraphQL API: HTTP error 401 Unauthorized`

解决方法：

> [Github actions 环境变量](https://docs.github.com/cn/actions/reference/environment-variables)

所以就在 deploy.yaml 中的env下添加 `GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}` 就好了



# 添加Google Analytics

<a href="https://analytics.google.com/analytics" alt=""
    target="_blank" >https://analytics.google.com/analytics</a>

> [为网站设置 Google Analytics（分析）(Universal Analytics)](https://support.google.com/analytics/answer/10269537?hl=zh-Hans&ref_topic=9303319)
>
> [How to add Google Analytics to your Gatsby.js website](https://javascript.plainenglish.io/how-to-connect-your-gatsby-js-landing-page-to-google-analytics-and-deploy-to-netlify-step-by-step-8352467583df)

<!--不懂为什么github actions的部署方式无法让这个生效-->



# 添加分页

> [Minimal Blog Theme Pagination Help](https://github.com/LekoArts/gatsby-themes/issues/539) 
>
> [Adding Pagination](https://www.gatsbyjs.cn/docs/adding-pagination/)
>
> [Creating and Modifying Pages](https://www.gatsbyjs.com/docs/creating-and-modifying-pages/#modifying-pages-created-by-core-or-plugins)

<!--没搞出来，思路懂了但一些细节问题还是没弄清-->



# Hexo、VuePress以及Gatsby的使用体验

- 以前使用过 Jekyll，体验很差，对这三个的上手难度和操作体验都比较满意
- 目前文章很少的情况下，Hexo最快，Gatsby次之，vuepress最慢
- Hexo-blog使用了一个完成度很高的主题，功能强大；
- Gatsby文档齐全，生态强大，自定义空间巨大，而且能用jsx实现组件化就很爽，mdx配合着用；
- vuepress目前用的是 1.x 的版本，捉襟见肘，但是感觉挺适合用于一个项目的gh-pages分支，作为在线文档。
- 技术上的差异不怎么了解，以后再深入
- 目前打算将gatsby-blog做主网站，会陆续拓展功能，以后可能迁移至个人服务器；vuepress-doc就用来记录比较系统的学习笔记，比如React学习笔记；hexo-blog就用来记录比较零散的学习笔记，或者项目笔记，诸如 tips-and-tricks 之类的。



# 其它

**与 nextjs**

> [Gatsby.JS vs Next.JS](https://www.solutelabs.com/blog/gatsby-js-vs-next-js-which-one-to-choose-when) 
>
> [[译] NextJS vs. NuxtJS vs. GatsbyJS](https://juejin.cn/post/6882994530149203981)

