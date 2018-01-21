#hexo多终端协作

使用hexo搭建静态博客十分方便，但是每次hexo在发布博客的时候只会发布生成的静态文件，对于有多个终端博客写作来说，非常不方便。如果要做多终端同步协作的话，基本想法就是说把hexo源文件也同时都传到github上去。要么就是重新建一个repo，但更好的方式可以在原repo上建一个分支，即采用master+hexo两个分支，master存放博客静态资源，hexo存放hexo源代码。

##准备工作
安装node.js，git，hexo环境，对接好了本地hexo与远端github仓库。可以参考[Hexo官方文档](https://hexo.io/zh-cn/docs/)

##本地初始化工作
首先进入到博客根目录，执行以下命令（注意，hexo源文件中包含了一个.gitignore文件，所以直接添加当前目录`.`就行了。
```
git init
git add .
git commit -m "blog source files"
git checkout -b hexo
git remote add origin https://github.com/yourname/yourname.github.io.git
git push origin hexo
```

##另一终端初始化和同步
```
git clone -b hexo https://github.com/yourname/yourname.github.io.git  //将Github中hexo分支clone到本地
cd  yourname.github.io  //切换到刚刚clone的文件夹内
npm install    //注意，这里一定要切换到刚刚clone的文件夹内执行，安装必要的所需组件，不用再init
hexo new post "new blog name"   //新建一个.md文件，并编辑完成自己的博客内容
git add .  //经测试每次只要更新sorcerer中的文件到Github中即可，因为只是新建了一篇新博客
git commit -m "XX"
git push origin hexo  //更新分支
hexo d -g   //push更新完分支之后将自己写的博客对接到自己搭的博客网站上，同时同步了Github中的master
```

##不同终端之间的同步操作
```
git pull origin hexo  //先pull完成本地与远端的融合
hexo new post "new blog name"
git add source
git commit -m "XX"
git push origin hexo
hexo d -g
```

	参考自[http://blog.csdn.net/Monkey_LZL/article/details/60870891](http://blog.csdn.net/Monkey_LZL/article/details/60870891)