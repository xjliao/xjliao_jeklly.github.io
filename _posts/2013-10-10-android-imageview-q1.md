---
layout: post
title: 'Android中 ImageView 顶部,底部出现一段空白区间, 不填充'
categories:
- Android
tags:
- Android

---
1、在XML文件中设置：  

      android:adjustViewBounds="true"  
2、在Java代码中进行设置：  

      mImageView.setAdjustViewBounds(true);