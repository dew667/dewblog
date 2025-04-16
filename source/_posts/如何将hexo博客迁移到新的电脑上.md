---
title: 如何将hexo博客迁移到新的电脑上
---

最近购入了一台新的笔记本电脑。现在面临将我的hexo博客迁移到新的笔记本电脑上的问题。  
我需要将原来电脑Linux系统中的博客转移到新电脑上，并且我的新电脑也已经装好了Linux环境，只不过是在Windows10上的Linux子系统（WSL）。

#### 1.在新电脑Linux环境下安装必要软件

参照我的[上一篇文章](http://dew667.xyz/2019/05/25/%E5%9C%A8Linux%E4%B8%8A%E6%90%AD%E5%BB%BA%E6%88%91%E7%9A%84hexo%E5%8D%9A%E5%AE%A2/)正确安装Git、Nodejs、hexo等  
在`/home/username`目录下新建博客文件夹`mkdir blog`
 <!--more-->
#### 2.拷贝hexo博客文件

进入blog目录`cd /home/username/blog`  
初始化博客`hexo init`  
拷贝原来电脑的blog文件下的这些文件到新电脑的blog目录覆盖：

    scaffolds    文章模板
    source       博客文章
    themes       主题
    .gitignore
    _config.yml  站点配置文件
    package.json 安装包的名称

拷贝这些文件的方式有：

-   通过网盘传，需要将blog文件打包后上传到网盘，避免文件数上传超过限制
    
-   用Linux的scp命令传文件，需要处在同一局域网
    
-   用U盘拷贝，涉及到Linux设备挂载
    

#### 3.重新安装插件

首先初始换git本地仓库  
`git init`  
安装上传插件  
`cnpm install hexo-deployer-git --save`  
安装其他插件，如我用的yilia主题中所有文章目录显示需要的插件

#### 4.完成迁移

重启博客即可