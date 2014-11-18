---
layout: post
title: 'No resource found that matches the given name '@style/Theme.AppCompat.Light'
categories:
- Android
tags:
- Android

---

what are the steps for that? where is AppCompat located?
To download

http://developer.android.com/tools/support-library/setup.html

If use eclipse

Goto windows android sdk manager. Under extras check Android Support Library and click install.

enter image description here

After that the AppCompat Can be found at

android-sdk/extras/android/support/v7/appcompat
You need to reference this AppCompat in your android project.

Import the library to eclipse.

Right click on your android project. Goto Properties. goto andorid. Click Add Library and reference the same.

Clean and Rebuild your project

解决方案:
<http://stackoverflow.com/questions/21059612/no-resource-found-that-matches-the-given-name-style-theme-appcompat-light>

