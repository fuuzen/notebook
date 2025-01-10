---
title: Hexo
date: 2024/01/27
math: true
categories:
  - [计算机科学, 前端]
tags:
  - Node.js
---

Hexo 是一个基于 Node 框架的博客框架

```shell
pnpm install hexo-cli -g		//安装

hexo init blog						//初始化

cd blog; npm install			//安装依赖
```

blog 目录下生成静态文件（到 public 目录下，用于发布）

```shell
hexo g						//缩略
hexo generate			//一样
```

blog 目录下清除已生成的静态文件和 cache（ 到 public 目录下）

```shell
hexo clean
```

blog 目录下本地运行 hexo 服务

```shell
hexo s						//缩略
hexo server				//一样
```

在 blog 目录下将 public 目录下的静态文件复制到 nginx 的对应目录，从而使用 nginx 服务部署：

```shell
sudo cp -r public/* /var/www/html/
```

一键部署，参考： https://hexo.io/docs/one-command-deployment

```
hexo d					//缩略
hexo deploy			//一样
```
