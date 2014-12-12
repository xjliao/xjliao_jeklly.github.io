---
layout: post
title: '玩 Raspberry'
categories:
- Raspberry
tags:
- Raspberry

---
官网:<http://www.raspberrypi.org/>
##1.安装
下载地址:http://www.raspberrypi.org/downloads/  
推荐下载RASPBIAN版本   
zip文件下载:<http://downloads.raspberrypi.org/raspbian_latest>   
种子下载:http://downloads.raspberrypi.org/

需要一个大一点的内存卡(8G以上最好,分区格式为fat32)  
假如你的内存卡电脑已被识别
(在mac上)

#####查看设备:

```console
diskutil list
 ```
 
记住设备号,下面的例子假设查看得到的设备为:disk1  

#####卸载设备

```console
diskutil unmountDisk /dev/disk1
```

#####刻入镜像

```console
sudo dd bs=4m if=2014-09-09-wheezy-raspbian.img of=/dev/risk1(这里第disk1替换为rdisk1原始设备号,刻录速度会更快)
``

等一会吧 刻录完成会有提示信息

##2.连接
在这里首页你要得有跟网线连接上路由了,而且路由设置了dhcp 

ssh默认开启,这里通过ssh连接上系统.  
账号:pi   密码:raspberry

ip查看这里可以通过进入路由器查看dhcp列表,会有一台叫raspberrypi的机器,对应的ip就是了.这样我们就可以连接上了
反正不管你用啥办法,都要搞到ip


