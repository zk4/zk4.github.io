---
layout: post
title: 部署 gatsby JS 到 github  gh-pages
category: 前端
tags: [gatsby,github,blog]
keywords: markdown
description:
typora-copy-images-to: ipic

---

# 示例

<https://zk4.github.io/gatsby-blog>

# 步骤

1. 准备 github repo
2. 准备 gatsby 环境
3. 上传



## 准备 github repo

在 github 里建一个公开库。随意取个名字，比如示例的 gatsby-blog。

## 准备 gatsby 环境

``` js 
yarn global add  gatsby-cli
gatsby new gatsby-blog 
cd gatsby-blog 
git init 
git remote add origin git@github.com/zk4/gatsby-blog
yarn add --dev gh-pages 
```
gh-pages 命令会帮你管理在 github 上建分支的问题。  
git remote  为 gh-pages 命令提供传输到的位置。  

gatsby new 也可以从模板创建  
``` js
gatsby new gatsby-blog https://github.com/gatsbyjs/gatsby-starter-hello-world
```

在 `package.json` 中添加

```json
"homepage": "https://<github-username>.github.io/<project-repo>"
```
比如我的  

```` json
"homepage": "https://zk4.github.io/gatsby-blog"
````

在 gatsby-config.js 中添加

``` json
module.exports = {
pathPrefix: "/gatsby-blog"
}
```
这是为了保证生成的页面的路径路由正确。

## 上传
```bash
gatsby build --prefix-paths && gh-pages -d public
```



console 显示

````bash
Published
✨  Done in 50.75s.
````



 打开 <https://zk4.github.io/gatsby-blog>

![](https://ws1.sinaimg.cn/large/006tNbRwly1fyezhfg714j30qp0ae0vs.jpg)

成功。

