<https://lins403.github.io/hexo-blog>

## 建站

> https://hexo.io/zh-cn/docs/setup



## 更换主题

> https://github.com/fluid-dev/hexo-theme-fluid



## 添加GitHub Pages

> [如何使用Github Pages?](https://developer.mozilla.org/zh-CN/docs/Learn/Common_questions/Using_Github_pages)
>
> [新增 SSH 密钥到 GitHub 帐户](https://docs.github.com/cn/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account)

## 部署

```bash
deploy:
  type: git
  repo: https://github.com/lins403/hexo-blog.git
  branch: gh-pages

npm install hexo-deployer-git --save
hexo clean
hexo d -g	#deploy,部署之前预先生成静态文件
```
