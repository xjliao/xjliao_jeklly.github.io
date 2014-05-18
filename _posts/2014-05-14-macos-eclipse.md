---
layout: post
title: "macos eclipse 提示：如果打开“Eclipse.app”，您需要Java SE 6 runtime。您想现在安装一个吗？"
categories:
- Java
tags:
- Java


---
eclipse 默认使用jdk 6，但macos 10.9已经不自带jdk6了  

解决方案：  
1、使用自定义jdk  
安装好jdk后，修改文件。我这里使用得是jdk1.7.0_55

```console
xjliao:xjliao.me lxj$ sudo vim /Library/Java/JavaVirtualMachines/jdk1.7.0_55.jdk/Contents/Info.plist 
```

修改或添加如下内容

{% highlight xml linenos %}
<key>JVMCapabilities</key>
<array>
      <string>JNI</string>
       <string>BundledApp</string>
       <string>WebStart</string>
        <string>Applets</string>
       <string>CommandLine</string>
</array>
{% endhighlight %}

2、按照提示去下载jdk6  
[下载：http://support.apple.com/kb/DL1572?viewlocale=en_US](http://support.apple.com/kb/DL1572?viewlocale=en_US)  
安装后启动即可