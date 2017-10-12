---
layout: '[poto]'
title: git使用（二）：git 初学解决错误
date: 2017-07-24 15:48:58
tags: [git]
categories: [git]#文章分类目录，可以为空，注意:后面有个空格
---
## 1.git init 产生的目录解释
```bash
  error: src refspec master does not match any.
```
引起该错误的原因是，目录中没有文件，空目录是不能提交上去的
## 2.错误提示：
```bash
  error: insufficient permission for adding an object to repository database ./objects
```
服务端没有可写目录的权限
## 3.错误提示：
```
  fatal: remote origin already exists.
```
  解决办法：
```
  $ git remote rm origin
```
## 4.错误提示：
```
  error: failed to push som refs to ........
```
  解决办法：
```bash
  $ git pull origin master //先pull 下来 再push 上去
```
## 5.常用git命令的解释：
```bash
Git init //在当前项目工程下履行这个号令相当于把当前项目git化，变身！
git add .//把当前目次下代码参加git的跟踪中，意思就是交给git经管，提交到本地库
git add <file> //把当前文件参加的git的跟踪中，交给git经管，提交到本地库
git commit -m “…”//相当于写点提交信息
git remote add origin git＠github.com:ellocc/gittest.git //这个相当于指定本地库与github上的哪个项目相连
git push -u origin master //将本地库提交到github上。
git clone git＠github.com:ellocc/gittest.git  //将github上的项目down下来。
git fetch origin //取得长途更新，这里可以看做是筹办要取了
git merge origin/master //把更新的内容归并到本地分支/master
```
```bash
下面是删除文件后的提交
git status //可以看到我们删除的哪些文件
git add .   //删除之后的文件提交git经管。
git rm a.c //删除文件
git rm -r gittest //删除目次 
git reset --hard HEAD 回滚到add之前的状态
git diff比较的是跟踪列表中的文件和文件系统中文件的差别
```
## 6.删除github远程分支
如果不再需要某个远程分支了，比如搞定了某个特性并把它合并进了远程的 master 分支（或任何其他存放稳定代码的地方），可以用这个非常无厘头的语法来删除它：git push  [远程名] :[分支名]。如果想在服务器上删 
除 serverfix 分支，运行下面的命令：
```bash
  git push origin :serverfix
  To git@github.com:schacon/simplegit.git
  - [deleted] serverfix
```
  咚！服务器上的分支没了。注意origin后有空格。
## 7.Host
Q:

![出现如下问题][1]

A:
### 1.点开如下文件夹：
![此处输入图片的描述][2]
### 2.打开hosts文件，添加github的ip与网址即可
 ![此处输入图片的描述][3] 
 
## 8.Key
  
 Q: 
 ![此处输入图片的描述][4]
  
A:应该是ssh key过期了吧
[window下配置SSH连接GitHub、GitHub配置ssh key_百度经验][5] 

## 9.配置hexo时出现了一些奇怪的东西
```bash
	$ hexo g
    sh: hexo: command not found
```
解决方法：
添加环境变量（控制面板>系统和安全>系统>高级系统设置>环境变量）：
把C:\Users\Fancy(你的电脑用户名)\node_modules\hexo\bin添加到用户变量的PATH变量后面。
注意千万不要把原来的删掉，用”;”把它们隔起来，不然你会死的很惨。

## 10.如果输入
```bash
$ git remote add origin git@github.com:djqiang（github帐号名）/gitdemo（项目名）.git       
    提示出错信息：fatal: remote origin already exists.
```

解决办法如下：
```bash
    1、先输入$ git remote rm origin
    2、再输入$ git remote add origin    git@github.com:djqiang/gitdemo.git 就不会报错了！
```

## 11.当要push代码到git时，出现提示：
```bash
error:failed to push some refs to ...
Dealing with “non-fast-forward” errors
From time to time you may encounter this error while pushing:
1.	$ git push origin master  
2.	To ../remote/  
3.	 ! [rejected]        master -> master (non-fast forward)  
4.	error: failed to push some refs to '../remote/'  
```
问题（Non-fast-forward）的出现原因在于：git仓库中已经有一部分代码，所以它不允许你直接把你的代码覆盖上去。于是你有2个选择方式：
1，强推，即利用强覆盖方式用你本地的代码替代git仓库内的内容
```bash
git push -f
```
2，先把git的东西fetch到你本地然后merge后再push
```bash
$ git fetch
$ git merge
```
这2句命令等价于
```bash
$ git pull  
```

## 12.提交代码到服务器后发现git clone下来的有些目录是空的。
查看服务器的目录果然是空的。看git add .    后查看git  status 
```bash 
modified: xxx(modified content, untracked content)
```
大概意思是xxx目录没有被跟踪。那自然push上去的时候是空的了
解决办法：后来发现这主要是xxx目录下有一个.git 目录，可能是被人给你这个目录的时候里面有了.git目录。删除.git目录。重新git add .就可以了。
from:://http://blog.csdn.net/huguohu2006/article/details/7045052

## 13.Q:
![此处输入图片的描述][6]
A:
上面出现的 [branch "master"]是需要明确(.git/config)如下的内容
[branch "master"]
    remote = origin
    merge = refs/heads/master
这等于告诉git2件事:
1，当你处于master branch, 默认的remote就是origin。
2，当你在master branch上使用git pull时，没有指定remote和branch，那么git就会采用默认的remote（也就是origin）来merge在master branch上所有的改变
如果不想或者不会编辑config文件的话，可以在bush上输入如下命令行：
1.	$ git config branch.master.remote origin  
2.	$ git config branch.master.merge refs/heads/master  
之后再重新git pull下。最后git push你的代码吧。


  [1]: http://otl5jere6.bkt.clouddn.com/hostq.png
  [2]: http://otl5jere6.bkt.clouddn.com/hosta.png
  [3]: http://otl5jere6.bkt.clouddn.com/hosta2.png
  [4]: http://otl5jere6.bkt.clouddn.com/qkey.png
  [5]: http://jingyan.baidu.com/article/a65957f4e91ccf24e77f9b11.html
  [6]: http://otl5jere6.bkt.clouddn.com/git%E4%BD%BF%E7%94%A8%EF%BC%88%E4%BA%8C%EF%BC%89%EF%BC%9Agit%20%E5%88%9D%E5%AD%A6%E8%A7%A3%E5%86%B3%E9%94%99%E8%AF%AF-12%E5%9B%BE.png
