---
title: 记一次使用IDEA部署SSM项目时遇到的问题
---

> 寒雨连江夜入吴，平明送客楚山孤。洛阳亲友如相问，一片冰心在玉壶。 《芙蓉楼送新辛渐》—— 王昌龄

* * *

本文介绍我在一次导入SSM项目时遇到的问题。（踩坑记录）

#### IDEA导入SSM项目

* * *

我要导入的项目是一个Github上的一个聊天室项目 [Github链接](https://github.com/MrLiu1227/WebChat) 首先import项目，选择Maven项目，勾选Import Maven projects automatically选项以及Automatically download的两个选项
 <!--more-->
![Maven](https://s1.ax1x.com/2020/04/10/GoJLTA.png)

等待Maven依赖下载完成后开始配置Tomcat。 点击Edit Configurations，并点击 “+” 配置Tomcat，在Deployment页面点击 ‘’+’’ 配置项目，选择WebChat:war exploded即可。

![Maven2](https://s1.ax1x.com/2020/04/10/GoJqwd.png)

#### 导入SQL脚本

* * *

导入SQL脚本比较简单，通过IDEA配置数据源即可在IDE环境中自动导入SQL脚本

#### Mysql依赖版本问题

* * *

这里遇到了一些坑。

由于本机安装的是Mysql8.0，而Maven依赖中的是Mysql5，所以需要更改版本号。在pom.xml中找到mysql项，更改版本为8.0.11。 更改完毕还未结束。Mysql8.0版本需要重新配置数据连接池，将驱动Driver更改为

`jdbc.driver=com.mysql.cj.jdbc.Driver` 以适配8.0版本，这里与5.0+版本不同

另外需要更改jdbc.url的时区和SSL设置 TimeZone设置为UTC，useSSL设置为false

`jdbc.url=jdbc:mysql://127.0.0.1:3306/webchat?characterEncoding=UTF-8&useSSL=false&serverTimezone=UTC`

这样就可以正常启动项目了。