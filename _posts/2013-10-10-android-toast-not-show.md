---
layout: post
title: 'Android toast 不显示'
categories:
- Android
tags:
- Android

---
问题很简单Toast.show()不显示toast，一开始以为是UI线程问题后来检查了发现在oncreate方法里调用也不弹出toast，但其他项目能正常显示很纠结。各种方法找尽了最后定位为手机问题。后来在网友的提醒发现原来是设置里面显示通知关闭了，开启后一切正常。设置见下图

![Alt text](http://xjliao.qiniudn.com/img/toast_no_show.png)

 参考资料:<http://blog.sina.com.cn/s/blog_783ede030101hj4r.html>