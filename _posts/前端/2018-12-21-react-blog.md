---
layout: post
title: 部署 react 到 github  gh-pages
category: 前端
tags: [react,github,blog]
keywords: markdown
description:
typora-copy-images-to: ipic

---

# 示例

<https://zk4.github.io/react-blog>

# 步骤

    1. 准备 github repo
    2. 准备 react 环境
    3. 上传



## 准备 github repo

在 github 里建一个公开库。随意取个名字，比如示例的 react-blog。

## 准备 react 环境

``` js 
create-react-app react-blog
cd react-blog
git init 
git remote add origin git@github.com/zk4/react-blog
yarn add --dev gh-pages 
```

gh-pages 命令会帮你管理在 github 上建分支的问题。
git remote  为 gh-pages 命令提供传输到的位置。


在 `package.json` 中添加

```json
"homepage": "https://<github-username>.github.io/<project-repo>"
```

比如我的

````
"homepage": "https://zk4.github.io/react-blog"
````



## 上传

```bash
yarn build&&gh-pages -d build
```



console 显示

````bash
Published
✨  Done in 50.75s.
````



 打开 <https://zk4.github.io/react-blog>

![](https://ws1.sinaimg.cn/large/006tNbRwly1fyezjkcqsnj30i30cnmzk.jpg)

成功。

