title: Ubuntu 16.04 安装 uTorrent
date: 2017-10-06 20:33:30
tags: [笔记,Ubuntu]
categories: [Ubuntu] 
---

[Ubuntu 16.04 安装 uTorrent](http://blog.topspeedsnail.com/archives/5752)
uTorrent是小巧的BT下载软件，是BitTorrent开发的。
## 在Ubuntu上安装uTorrent
```
$ sudo apt-get update
$ sudo apt-get install libssl1.0.0 libssl-dev
```

从官网下载最新的uTorrent server：

64位系统：
```
$ wget http://download-new.utorrent.com/endpoint/utserver/os/linux-x64-ubuntu-13-04/track/beta/ -O utserver.tar.gz
```

解压文件：
```
$ sudo tar -zxvf utserver.tar.gz -C /opt/
```

更改目录权限：
```
$ sudo chmod 777 /opt/utorrent-server-alpha-v3_3/
```

创建/usr/bin的链接：
```
$ sudo ln -s /opt/utorrent-server-alpha-v3_3/utserver /usr/bin/utserver
```
启动uTorrent Server：
```
$ utserver -settingspath /opt/utorrent-server-alpha-v3_3/
```

	

运行之后，使用浏览器访问：http://your-ip-:8080/gui

用户名admin，密码为空：
