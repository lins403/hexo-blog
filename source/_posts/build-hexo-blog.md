---
title: 使用Hexo和Github Pages搭建个人博客
date: 2021-05-14 10:29:17
index_img: /img/2021/05/build-hexo-blog.jpg
categories: 博客搭建
excerpt: 本地建站、更换主题、添加GitHub Pages、部署、添加actions
tags: 
    - "hexo"
---


# 博客地址

<https://lins403.github.io/hexo-blog>

# 本地初始化

> https://hexo.io/zh-cn/docs/setup



# 更换主题

> https://github.com/fluid-dev/hexo-theme-fluid



# 添加GitHub Pages和SSH密钥

> [如何使用Github Pages?](https://developer.mozilla.org/zh-CN/docs/Learn/Common_questions/Using_Github_pages)
>
> [新增 SSH 密钥到 GitHub 帐户](https://docs.github.com/cn/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account)



# 部署

> https://hexo.io/zh-cn/docs/github-pages

```yaml
deploy:
  type: git
  repo: https://github.com/lins403/hexo-blog.git
  branch: gh-pages
```
```bash
npm install hexo-deployer-git --save
hexo clean
hexo d -g	#deploy,部署之前预先生成静态文件
```



# 添加actions

> [github actions 入门指南及实践](https://shanyue.tech/no-vps/github-action-guide.html#%E5%BF%AB%E9%80%9F%E5%BC%80%E5%A7%8B)

- 创建 Workflow 文件：`.github/workflows/deploy.yml`

```yaml
# 模板源于B站up主`objtube的卢克儿`的分享
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
          BRANCH: gh-pages # The branch the action should deploy to.
          FOLDER: public # The folder the action should deploy.
```



# 完整操作

- [使用 Github Pages 和 Hexo 搭建自己的独立博客【超级详细的小白教程】](https://itrhx.blog.csdn.net/article/details/82121420)
- [Luke教你20分钟快速搭建个人博客系列(hexo篇) ](https://www.bilibili.com/video/BV1dt4y1Q7UE)