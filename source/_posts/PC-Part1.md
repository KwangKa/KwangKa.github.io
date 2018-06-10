title: Python Challenge (Level 0-5)
date: 2015-01-18 11:27:25
tags: [Python]
---

[Python Challenge](http://www.pythonchallenge.com/)是一个解题游戏网站, 每解出一道题就会跳到下一题. 目前为止一共有33关. 虽说是Python Challenge, 其实用其他语言,比如Java, C++都是可以的.

[第0关](http://www.pythonchallenge.com/pc/def/0.html)
提示很明显了, 换一下URL地址. 把地址中的0换成 2^38.
``` Python
2 ** 38
```
得到第二关地址http://www.pythonchallenge.com/pc/def/274877906944.html
<!-- more -->
<br>
[第1关](http://www.pythonchallenge.com/pc/def/274877906944.html)
根据图片上的提示, 应该是要做一个映射. 再加上"think twice"这个提示, 应该可以猜出是平移映射两位. 这里用到string的translate来完成
``` Python
import string
intab = string.ascii_lowercase
outtab = string.ascii_lowercase[2 : ] + string.ascii_lowercase[0 : 2]
table = string.maketrans(intab, outtab)
"map".translate(table)
```
URL上的"map" translate后得到"ocr", 这就得到了第三关的地址 http://www.pythonchallenge.com/pc/def/ocr.html
这一关图片下面有一大段英文, translate后可以得到
> i hope you didnt translate it by hand. thats what computers are for. doing it in by hand is inefficient and that's why this text is so long. using string.maketrans() is recommended. now apply on the url.

<br>
[第2关](http://www.pythonchallenge.com/pc/def/ocr.html)
根据提示, 在网页源码里找到一大段字符串。里面写着“find rare characters in the mess bellow”, 所以要我们统计字符的个数, 找出出现次数少的字符。字符串太长, 就不贴上来了, 假定字符串存在msg里
``` Python
for c in set(msg):
    print c, msg.count(c)
# 得到
'''
1219
! 6079
# 6115
% 6104
$ 6046
& 6043
) 6186
( 6154
+ 6066
* 6034
@ 6157
[ 6108
] 6152
_ 6112
^ 6030
a 1
e 1
i 1
l 1
q 1
u 1
t 1
y 1
{ 6046
} 6105
'''
```
把只出现一次的那几个字母组合一下, 得到"equality", 替换URL里的"ocr"得到 http://www.pythonchallenge.com/pc/def/equality.html
<br>
[第3关](http://www.pythonchallenge.com/pc/def/equality.html)
图片信息依旧没有用， 图片下面英文提示找一个小写字母， 两边各有三个大写字母，注意"EXACTLY"。查看网页源码， 假定把一大堆字符存到msg这个变量
``` Python
import re
>>> pat = re.compile(r'[^A-Z][A-Z]{3}([a-z])[A-Z]{3}[^A-Z]')
>>> pat.findall(msg)
['l', 'i', 'n', 'k', 'e', 'd', 'l', 'i', 's', 't']
```
得到"linkedlist", 下一关URL http://www.pythonchallenge.com/pc/def/linkedlist.html
<br>
[第4关](http://www.pythonchallenge.com/pc/def/linkedlist.html)
点进去发现html换成php， http://www.pythonchallenge.com/pc/def/linkedlist.php
进去是一张图片， 查看page source， 有提示
> urllib may help. DON'T TRY ALL NOTHINGS, since it will never 
end. 400 times is more than enough.

看来是要用urllib这个库了。回到网页，点击图片, 得到信息 *and the next nothing is 44827* 再看看此时的URL，应该是要不断跳转, 不断更新 *nothing=* 后面的数字
``` Python
import re
import urllib

url_template = 'http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing={0}'
next_num = '12345'
pat = re.compile('the next nothing is (\d+)')

while True:
    response = urllib.urlopen(url_template.format(next_num))
    text = response.readlines()
    next_num = pat.findall(text[0])[0]
    print next_num
```
期间会出现两次错误
> (1)[Yes. Divide by two and keep going.](http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing=16044)
> (2)[There maybe misleading numbers in the text. One example is 82683. Look only for the next nothing and the next nothing is 63579](http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing=82682)

按照提示修改就可以继续了。最后得到 *peak.html*, 亦即 http://www.pythonchallenge.com/pc/def/peak.html

<br />
[第5关](http://www.pythonchallenge.com/pc/def/peak.html)
实在看不懂， 网上找了下， 原来用到 *pickle*  (peak hell sounds like Pickle -.-!)
查看Page Source， 把[banner.p](http://www.pythonchallenge.com/pc/def/banner.p)下载到本地，用pickle反序列化
``` Python
import cPickle
from pprint import pprint
with open('banner.p', 'rb') as inFile:
    data = cPickle.load(inFile)
pprint(data)

#得到这一堆东西
'''
[(' ', 95)],
 [(' ', 14), ('#', 5), (' ', 70), ('#', 5), (' ', 1)],
 [(' ', 15), ('#', 4), (' ', 71), ('#', 4), (' ', 1)],
 [(' ', 15), ('#', 4), (' ', 71), ('#', 4), (' ', 1)],
 [(' ', 15), ('#', 4), (' ', 71), ('#', 4), (' ', 1)],
 [(' ', 15), ('#', 4), (' ', 71), ('#', 4), (' ', 1)],
 [(' ', 15), ('#', 4), (' ', 71), ('#', 4), (' ', 1)],
 [(' ', 15), ('#', 4), (' ', 71), ('#', 4), (' ', 1)],
 [(' ', 15), ('#', 4), (' ', 71), ('#', 4), (' ', 1)],
...
'''
```
依旧看不穿, 网上大神又有话说了， print出来
``` Python
for item in data:
    print ''.join(i[0] * i[1] for i in item)

'''
              #####                                                                      ##### 
               ####                                                                       #### 
               ####                                                                       #### 
               ####                                                                       #### 
               ####                                                                       #### 
               ####                                                                       #### 
               ####                                                                       #### 
               ####                                                                       #### 
      ###      ####   ###         ###       #####   ###    #####   ###          ###       #### 
   ###   ##    #### #######     ##  ###      #### #######   #### #######     ###  ###     #### 
  ###     ###  #####    ####   ###   ####    #####    ####  #####    ####   ###     ###   #### 
 ###           ####     ####   ###    ###    ####     ####  ####     ####  ###      ####  #### 
 ###           ####     ####          ###    ####     ####  ####     ####  ###       ###  #### 
####           ####     ####     ##   ###    ####     ####  ####     #### ####       ###  #### 
####           ####     ####   ##########    ####     ####  ####     #### ##############  #### 
####           ####     ####  ###    ####    ####     ####  ####     #### ####            #### 
####           ####     #### ####     ###    ####     ####  ####     #### ####            #### 
 ###           ####     #### ####     ###    ####     ####  ####     ####  ###            #### 
  ###      ##  ####     ####  ###    ####    ####     ####  ####     ####   ###      ##   #### 
   ###    ##   ####     ####   ###########   ####     ####  ####     ####    ###    ##    #### 
      ###     ######    #####    ##    #### ######    ###########    #####      ###      ######
'''
```
这下终于像皮克那样看穿一切了. http://www.pythonchallenge.com/pc/def/channel.html
