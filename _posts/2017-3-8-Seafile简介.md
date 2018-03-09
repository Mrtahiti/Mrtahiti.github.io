
# 个人网盘——**Seafile**搭建

**在阅读本文之前，请确保自己拥有VPS**


本文以 **Ubuntu**为例


***
## 目录
* **搭建需求**
* **Seafile 介绍**
* **搭建教程**
* **常见问题**
* **参考网站**


***


## **搭建需求**(此处与 **百度云盘** 进行对比)

### 1.去哪存放文件（不重要但是占用本地空间）？
容易被和谐、上传速度慢
### 2.如何分享本地文件(此处指 大体积文件)？
臭名昭著的限速、链接失效问题


***


## **Seafile介绍**
>Seafile 是一款开源的企业云盘，注重可靠性和性能。支持 Windows, Mac, Linux, iOS, Android 平台。支持文件同步或者直接挂载到本地访问。


### 个人使用体验：
#### 1.空间**四星**
这也可以说是一个缺点吧，取决于你的VPS空间的大小。我使用的是DO的20GB的VPS，所以不能存放太多的文件。一次性的文件记得删除就好，对于实际影响不是很大。~~如果你小姐姐太多，那还是买个移动硬盘吧。~~
#### 2.速度**五星**
基本是**满带宽**的，取决于你的**VPS线路速度**，不论下载还是上传，自己下载还是分享下载。

PS:视频文件支持在线预览（格式有限），不会压缩质量
###### **奇巧淫技**
使用用校园网的**IPV6**，可以达到10 MB/S

#### 综合来看，评分可以**五星半**


***


## **搭建教程**

为了不折腾，此处推荐**一键安装脚本**
```
wget https://raw.githubusercontent.com/helloxz/seafile/master/install_seafile.sh
chmod +x install_seafile.sh && ./install_seafile.sh
```
### 1.打开**Putty**，连接自己的**VPS**
![](http://ww1.sinaimg.cn/large/d8b30b42gy1fp5dum1bj5j20go0deac5.jpg)


![](http://ww1.sinaimg.cn/large/d8b30b42gy1fp5dvnwidnj20o10awdhi.jpg)
### 2.**VPS端设置**

**由于我的VPS已经安装了Seafile，不能进行实际操作**


以下图片来自[小Z博客](https://www.xiaoz.me/archives/8480)

#### 1.复制上述代码（Putty**右击粘贴**）


#### 2.选择1

![](http://ww1.sinaimg.cn/large/d8b30b42ly1fp6tr7ibihj20h50393yf.jpg)

#### 3.按下回车

![](http://ww1.sinaimg.cn/large/d8b30b42ly1fp6tr7ifngj20i207hjrh.jpg)

#### 4.起个名字，字母数字都可以

![](http://ww1.sinaimg.cn/large/d8b30b42ly1fp6tr7il7yj20ij05bwek.jpg)

#### 5.填入自己的VPS IP

![](http://ww1.sinaimg.cn/large/d8b30b42ly1fp6tr7icqzj20ik04wmxb.jpg)

#### 6.小白可以一路回车(**默认设置**就好)

![](http://ww1.sinaimg.cn/large/d8b30b42ly1fp6tr7jxlpj20i60gpq3e.jpg)


#### 7.设置管理员

![](http://ww1.sinaimg.cn/large/d8b30b42ly1fp6tr7kcf8j20ld060wem.jpg)

#### 8.VPS端设置完成

![](http://ww1.sinaimg.cn/large/d8b30b42ly1fp6tr7o6lmj20dx04dmx4.jpg)

### 3.**网页端设置**
#### 1.打开 http://IP:8000
**将IP替换为自己刚才填入的IP**
#### 2.填入管理员邮箱和密码
![](http://ww1.sinaimg.cn/large/d8b30b42ly1fp6tr7ogtoj20mb0ddjrx.jpg)
#### 3.界面介绍

![](http://ww1.sinaimg.cn/large/d8b30b42gy1fp5e0zso8oj21hc0qbgo4.jpg)
#### 4.打开私人资料库

![](http://ww1.sinaimg.cn/large/d8b30b42gy1fp5e1d0qvpj21hc0go768.jpg)
### 至此，Seafile就算设置好了。
IOS端与Win端可以自行下载客户端，不再赘述。


***


## 常见问题
### 1.Ubuntu密码输入键入不上？
在Ubuntu中，密码输入是**不可见的**。敲完**自信地按下回车**就可以了。
### 2.Ubuntu安装Seafile很慢？
在安装过程中，需要下载各种包。速度取决于VPS线路速度。




***


## 参考网站：
* [小Z博客-CentOS 7 一键安装 Seafile 搭建私有云存储](https://www.xiaoz.me/archives/8480)
* [少数派-安全、可靠、快速的“私人云盘”](https://sspai.com/post/42678)
* [Seafile官网](https://www.seafile.com/home/)
