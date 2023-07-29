---
title: 将GitHub的分支作为hexo源文件的仓库
cover: https://cdn.jsdelivr.net/gh/aimer-period/pic/img/21.png
date: 2022-01-12 22:26:03
tags: hexo
---

# 1、建立仓库关联

```bash
cd blog
git remote #查询链接的仓库
```

我已经做好了仓库的关联，如果没有的话先在本地文件夹初始化，然后链接仓库

```bash
git init

git remote add origin https://github.com/aimer-period/aimer-period.github.io.git

git remote
```



# 2、提交文件

```bash
git add
git commit -m "源文件"
git push -u origin HEAD:分支名
#分支名改为你想建立的分支
```

​		我们提交之后仓库就会多出来一个分支，我们把他设为默认，以后就不用特地写分支了

之后修改就像正常的提交一样

​		至于这里为什么不先在 Github 上面手动建立分支，然后再在本地建立关联，是因为如果是远程手动建立分支会自动以 master 分支为模板建立一份一模一样的文件，而我们仓库里面 master 分支存的都是经过 hexo 编译的文件，跟源文件完全不一样，新建这样一个分支之后还要手动把里面的文件删掉，另一个原因是如果在远程手动建分支，你在本地还得手动用 git fetch origin 拉取远程分支的更新，然后再手动建立与分支的关联，比较麻烦



# 3、恢复

重装电脑后，或者在其它电脑上想修改博客：

1. 安装git；
2. 安装Nodejs和npm；
3. 使用`git clone https://github.com/aimer-period/aimer-period.github.io.git`将仓库拷贝至本地；
4. 在文件夹内执行以下命令`npm install hexo-cli -g`、`npm install`、`npm install hexo-deployer-git`。



# 4、hexo的源文件

这里说一下步骤4为什么只需要拷贝6个，而不需要全部：

1. `_config.yml`站点的配置文件，需要拷贝；
2. `themes/`主题文件夹，需要拷贝；
3. `source`博客文章的.md文件，需要拷贝；
4. `scaffolds/`文章的模板，需要拷贝；
5. `package.json`安装包的名称，需要拷贝；
6. `.gitignore`限定在push时哪些文件可以忽略，需要拷贝；
7. `.git/`主题和站点都有，标志这是一个git项目，不需要拷贝；
8. `node_modules/`是安装包的目录，在执行`npm install`的时候会重新生成，不需要拷贝；
9. `public`是`hexo g`生成的静态网页，不需要拷贝；
10. `.deploy_git`同上，`hexo g`也会生成，不需要拷贝；
11. `db.json`文件，不需要拷贝。

其实不需要拷贝的文件正是`.gitignore`中所忽略的。

