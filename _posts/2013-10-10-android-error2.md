---
layout: post
title: 'The Import android.support.v7 cannot be resolved'
categories:
- Android
tags:
- Android

---

I tried the answer described here but it doesn´t worked for me. I have the last Android SDK tools ver. 23.0.2 and Android SDK Platform-tools ver. 20

The support library android-support-v4.jar cause this conflict, just delete the library under /libs folder of your project, don´t be scared, the library is already contained in the library appcompat_v7, clean and build your project, and your project will work like a charm!

详见资料:
<http://stackoverflow.com/questions/24998368/the-import-android-support-v7-cannot-be-resolved>