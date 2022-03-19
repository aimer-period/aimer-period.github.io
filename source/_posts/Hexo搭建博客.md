---
title: Hexo搭建博客
date: 2022-01-12 17:24:40
cover: https://cdn.jsdelivr.net/gh/aimer-period/pic/img/6t.png
tags: hexo
---

# 1、安装node.js

直接官网下载安装即可

[https://nodejs.org/zh-cn/[Node.js](https://nodejs.org/zh-cn/)](https://nodejs.org/zh-cn/%5BNode.js%5D(https://nodejs.org/zh-cn/))

安装后

```bash
node --v
npm --v
```

查看是否安装成功

# 安装hexo

找一个位置存放文件，我放的是`E:\Study\program\blog`

在该目录下右击，打开`Git Bash Here`

输入`npm i hexo-cli -g`安装Hexo。会有几个报错，无视它就行。

安装完后输入`hexo -v`验证是否安装成功。

然后就要初始化我们的网站，输入`hexo init`初始化文件夹，接着输入`npm install`安装必备的组件。

这样本地的网站配置也弄好啦，输入`hexo g`生成静态网页，然后输入`hexo s`打开本地服务器，然后浏览器打开`http://localhost:4000/`，就可以看到我们的博客啦，效果如下：

# 链接GitHub

打开博客根目录下的`_config.yml`文件，这是博客的配置文件，在这里你可以修改与博客相关的各种信息。

修改最后一行的配置：

```bash
deploy:
  type: git
  repository: https://github.com/aimer-period/aimer-period.github.io
  branch: main
```

repository修改为你自己的github项目地址。

# 写文章

首先在博客根目录下右键打开git bash，安装一个扩展`npm i hexo-deployer-git`。

然后输入`hexo new post "article title"`，新建一篇文章。

然后打开`E:\study\program\blog\source\_posts`的目录，可以发现下面多了一个文件夹和一个`.md`文件，一个用来存放你的图片等数据，另一个就是你的文章文件啦。

编写完markdown文件后，根目录下输入`hexo g`生成静态网页，然后输入`hexo s`可以本地预览效果，最后输入`hexo d`上传到github上。这时打开你的github.io主页就能看到发布的文章啦。
