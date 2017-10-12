---
layout: '[poto]'
title: 搭建博客（三）：使用hexo，如果换了电脑怎么更新博客？
date: 2017-07-23 18:22:07
tags: [git,hexo]
categories: [建博客]#文章分类目录，可以为空，注意:后面有个空格
---

A:[使用hexo，如果换了电脑怎么更新博客？][6] 
Q:
## 方法一：建branch【[CrazyMilk的回答][7]】：
作者：CrazyMilk
链接：https://www.zhihu.com/question/21193762/answer/79109280
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

其实，Hexo生成的文件里面是有一个.gitignore的，所以它的本意应该也是想我们把这些文件放到GitHub上存放的。
但是考虑到如果每个GitHub Pages都需要额外的一个仓库存放这些文件，就显得特别冗余了。这个时候就可以用分支的思路！
一个分支用来存放Hexo生成的网站原始的文件，另一个分支用来存放生成的静态网页。
最近我也用GitHub Pages搭建了一个独立博客，想到了这个方法，使用之后真的特别简洁。
为了更直观地说明，奉上使用这种方法不同时候的流程：
------------------------  华丽的分割线1 --------------------------------
### 一、关于搭建的流程
1. 创建仓库，http://CrazyMilk.github.io；
2. 创建两个分支：master 与 hexo；
3. 设置hexo为默认分支（因为我们只需要手动管理这个分支上的Hexo网站文件）；
4. 使用git clone git@github.com:CrazyMilk/CrazyMilk.github.io.git拷贝仓库；
5. 在本地http://CrazyMilk.github.io文件夹下通过Git bash依次执行npm install hexo、hexo init、npm install 和 npm install hexo-deployer-git（此时当前分支应显示为hexo）;
6. 修改_config.yml中的deploy参数，分支应为master；
7. 依次执行git add .、git commit -m "..."、git push origin hexo提交网站相关的文件；
8. 执行hexo g -d生成网站并部署到GitHub上。

这样一来，在GitHub上的http://CrazyMilk.github.io仓库就有两个分支，一个hexo分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页。完美( •̀ ω •́ )y！

### 二、关于日常的改动流程在本地对博客进行修改（添加新博文、修改样式等等）后，通过下面的流程进行管理。

1. 依次执行git add .、git commit -m "..."、git push origin hexo指令将改动推送到GitHub（此时当前分支应为hexo）；
2. 然后才执行hexo g -d发布网站到master分支上。

虽然两个过程顺序调转一般不会有问题，不过逻辑上这样的顺序是绝对没问题的（例如突然死机要重装了，悲催....的情况，调转顺序就有问题了）。
### 三、本地资料丢失后的流程当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：
1. 使用git clone git@github.com:CrazyMilk/CrazyMilk.github.io.git拷贝仓库（默认分支为hexo）；
2. 在本地新拷贝的http://CrazyMilk.github.io文件夹下通过Git bash依次执行下列指令：
npm install hexo、npm install、npm install hexo-deployer-git（记得，不需要hexo init这条指令）。----------------------------------------------------------  华丽的分割线2 ----------------------------------------------------------以上就是我采用的方法，虽说文字有点多，但是我个人觉得真的挺高效和简洁的。更详细的可以参考我刚写的博文：GitHub Pages + Hexo搭建博客。

## 方法二：复制粘贴【[skycrown的回答][8]】
作者：skycrown
链接：https://www.zhihu.com/question/21193762/answer/103097754
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

不知道题主是不是换了新电脑，需要在新电脑上进行部署，如果是，可以参考下面的方法：
### 1、从官网Git下载git，在新电脑上安装，因为https速度慢，而且每次都要输入口令，常用的是使用ssh。使用下面方法创建：
（1）打开git bash，在用户主目录下运行：ssh-keygen -t rsa -C "youremail@example.com" 把其中的邮件地址换成自己的邮件地址，然后一路回车
（2）最后完成后，会在用户主目录下生成.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH key密钥对，id_rsa是私钥，千万不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
（3）登陆GitHub，打开「Settings」->「SSH and GPG keys」，然后点击「new SSH key」，填上任意Title，在Key文本框里粘贴公钥id_rsa.pub文件的内容（千万不要粘贴成私钥了！），最后点击「Add SSH Key」，你就应该看到已经添加的Key。
注意：不要在git版本库中运行ssh，然后又将它提交，这样就把密码泄露出去了。
### 2、下载Node.js，并安装
### 3、打开git bash客户端，输入 npm install hexo-cli -g，开始安装hexo
### 4、下面就将原来的文件拷贝到新电脑中，但是要注意哪些文件是必须的，哪些文件是可以删除的。
（1）讨论下哪些文件是必须拷贝的：首先是之前自己修改的文件，像站点配置_config.yml，theme文件夹里面的主题，以及source里面自己写的博客文件，这些肯定要拷贝的。除此之外，还有三个文件需要有，就是scaffolds文件夹（文章的模板）、package.json（说明使用哪些包）和.gitignore（限定在提交的时候哪些文件可以忽略）。其实，这三个文件不是我们修改的，所以即使丢失了，也没有关系，我们可以建立一个新的文件夹，然后在里面执行hexo init，就会生成这三个文件，我们只需要将它们拷贝过来使用即可。总结：_config.yml，theme/，source/，scaffolds/，package.json，.gitignore，是需要拷贝的。
（2）再讨论下哪些文件是不必拷贝的，或者说可以删除的：首先是.git文件，无论是在站点根目录下，还是主题目录下的.git文件，都可以删掉。然后是文件夹node_modules（在用npm install会重新生成），public（这个在用hexo g时会重新生成），.deploy_git文件夹（在使用hexo d时也会重新生成），db.json文件。其实上面这些文件也就是.gitignore文件里面记载的可以忽略的内容。总结：.git/，node_modules/，public/，.deploy_git/，db.json文件需要删除。
### 5、在git bash中切换目录到新拷贝的文件夹里，使用 npm install 命令，进行模块安装。很明显我们这里没用hexo init初始化，因为有的文件我们已经拷贝生成过来了，所以不必用hexo init去整体初始化，如果不慎在此时用了hexo init，则站点的配置文件_config.yml里面内容会被清空使用默认值，所以这一步一定要慎重，不要用hexo init。
### 6、安装其他的一些必要组件，如果在node_modules里面有的，就不要重复安装了：
（1）为了使用hexo d来部署到git上，需要安装
npm install hexo-deployer-git --save
（2）为了建立RSS订阅，需要安装
npm install hexo-generator-feed --save
（3）为了建立站点地图，需要安装
npm install hexo-generator-sitemap --save
插件安装后，有的需要对配置文件_config.yml进行配置，具体怎么配置，可以参考上面插件在github主页上的具体说明
### 7、使用hexo g，然后使用hexo d进行部署，如果都没有出错，就转移成功了！


[6]: https://www.zhihu.com/question/21193762
[7]: https://www.zhihu.com/question/21193762
[8]: https://www.zhihu.com/question/21193762