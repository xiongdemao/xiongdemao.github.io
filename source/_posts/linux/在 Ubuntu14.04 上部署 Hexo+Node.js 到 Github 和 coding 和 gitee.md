---
title: 在 Ubuntu14.04 上部署 Hexo+Node.js 到 Github 和 coding 和 gitee
date: 2017-10-06 20:33:30
tags: [笔记,Ubuntu]
categories: [Ubuntu] 
---
## 1.在 Ubuntu14.04 上部署 Hexo 到 Github
首先部署到github上，用下边的就行：
[在 Ubuntu14.04 上部署 Hexo 到 Github](http://www.leyar.me/create-a-blog-with-hexo-in-ubuntu/)

这里仅仅是将网页部署到github上了，下边部署到gitee和coding上。

## 2.首先是创建同名库问题
- github：username.github.io

- coding：username.coding.me 

- gitee（码云）：username 【这个直接就是用户名】 

然后在仓库中分别再创建一个分支`hexo`并设置为主分支，用来存储所有文件，而`master`用来存储生成的网页文件。

## 3.然后是部署到gitee和coding上
这里仅需重点修改`_config.yml`：
重点：
```
deploy:
  type: git
  repository:
      github: git@github.com:ShomyLiu/ShomyLiu.github.io.git
      coding: git@git.coding.net:shomyliu/shomyliu.git
      gitee: git@gitee.com:XiongDeMiao/xiongdemiao.git
  branch: master
```
然后是同终端多git帐号问题。

## 4.同终端多git帐号问题

先查看下我已经存在的 SSH keys：
```
$ ls -al ~/.ssh   #这个是查看已经存在的ssh keys；
    # Lists the files in your .ssh directory, if they exist
```
这是我这边的显示结果：
```
id_rsa				# 这是私钥
id_rsa.pub			# 这是公钥
github_rsa			# 这是给 github 生成的私钥，下面会讲到创建方法
github_rsa.pub 
known_hosts
config				# 这个文件就是重点，下面会讲到。
```
下面我们来开始给创建 SSH key 吧。
```
ssh-keygen -t rsa -C ' "your_email@example.com" ' 
#这里引号内改成你在 Github 上设置的邮箱，比如我的是 leyar.me@gmail.com
```
回车后会提示：
```
Generating public/private rsa key pair.
#提示生成密钥配对
```
继续回车，提示：
```
Enter file in which to save the key (/home/leyar/.ssh/id_rsa): /home/leyar/.ssh/github_rsa 
#注意！冒号后面这里官方帮助文档里是建议你直接回车，如果涉及多个 SSH keys，这个方法是不可行的。因此后面输入你想要创建的文件及其路径再回车。比如上面我输入的。 github_rsa 这个文件名你可以换别的。
```
然后出现提示：
```
Enter passphrase (empty 'for' no passphrase):		# 这里输入你想设置的密码，也可以直接回车
Enter same passphrase again:			# 再次输入密码，或者直接回车
```
官方文档强烈推荐设置密码,具体原因参考：[Working with SSH key passphrases](https://help.github.com/articles/working-with-ssh-key-passphrases/) 。

界面会提示成功：
```
Your identification has been saved in /home/leyar/.ssh/github_rsa.
Your public key has been saved in /home/leyar/.ssh/github_rsa.pub.
The key fingerprint is:
01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db leyar.me@gmail.com
```
接下来也是至关重要的一步，它保证了 GitHub 能否正确读取到你新建的密钥。
编辑 ssh 配置文件：
```
gedit  ~/.ssh/config
```
在文件里面输入如下配置信息：

```
#GitHub user(leyar.me@gmail.com)
Host ileyar.github.com				#这里修改成你自己的 host
HostName github.com
PreferredAuthentications publickey	# 这个貌似可以不需要的，暂时还没弄清楚。
IdentityFile ~/.ssh/github_rsa		# 这里填入你前面新建的密钥的路径
User git
```
输入完成后，保存，关闭。回到终端。

接下来看看你添加的 SSH key 是否在运行了。
```
ssh-add -l    #这个是检验是否运行ssh
2048 d9:1b:af:b4:8e:8d:28:bb:2e:b3:7c:ce:eb:83:c6:52 leyar@iLeyar (RSA)
2048 3e:22:25:65:0b:24:f5:51:43:27:8b:fd:91:3f:7f:85 leyar.me@gmail.com (RSA)
```
可以看到第二个就是我新建的，已经在运行了。如果没有出现，可以通过如下操作：
```
ssh-add ~/.ssh/github_rsa
```
接着就是把本地生成的 SSH key 拷贝到 GitHub 网页里面了。
打开 ~/.ssh/github_rsa.pub 文件，把里面的内容（公钥）复制出来。
```
gedit ~/.ssh/github_rsa.pub
```
登陆 Github ，点击 Add SSH key，”Title“ 随便填写，然后把你复制的内容，粘贴到 ”Key“ 里面。点击 Add key，OK！

检验结果的时刻到来：
```
ssh -T git@github.com  #这个是检验是否连上服务器
Hi iLeyar! You’ve successfully authenticated, but GitHub does not provide shell access.
```
这里代表已经配对成功了～ 

其中
github用：
```
ssh -T git@github.com
```
coding用：

```
ssh -T git@git.coding.net
```

gitee用：

```
ssh -T git@gitee.com
```
## 5.修改hexo文档
在以前的笔记里有说明。
修改之后就可以
```
hexo g
hexo s
hexo d
```
至此，`hexo g`一下可以同时在三个网址更新你blog。
## 6.把本地仓库传到github上的hexo主分支去
接下来我们要做的就是把本地仓库传到github上去，
可能遇到需要设置username和email，因为github每次commit都会记录他们
```
git config --global user.name your name
git config --global user.email your_email@youremail.com
```
 此时已经在github和coding中已经设置好了SSH。

一般是：
```
git add .
git commit -m "fist..."
git remote add origin git@git.coding.net:user/project.git 
git push -u origin hexo
```
但是这里要同时push三个，所以需要：用`git remote set-url`命令实现二者的同步
```
$git remote -v #查看当前远端仓库
```
```
$git remote add both git@git.coding.net:user/project.git
#添加一个名为 both 的远端
$git remote set-url --add --push both git@git.coding.net:user/project.git
# 为其添加 push 到 Coding 的 SSH 地址
$git remote set-url --add --push both git@github.com:user/repo.git
# 为其添加 push 到 GitHub 的 SSH 地址
```
```
$git remote -v #查看当前远端仓库
origin  git@git.coding.net:user/project.git (fetch)
origin  git@git.coding.net:user/project.git (push)
github  git@github.com:user/repo.git (fetch)
github  git@github.com:user/repo.git (push)
```

之后在推送的时候科研用git push both实现二者同步更新

注意出现：
```
To git@gitee.com:XiongDeMiao/xiongdemiao.git
 ! [rejected]        hexo -> hexo (fetch first)
error: 无法推送一些引用到 'git@gitee.com:XiongDeMiao/xiongdemiao.git'
提示：更新被拒绝，因为远程仓库包含您本地尚不存在的提交。这通常是因为另外
提示：一个仓库已向该引用进行了推送。再次推送前，您可能需要先整合远程变更
提示：（如 'git pull ...'）。
提示：详见 'git push --help' 中的 'Note about fast-forwards' 小节。
```
说明远程库有东西，可以：`git push -u both +hexo`强制push
```
kuo@kuo-Inspiron-7420:~/文档/blog/xiongdemao.github.io$ git push -u both +hexo
```

## 写在后面

按照以上的步骤就进行了 hexo 源文件的初次备份。
以后每次修改了内容之后，都可通过以下几条命令实现同步。
```
git add .
git commit -m "..."	 # 双引号内填写更新内容
git push both hexo	# 或者 git push
```
另外刚在 stackoverflow 上看到一个关于 git add . , git add -u 以及 git add -A 的区别。
```
git add -A	# stages **ALL**
git add .	# stages new and modified, **without deleted**
git add -u	# stages modified and deleted, **without new**
 
```





