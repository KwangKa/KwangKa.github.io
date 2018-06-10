title: Python Challenge (Level 8)
date: 2015-01-22 23:05:40
tags: [Python]
---

[第8关](http://www.pythonchallenge.com/pc/def/integrity.html)
图片上是蜜蜂采蜜, 网页的标题是"work hard?"。 点击图片会要求输入用户名和密码. 查看Page Source, 下面有两行提示信息, 可以看出应该是username(un)和password(pw), 但看不出内容是什么.

网上的大神解释说(work hard? ==> busy)，要用[bz2](https://docs.python.org/2/library/bz2.html)模块。解压两个字符串可以得到信息
``` Python
import bz2
un = 'BZh91AY&SYA\xaf\x82\r\x00\x00\x01\x01\x80\x02\xc0\x02\x00 \x00!\x9ah3M\x07<]\xc9\x14\xe1BA\x06\xbe\x084'
pw = 'BZh91AY&SY\x94$|\x0e\x00\x00\x00\x81\x00\x03$ \x00!\x9ah3M\x13<]\xc9\x14\xe1BBP\x91\xf08'

print bz2.decompress(un), bz2.decompress(pw)
```

<!-- more -->
得到用户名 **huge**, 密码 **file** , 输入后跳到http://www.pythonchallenge.com/pc/return/good.html
