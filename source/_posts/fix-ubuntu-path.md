title: Ubuntu环境变量(PATH)修复
date: 2015-08-25 11:26:56
tags: [随笔]
---

今天早上打开电脑，发现Ubuntu图形界面登录不了。在登录界面输入了密码后，画面闪一下又跳回登录界面。于是，切到换到命令行界面（同时按下Ctrl+Alt+F1），看看出了什么问题。

命令行界面能顺利登录，但是sudo/ls等命令都用不了，提示错误
> The command could not be located because '/usr/bin:/bin' is not included in the PATH environment variable.

显然，PATH出问题了。回想到昨天晚上安装了scala和spark，应该是配置PATH的时候出问题了。于是，尝试用vim来重新编辑/etc/profile，但是，vim命令也报错....

纠结，后来在网上找到解决方案，那就是用绝对路径：
``` bash
/usr/bin/sudo /usr/bin/vim /etc/profile
```
修复了/etc/profile文件后，再重启
``` bash
/usr/bin/sudo /sbin/reboot
```
重启后，又能重新登录图形界面了。
