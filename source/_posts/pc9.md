title: Python Challenge (Level 9)
date: 2015-01-23 10:32:31
tags: [Python]
---
[第9关](http://www.pythonchallenge.com/pc/return/good.html)
图片上散落着一些黑点， 网页title提示**connect the dots**. 看来还是要用到图像处理模块——[Pillow](https://pillow.readthedocs.org/index.html)

Page Source里有first和second两组数据，看起来似乎是两组坐标[x1, y1, x2, y2, ...]. Pillow里面的**[ImageDraw](https://pillow.readthedocs.org/reference/ImageDraw.html)**有Draw.line方法， 参数格式与first, second的格式吻合。
<!-- more -->

``` Python
# -*- coding:utf-8 -*-
from PIL import Image, ImageDraw

first = [146,399,163,403, ...]   #限于篇幅， 省略了坐标数据
second = [156,141,165,135, ...]
im = Image.open('good.jpg')

newIm = Image.new('L', im.size)  #新建8bit黑白图像，默认填充黑色
draw = ImageDraw.Draw(newIm)
draw.line(first, fill=255)       #画白色线条
draw.line(second, fill=255)
newIm.show()
```

画出来是一头牛的轮廓，输入cow， 提示 *hmm. it's a male.*， 得到最终答案——bull， http://www.pythonchallenge.com/pc/return/bull.html
