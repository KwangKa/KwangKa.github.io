title: Python Challenge (Level 6)
date: 2015-01-20 11:28:54
tags: [Python]
---

[第6关](http://www.pythonchallenge.com/pc/def/channel.html)

这一关挺有意思的, 绕了好几重。 图片上面是一条拉链, Page Source里面有*zip*的提示。所以这一关会用到[zipfile](https://docs.python.org/2/library/zipfile.html)模块。 

把URL里的channel换成zip，结果出现这个
> yes. find the zip

网上搜一下答案， 原来是要把html改成zip, 再下载[zip文件](http://www.pythonchallenge.com/pc/def/channel.zip)。下载得到的zip文件里有个readme.txt
> welcome to my zipped list.

> hint1: start from 90052
> hint2: answer is inside the zip

<!-- more -->
90052.txt里面是 **Next nothing is 94191**， 看来有点像urllib那一关， 不断寻找下一个txt
``` Python
import re
import zipfile

next_num = '90052'
ext = '.txt'

zf = zipfile.ZipFile('channel.zip')
pat = re.compile('Next nothing is (\d+)')

while True:
    text = zf.read(next_num + ext)
    print text
    next_num = pat.findall(text)
    if next_num:
        next_num = next_num[0]
    else:
        break
```
<br>
看到最后一个文件的提示 **Collect the comments.** 看来还要把每个文件的comment弄出来。
``` Python
import re
import zipfile

next_num = '90052'
ext = '.txt'

zf = zipfile.ZipFile('channel.zip')
pat = re.compile('Next nothing is (\d+)')
comment = ""
while True:
    text = zf.read(next_num + ext)
    zinfo = zf.getinfo(next_num + ext)
    comment += zinfo.comment
    print text
    next_num = pat.findall(text)
    if next_num:
        next_num = next_num[0]
    else:
        break

print comment
```

打印出来是这个样子的
``` Python
'''
****************************************************************
****************************************************************
**                                                            **
**   OO    OO    XX      YYYY    GG    GG  EEEEEE NN      NN  **
**   OO    OO  XXXXXX   YYYYYY   GG   GG   EEEEEE  NN    NN   **
**   OO    OO XXX  XXX YYY   YY  GG GG     EE       NN  NN    **
**   OOOOOOOO XX    XX YY        GGG       EEEEE     NNNN     **
**   OOOOOOOO XX    XX YY        GGG       EEEEE      NN      **
**   OO    OO XXX  XXX YYY   YY  GG GG     EE         NN      **
**   OO    OO  XXXXXX   YYYYYY   GG   GG   EEEEEE     NN      **
**   OO    OO    XX      YYYY    GG    GG  EEEEEE     NN      **
**                                                            **
****************************************************************
 **************************************************************
'''
```
好像就是这个**HOCKEY**了， 但是 http://www.pythonchallenge.com/pc/def/hockey.html 这里只有一句话
> it's in the air. look at the letters.

原来组成**HOCKEY**的字母才是答案——oxygen（氧，in the air).

