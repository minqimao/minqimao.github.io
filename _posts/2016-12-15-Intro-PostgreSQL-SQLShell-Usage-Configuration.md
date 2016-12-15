---
layout: post
title:  intro, postgresql sql shell usage configuration
date:   2016-12-15
categories: postgresql,sql,shell
permalink: Intro-Postgresql-sqlshell-usage-configuration
tags: postgresql,sql,shell

# author
author: minqi mao
---
  
---
  
## configuration
* system windows 10
* postgresql 9.6.1
* postgis 2.3.1

## postgresql 初步
安装完成后,sql shell 使用问题  
首先打开sql shell 如下图

![drawing](minqimao.github.io/images/postsimage/2016/20161215203129.png)

如果要进入,则输入‘[]’中的内容,如我们在冒号‘:’后面输入: local host  
等等一步步,进入  

![drawing](minqimao.github.io/images/postsimage/2016/20161215200048.png)

然后可以通过各种命令来查看postgresql中的各种项目.  

## 命令行(cmd)快速启动
在Windows 10 --> 开始菜单 --> 所有应用 --> P 排序中 --> sql shell  
这种方式显然很慢,通过环境变量,配置快捷方式,最终归根到runpsql.bat文件上.  
因此,把目录加入环境变量path当中.可以实现命令行(cmd)快速启动  

![drawing](minqimao.github.io/images/postsimage/2016/20161215203458.png)

## version
2016/12/15  --version 1  

---