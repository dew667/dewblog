---
title: 在Linux上搭建我的hexo博客
---

这篇文章将会介绍我是如何在Linux上搭建hexo个人博客系统的，我用的操作系统是deepin Linux。

#### 1.安装Git

Git是目前广泛使用的分布式版本控制系统  
首先在Linux终端输入以下代码，检测是否已经安装了Git  
`git`  
安装Git方法如下:  
`sudo apt-get install git`
 <!--more-->
#### 2.安装Node.js

Node官网已经把linux下载版本更改为已编译好的版本了，我们可以直接下载解压后使用。  
用浏览器导航到[Node.js中文网](http://nodejs.cn/download/)下载对应的压缩包文件(选择下载Linux二进制文件x64这一项)  
![Node.js中文网下载页](https://s1.ax1x.com/2020/04/10/GoYSl8.png)  
下载完成后拷贝压缩包到/opt目录下并进行解压：  
`sudo cp 下载路径/node-v10.15.3-linux-x64.tar.xz /opt`  
`sudo tar xf node-v10.15.3-linux-x64.tar.xz`  
解压完成后解压文件的bin目录下包含了node、npm等命令，我们用ln命令来设置软连接:  
`ln -s /opt/node-v10.15.3-linux-x64/bin/npm /usr/local/bin/`  
`ln -s /opt/node-v10.15.3-linux-x64/bin/node /usr/local/bin/`  
执行node命令，查看版本，检查是否安装成功：  
`node -v`  
`npm -v`

#### 3.安装cnpm

这里为避免安装hexo过慢，采用淘宝npm镜像源，先利用npm安装cnpm  
先进入管理员模式，获取权限  
`sudo su`  
接下来进行安装:  
`npm install -g cnpm --registry==https://registry.npm.taobao.org`  
设置软连接：  
`ln -s /opt/node-v10.15.3-linux-x64/bin/cnpm /usr/local/bin/`  
安装完成，查看版本:  
`cnpm -v`

#### 4.安装hexo

下面利用cnpm安装hexo  
`cnpm install -g hexo-cli`  
设置软连接：  
`ln -s /opt/node-v10.15.3-linux-x64/bin/hexo /usr/local/bin/`  
查看版本，验证:  
`hexo -v`

#### 5\. 正式搭建博客

进入用户路径，创建blog目录(出现问题可删除该目录并重新建立)  
`mkdir blog`  
进入blog目录  
`cd blog`  
生成hexo博客  
`sudo hexo init`  
启动博客  
`hexo s`  
启动成功可在本地访问博客页面(localhost:4000)  
建立第一篇博客文章  
`hexo new "我的第一篇博客文章"`  
编辑文章，先进入文章目录blog/source/\_posts:  
`cd source/_posts`  
再使用markdown编辑器或vim编辑:  
`vim 我的第一篇博客文章.md`  
编辑完成按一下步骤重启博客  
`hexo clean`  
`hexo g`  
`hexo s`

#### 6.将博客部署到Github

首先注册Github账号  
建立一个新的仓库new repository  
注意repository name要与Github用户名一致  
如dew667.github.io  
![Github](https://s1.ax1x.com/2020/04/10/GoJzSf.png)  
安装deployer插件：  
`cnpm install --save hexo-deployer-git`  
设置配置文件\_config.yml:  
`vim _config.yml`  
找到＃Deployment,修改type项为git  
repo项设置为Github仓库地址如[https://github.com/dew667/dew667.github.io.git](https://github.com/dew667/dew667.github.io.git)  
添加branch项为master  
下面部署到github:  
`hexo d`  
成功后即可访问dew667.github.io查看博客页面

#### 7.更换hexo博客主题

这里选用yilia主题  
首先克隆到本地blog/themes/yilia目录  
`git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia`  
编辑blog目录下的\_config.yml  
更改theme项为yilia  
重新启动博客即可`hexo clean` `hexo g` `hexo d`  
针对yilia主题所有文章显示问题,根据主题提示用cnpm安装插件并修改\_config.yml文档后即可

* * *

以上就是我建立hexo个人博客的具体步骤，后续将探讨如何部署到云服务器上并使用新的域名。谢谢大家！