---
title: IOS APN Across The Great Fire Wall
tags:
  - proxy
categories:
  - Network
permalink: iOS-APN-across-the-great-fire-wall
date: 2016-01-26 00:00:00
---

##### 本文目的是搭建全平台的代理，主要分类三类，一类是 WiFi 下使用 pac文件，一类是 iPhone 手机 3G/4G 条件下修改 APN，最后是 Android 设置通用代理(包括 WiFi 和 3G/4G)。

### 前言
用过几种付费代理，这之中体验最好的个人来看只有曲径了，速度很快，全平台支持，可惜的是曲径也逃脱不了被关的命运，这么好的服务没了，只能自己动手搭建了。但是现有的代理方式基本上都是倾向于 PC 端的，想着怎么才能实现曲径那样的全平台，各方搜索得到一些资料。

[曲径原理](http://www.duanzhihu.com/answer/6725567)：
>我晕，上面的两个答案都是在扯淡啊，什么 GAE 什么 goagent 啊。

>曲径是这样服务的：在阿里云租了好几台 VPS，然后在这些 VPS 上，进行防滥用以及翻的一些设置。

>由于用户直接连接的是阿里云的服务器，所以在全国各地使用曲径都会很快。

>而当连接到曲径的服务器之后，曲径会自动判断目标是国内 IP，还是国外 IP。如果是国内的话，则由阿里云的机房直接连接；如果是国外的话，则自动选择最快的加密通道为用户翻。当然，这些对用户来说都是透明的，用户只要把代理设成他们的 http 代理地址即可。

>提到 http 地址，怕有些朋友不懂。是这样的，曲径提供的都是个在线 pac 文件，在这个 pac 中，曲径定义了一些默认需要翻的域名，如果目标域名不在这些域名中，则不走曲径的代理。打开这个 pac 文件就可以看到那个 http 代理地址了。

>给用户暴露的那个 http 地址可以解析出多个阿里云的机房来，可以自己 nslookup 试试。

>至于上面提到的防滥用，是防止恶意人士去批量扫描曲径的代理端口，随便一扫，曲径就会封你 IP 。

>目前看来，曲径的速度确实相当快，但是也贵。

p.s. 本来在知乎上之前是看到过这个的，但是再过几天发现所有曲径话题全部被删除了，还好找到一个备份的......

曲径的原理和我在上一篇中第二种代理方式差不多，使用了国内 vps，所以速度才会那么快。

### 最简单方式
当时知道曲径原理后，第一时间想到的就是 `shadowsocks`，为什么呢？因为它本身就是`ssserver`和`sslocal`两部分，这样 `sslocal`跑在国内 vps，`sserver`跑在国外 vps 上，然后手机 wifi 设置那里填入 pac 文件地址就行，这样也是一种方式，但是这实际是`socks5`代理，手机 app 很多不支持这种方式，结果一些 app 用不了。想到 `socks5` 转 `http`，这样的工具有 `Polipo`, `ProxyChain`, `Privoxy`，但是我用了之后效果都不是很好，`ProxyChain`这个不知道是什么原因成功率非常低，后面在 Github 上发现了一个神器，于是有了今天这篇文章。

---------
### 搭建所需要用到的东西

- 一台国外服务器，用来访问国外资源
- 一台国内服务器，用来做分流作用
- [cow](https://github.com/cyfdecyf/cow) 神器，配合`shadowsocks`完美！
- [APNGenerator](https://github.com/mapleray/APNGenerator) 用于生成 iPhone 的描述文件

>COW 的设计目标是自动化，理想情况下用户无需关心哪些网站无法访问，可直连网站也不会因为使用二级代理而降低访问速度。

`cow`本身就是一个提供 `http` 的代理，还带了自动化的 pac 文件。

### 国外服务器配置

国外服务器只用来做代理，最低配置就能满足要求，推荐 [DigitalOcean](https://m.do.co/c/8bcb79b8d4c8) 和 [Linode](https://www.linode.com/?r=1d3570963c5037174d082ef89f240a41379cd9d4)，操作系统建议 Ubuntu。
服务器只需要安装 [shadowsocks](https://shadowsocks.org/en/index.html) 就行。

```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python-dev
sudo pip install shadowsocks
```	
新建一个 `shadowsocks.json` 的文件，添加如下内容

```json
{
    "server":"your_server_ip",
    "server_port": 5151,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"your_password",
    "timeout":600,
    "method":"aes-256-cfb"
}
```

然后后台启动 `shadowsocks`:

```bash
ssserver -c /path/to/your/shadowsocks.json -d start
```

如果想停止服务的话，使用命令：

```shell
ssserver -c /path/to/your/shadowsocks.json -d stop
```

### 国内服务器

国内服务器可以选用阿里云，腾讯云或者美团云等。

#### 安装 cow

```bash
# 先安装 golang
sudo apt-get install golang
# 再安装 cow
curl -L git.io/cow | bash
```
#### 编辑 `~/.cow/rc` 文件

因为是放在国内服务器上面，在 `listen`那里需要把地址改为`0.0.0.0`, 使用`shadowsocks`作为二级代理，里面填写在服务器对应的配置信息，部分配置如下

```
#开头的行是注释，会被忽略
# 本地 HTTP 代理地址
# 配置 HTTP 和 HTTPS 代理时请填入该地址
# 若配置代理时有对所有协议使用该代理的选项，且你不清楚此选项的含义，请勾选
# 或者在自动代理配置中填入 http://127.0.0.1:7777/pac
listen = http://0.0.0.0:7777

# SOCKS5 二级代理
proxy = socks5://127.0.0.1:1080
# HTTP 二级代理
proxy = http://127.0.0.1:8080
proxy = http://user:password@127.0.0.1:8080
# shadowsocks 二级代理
proxy = ss://aes-256-cfb:your_password@your_server_ip:server_port
# cow 二级代理
proxy = cow://aes-128-cfb:password@1.2.3.4:8388
```

#### 启动cow
启动 cow 可以使用 `./cow &` 或者用 `tmux` 单开窗口执行。

启动后， pac 的文件地址是 http://YOUR_INSIDE_SERVER_IP:7777/proxy.pac

-------------------

到目前，在 iOS 手机 Wi-Fi 里面设置 pac 代理便可使用， 想用 APN 方式还需要 [APNGenerator](https://github.com/mapleray/APNGenerator), 在代理那项里填入 `YOUR_INSIDE_SERVER_IP`，端口为 `7777` ，用 `safari` 访问或者直接生成再安装到手机就能畅快的玩耍了。

如果不想用或者代理发生变化，直接在`设置`-`通用`-`描述文件` 里把之前安装的删除即可




