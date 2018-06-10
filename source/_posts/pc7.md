title: Python Challenge (Level 7)
date: 2015-01-21 10:40:51
tags: [Python]
---
[第7关](http://www.pythonchallenge.com/pc/def/oxygen.html)
依旧是一张图片， Page Source里没有什么提示信息。图片中间有一块灰度图像，答案应该就在这一块特别的图像里了。网上的答案提到用GIMP可以得到中间灰度图像的位置， 也可以观察出后面会用到的step=7, 也就是水平方向每7个像素点采样一次。可以看到垂直方向上的像素值是一样的，所以y坐标在[y_min, y_max]中取一个好了。 将这一块图像的像素转换为ASCII字符就可以得到通关信息。灰度图像的RGB三个chanel的值是一样的，所以取一个chanel就可以了，这里取得是R通道，也就是\[0\] 

获取图像的像素值用到 [Pillow](http://pillow.readthedocs.org/en/latest/reference/Image.html)模块
<!-- more -->

``` Python
from PIL import Image
# pos of gray part
x_min, x_max = 0, 609
y_min, y_max = 43, 53
step = 7

im = Image.open("oxygen.png")
msg = [chr(im.getpixel((x, y_min))[0]) for x in range(x_min, x_max, step)]
print ''.join(msg)
```

得到的信息为
> smart guy, you made it. the next level is [105, 110, 116, 101, 103, 114, 105, 116, 121]

再将得到的信息转一次
``` Python
msg = [105, 110, 116, 101, 103, 114, 105, 116, 121]
print ''.join([chr(i) for i in msg])
```

得到**integrity**， http://www.pythonchallenge.com/pc/def/integrity.html
