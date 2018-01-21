---
title: Hexo入门
categories: 技术
tags: Node.js
---
楼主有一台国外的vps，之前搭过wordpress，但是访问延迟较大，又因为前段时间服务器系统搞坏了，重装了系统，打算把那台服务器完全用来搭VPN和SS了，不再放网站在上面了。于是，转入了Hexo。本博文完成以下目标，首先，安装hexo博客系统，然后安装主题，最后做一个域名绑定。楼主记性最近不好，虽然文章网上都能搜到，但是做个记载还是好的。

## Hexo安装


``` bash
$ npm install hexo-cli -g
$ hexo init [目录名]
$ cd [目录名]
$ npm install
$ hexo server
```

<!-- more -->
注意这个地方有个坑，楼主当时打完这段命令，然后按照提示用`4000`端口访问`localhost:4000`，发现怎么都加载不出来，百度了好久，发现是安装了福昕pdf阅读器的原因，太坑了！`4000`被占用了！所以，你可以使用如下命令换个端口号:
``` bash
$ hexo server -p [你要用的端口号]
```
详见：[server](https://hexo.io/docs/commands.html#server)

更多信息: [安装](https://hexo.io)

## Hexo相关命令

### 初始化项目

``` bash
$ hexo init [folder]
```

更多信息: [服务器](https://hexo.io/docs/server.html)

### 生成静态网页

``` bash
$ hexo generate
```

更多信息: [生成页面](https://hexo.io/docs/generating.html)

### 启动本地服务器

``` bash
$ hexo server
```

|选项|作用|
|------|------|
|`-p`,`--port`|指定端口号|
|`-s`,`--static`|仅仅加载静态文件|
|`-l`,`--log`|打开日志输出|

更多信息: [服务器](https://hexo.io/docs/server.html)

### 创建新文章(post)

``` bash
$ hexo new [layout] <title>
```
除了创建post（文章），还可以通过该命令创建自定义页面，

- 在博客根目录下：
``` bash
$ hexo new page xxxx
```
- 设置页面模板
``` html
	---
	title:xxxx
	layout:page  # 添加使用page模板
	---
```
- 修改菜单
``` html
	menu:
	  Home: /
	  Archives: /archives
	  About: /about/
	  xxxx: /xxxx/  # 确保与source/xxxx/index.md文件中的title一致
```

### 部署到站点

``` bash
$ hexo deploy
```
更多信息: [部署](https://hexo.io/docs/deployment.html)

Hexo主题虽然没有wordpress多，但也有相当量优秀的主题，楼主使用的这个主题叫[even](https://github.com/ahonn/hexo-theme-even)的主题,在此作分享。

## 域名绑定
首先，你得有一个域名。

然后，在`source`目录下新建`CNAME`文件，内容为要绑定的域名，例如`blog.riverhe.cn`。

然后，将域名解析到你的github-pages相对应的域名上来。

- 添加DNS解析记录类型CNAME
- 主机记录自己选择，我的是`blog`
- 记录值填写tribody.github.io（根据自己情况）
- 其他默认

如此便完成了域名绑定。