---
layout: '[poto]'
title: git使用（一）：常见的git命令
date: 2017-07-23 22:35:28
tags: [github,hexo,技术,静态独立博客]
categories: [git]
---
## 一. 上传项目(github)


根据如下步骤来

add     添加 
commit  确认添加  
push    提交到git

touch README.md //新建一个记录提交操作的文档
git init //初始化本地仓库
git add README.md //添加
git add .  //加入所有项目
git status //检查状态 如果都是绿的 证明成功
git commit -m "first commit"//提交到要地仓库，并写一些注释
git remote add origin git@github.com:youname/Test.git //连接远程仓库并建了一个名叫：origin的别名
git push -u origin master //将本地仓库的东西提交到地址是origin的地址，master分支下
Git init //在当前项目工程下履行这个号令相当于把当前项目git化，变身！

git clone git＠github.com:ellocc/gittest.git  //将github上的项目down下来。
git fetch origin //取得长途更新，这里可以看做是筹办要取了
git merge origin/master //把更新的内容归并到本地分支/master

下面是删除文件后的提交
 
git status //可以看到我们删除的哪些文件
git rm a.c //删除文件a.c
git rm -r gittest //删除目次
git reset --hard HEAD 回滚到add之前的状态 git diff比较的是跟踪列表中的文件和文件系统中文件的差别

---


## 二. 写博客(hexo)


hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
//新建页面之后可以在页面内修改，写博客。
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub

# 简写

hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy

---

## 三. 部署
将修改后的内容部署至GitHub（部署三步骤）：

1.hexo clean
2.hexo generate
3.hexo deploy

