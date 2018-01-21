---
title: ubuntu
date: 2016-11-16 13:11:18
tags: ubuntu
---
## ubuntu16.04安装后的一些优化

记录一些unbuntu安装后的调整优化工作和各种开发环境搭建工作

### 调整启动器

将默认的左栏调整到底部
``` bash
gsettings set com.canonical.Unity.Launcher launcher-position Bottom
```

### 卸载"Amazon"链接
在启动器删掉，然后执行
``` bash
sudo apt-get remove unity-webapps-common
```
<!-- more -->

### 卸载"LibreOffice"办公软件
不太方便，还是使用wps比较好
``` bash
sudo apt-get remove libreoffice-common"
```
### 删除很多几乎不用的软件
``` bash
sudo apt-get remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku  landscape-client-ui-install
```

### 安装"Oracle Java"
``` bash
sudo add-apt-repository ppa:webupd8team/java    
sudo apt-get update    
sudo apt-get install oracle-java8-installer 
```

### 安装"git"
``` bash
sudo apt-get install git
```

### 安装"vim"
``` bash
sudo apt-get install vim
```

### 安装"sublime text3"
``` bash
sudo add-apt-repository ppa:webupd8team/sublime-text-3    
sudo apt-get update    
sudo apt-get install sublime-text 
```
### 安装"unrar"
常用的解压软件
``` bash
sudo apt-get install unrar
```

### 安装"cmake和qtcreator"
``` bash
sudo apt-get install cmake qtcreator
```
### 安装"android studio"
先去官网下载最新安装包IDE，然后解压到一个目录，我解压到了`/usr/local/`，然后可以在`PATH`中加入启动命令
``` bash
sudo vim /etc/profile
```
注意这是修改的个人用户的全局变量，然后可以
``` bash
source /etc/profile
```
更新一下该文件

安装完android studio后会提示下载SDK，可以用代理快很多，另外64位的ubuntu要安装32支持包
``` bash
sudo apt-get install lib32z1 lib32ncurses5 lib32bz2-1.0 lib32stdc++6
```
按照官方文档会提示找不到该库，需要删掉其中一个`lib32bz2-1.0`，然后单独安装
``` bash
sudo apt-get install lib32z1 lib32ncurses5 lib32stdc++6
```
单独安装命令
``` bash
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install lib32bz2-1.0:i386
```

## 安装Macbuntu主题
链接地址[macbuntu](http://www.noobslab.com/2016/04/macbuntu-1604-transformation-pack-for.html)
macbuntu是一套主题图标和各种字体plank的一套素材，能让你的ubuntu非常接近mac osx的外观。


## ubuntu下的一些问题

无法挂载ntfs的格式的分区
使用了ntfsfix命令修复了这个问题。

	ntfsfix repairs some fundamental NTFS inconsistencies, resets the NTFS journal file and schedules an NTFS consistency check for the first boot into Windows.

``` bash
sudo ntfsfix /dev/[yourdevice partion]
```
