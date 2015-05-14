---
layout: post
title: 'Failed to initialize Monitor Thread: Unable to establish loopback connection'
categories:
- Android
tags:
- Android

---

##Failed to initialize Monitor Thread: Unable to establish loopback connection

出现以下问题：  
eclipse弹出框错误或者console报错信息如下:  

Failed to initialize Monitor Thread: Unable to establish loopback connection  

使用命令:adb devices  
* daemon not running. starting it now on port 5037 *  
ADB server didn't ACK  
* failed to start daemon *  
error: cannot connect to dae  


##解决方案：
关闭防火墙