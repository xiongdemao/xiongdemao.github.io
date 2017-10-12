---
title: 搭建博客（二）：Hexo的安装和配置
date: 2017-07-22 13:59:43
categories: [建博客]
tags: [git,hexo]
---
# Hexo的安装和配置
## 1、下载：
  登录node.js官网（nodejs.org/en） ,下载和你的电脑系统相对应的版本
## 2、安装：
  这里以Windows7系统为例，直接默认安装即可。
  打开E:\hexo文件夹，鼠标右键选择Git Bash Here，依次执行以下命令：
```bash
	$ npm install hexo-cli -g
    $ hexo init
    $ npm install
```
  Hexo会自动在本地仓库（即E:\hexo）下下载搭建网站所需的所有文件。
接着执行命令：
```bash
	$ hexo g
    $ hexo s
```
  然后用浏览器访问http://localhost:4000/，此时，你应该看到了一个漂亮的博客了，当然这个博客只是在本地的，别人是看不到的，hexo3.0使用的默认主题是landscape。（这是hexo的本地预览功能）
## 3.上传本地仓库（即博客）到github
### 1、设置username和email： 因为github每次commit都会记录他们，依次执行命令：
```bash
	$ git config --global user.name "your name"  
    $ git config --global user.email "your_email@youremail.com"
   （your name替换为你的github用户名,your_email@youremail.com替换为你注册github账号时的邮箱）
```
### 2、编辑_config.yml文件： 编辑E：\hexo下的_config.yml文件（建议使用Notepad++）。在_config.yml文件的最下方，添加如下配置：
```bash
	deploy:
    type: git
    repository: git@github.com:username/username.github.io.git
    branch: master
    (username替换为你github的账号名称，注意：hexo的配置文件中任何”:”后面都是带一个空格的，否则会报错)
```
### 3、上传：
  将仓库部署到Github上，配置好_config.yml保存并关闭，为了能够使Hexo部署到GitHub上，需要安装一个插件，执行以下命令：
```bash
  $ npm install hexo-deployer-git --save
```
  然后，执行下列指令即可生成博客静态网页并完成部署：
```bash
	$ hexo g
    $ hexo d
```
如果上传成功，会显示INFO Deploy done: git，到此，就大功告成了，在浏览器中输入username.github.io（username替换为github的仓库名），就可以看到自己的博客了。









