---
title: setupBlog3
date: 2019-01-09 12:31:06
tags: [hexo]
---
# 如何方便的迁移hexo博客

解决方案是：在同一个Repositories，创建两个branch，master用于存放静态网页文件，hexo用于存放hexo代码。

## 创建远程分支hexo

1. `git clone url` 
2. `git brand -r`, 若存在hexo分支且想重新创建，可 `git branch -r -d origin/hexo` `git push origin :hexo`
3. `git checkout -b hexo`, 创建新hexo分支
4. 将上层目录的hexo文件全部拷贝至 xxx.io 文件下
5. `git add .`
6. `git commit -m "add hexo branch"`
7. `git push --set-upstream origin hexo` 在远端创建hexo分支并上传文件, 如果有hexo的话，`git push origin`
8. Repositories 已经存在两个branch了

## 从新的环境中开始

### 从远端复原

1. `git clone -b hexo https://github.com/SunshineJunFu/JunFu.github.io.git`
2. `cd xx.io`
3. `npm install hexo --save`
4. `npm install`
5. `npm install hexo-deployer-git`
6. `hexo s`， 检查复原效果

## 书写博客

1. `hexo new name`
2. `编辑 xx.md 文件`
3. `git add .`
4. `git commit -m something`
5. `git push origin`, 更新远端hexo文件，对应hexo branch
6. `hexo g -d`， 更新远端静态网页文件，对应master branch


