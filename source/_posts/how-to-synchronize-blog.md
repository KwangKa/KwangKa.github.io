title: 不同电脑上同步Hexo Blog
date: 2015-01-17 23:11:36
tags: [随笔]
---

前天在实验室弄好这个Blog, 后来回到宿舍就有个问题: 怎么在宿舍更新我的博客, 然后同步上去? 难道每次都要到实验室电脑上写博客?这也太不方便了. Google一下, 找到了这个[解决办法](http://youthyblog.com/2014/06/28/%E4%BD%BF%E7%94%A8github%E7%AE%A1%E7%90%86hexo%E6%9C%AC%E5%9C%B0%E6%96%87%E4%BB%B6/), 思路就是在github上再新建一个repository, 用来同步博客.

假定在电脑A上成功搭建了博客, 也同步到了github上的仓库username.github.io 现在想在电脑B上也能更新博客, 并同步到github上. 过程如下:

首先, 在github上再键一个repository, 假定为HexoBlog. 进到电脑A上的博客的文件夹. 如果你的博客主题是从github上克隆下来的, 要把主题文件夹里的.git文件夹删掉. 以我自己的为例, 我用的主题是[yilia](https://github.com/litten/hexo-theme-yilia), 用 ls -la命令就看到 thems/yilia/ 下面的隐藏文件夹.git. 然后删掉.
```Bash
ls -la themes/yilia
rm -rf themes/yilia/.git
```
<!-- more -->
好了, 接下来用github同步整个博客文件夹
```Bash
git init
git add .
git commit -m "First commit"
```

这就完成了在电脑A上用git对博客文件夹进行管理, 接下来同步到刚刚新建的repository, 也就是HexoBlog
```Bash
git remote add origin git@github.com:username/HexoBlog.git
```
上面username换成你自己的github用户名, HexoBlog换成刚刚新建的repository名字. 最后, push到github.
```Bash
git push origin master
```

<br />
上面是在电脑A上做的工作,接下来回到电脑B. 首先, 像在电脑A上搭建博客一样, 要在B上安装git, node.js, hexo 安装过程自己Google(既然在A上搭好博客了,安装过程应该会了). 然后把github上的repository克隆到电脑B上
```Bash
git clone git@github.com:username/HexoBlog.git
```
按照Blog目录下自带的.gitignore文件, node_modules文件夹是不会同步的, 所以同步之后需要自己再次进行
``` Bash
npm install
```

这样, 在电脑B上写了新文章后, 更新到github上的 username.github.io
``` Bash
hexo g
hexo d
```

然后, 更新到github上的 HexoBlog (即刚刚新建的用来同步整个博客文件夹的repository)
``` Bash
git add .
git push
```

这样, 就可以在不同电脑上同步博客了.

hexo上有两个issue可以参考
[issue527](https://github.com/hexojs/hexo/issues/527)
[issue752](https://github.com/hexojs/hexo/issues/752)

@至善园
