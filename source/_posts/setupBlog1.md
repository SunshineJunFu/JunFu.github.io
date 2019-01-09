---
title: hexo搭建个人博客
date: 2019-01-09 12:29:58
tags: [hexo]
---
# 配置环境

## 安装node.js
1. 登陆 [node.js 官网](https://nodejs.org/en/)，根据对应的操作系统，选择相应的版本
2. 安装node.js
3. 在命令行里, 运行`node -v`, 若有版本信息，则安装成功， 反之亦然。

## 安装hexo

1. 运行 `npm install -g hexo-cli`， 安装hexo
2. 运行 `hexo -v`, 若有版本信息，则安装成功， 反之亦然。
3. 运行 `npm install hexo-deployer-git --save`, 安装github上传包

## 安装git
1. 登陆[git下载网址](https://git-scm.com/downloads), 根据对应的操作系统，选择相应的版本
2. 安装git
3. 在命令行里, 运行`git --version`, 若有版本信息，则安装成功， 反之亦然。

## github 
1. 注册账号
2. 建立一个Repositories，用于存放博客静态文件，名称格式最好为：`用户名.github.io`
3. 设置远程，`git remote add remoteName url`, url 为 Repositories 选项`setting`中`Github Pages`提示的`url`
4. 查看远程设置，`git remote -v`
5. 重新配置，`git remote set-url remoteName url`

# 初次搭建博客

## 创建本地文件
1. hexo init [blogsName]
2. cd yourpath/blogsName
3. 启动本地服务器，hexo s or hexo server
4. 在浏览器中输入，localhost:4000， 查看有无界面， 有则成功

## 配置_config.yml
1. 修改`site`字段，配置`language`，`author`，`title`，`description`, 支持的语言在[language option](https://github.com/iissnan/hexo-theme-next/wiki/%E8%AE%BE%E7%BD%AE%E8%AF%AD%E8%A8%80)
2. 修改`url`字段，配置 `url` 和 根目录 `root`
3. 修改`Extension`字段，配置`theme`，默认是landscape
4. 修改`deploy`字段，配置`type`, `repository`, `branch`

示例如下：
``` 
# Site
title: Hexo
subtitle:
description: keep hungry and practice more
keywords:
author: Jun Fu
language: en
timezone:

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://github.com/SunshineJunFu/JunFu.github.io
root: /JunFu.github.io/
permalink: :year/:month/:day/:title/
permalink_defaults:

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: landscape

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git 
  repository: https://github.com/SunshineJunFu/JunFu.github.io
  branch: master

```

## 上传至github
1. 运行`hexo g -d`
2. 点击对应Repositories 选项`setting`中`Github Pages`提示的`url`，查看效果

# hexo 命令

1. `hexo init blogsName`, 创建文件夹
2. `hexo generate`，可简写为`hexo g`, 生成网页静态文件 
3. `hexo server`，可简写为`hexo s`, 创建本地服务器
4. `hexo deploy`，可简写为`hexo d`, 上传至github
5. `hexo clean`, 清理缓存
6. `hexo new post Name`， 创建博客
7. `hexo new page Name`，  创建网页

更多信息，可以参考[hexo writing](https://hexo.io/zh-cn/docs/writing.html)

