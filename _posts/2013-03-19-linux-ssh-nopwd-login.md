---
layout: post
title: 'ssh 无密码登陆'
categories:
- Linux
tags:
- Linux

---
无密码登陆远程主机,需要将本地主机的公钥内容加入到远程主机的authorized_keys文件中  
本地生成公钥(注:此操作中如果没有相应目录或文件,请手动创建,并且你必须已经知道远程主机的账户密码)

```console
xjl:~ xjliao$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
xjl:~ xjliao$ scp ./.ssh/id_rsa.pub xjliao@106.186.19.59:/home/xjliao/
xjliao@106.186.19.59's password: 
id_rsa.pub                                                                                                                  100% 219
xjl:~ xjliao$ ssh 106.186.19.59
Last login: Wed Oct 22 09:25:00 2014 from 49.65.251.185
[xjliao@li539-59 ~]$ cat id_rsa.pub >> ~/.ssh/authorized_keys
[xjliao@li539-59 ~]$ chmod 700 ~/.ssh
[xjliao@li539-59 ~]$ chmod 600 ~/.ssh/authorized_keys
[xjliao@li539-59 ~] exit
xjl:~ xjliao$ ssh 106.186.19.59
Last login: Wed Oct 22 09:25:00 2014 from 49.65.251.185
```
哦了


