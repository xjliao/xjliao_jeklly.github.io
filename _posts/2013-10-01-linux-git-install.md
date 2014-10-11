---
layout: post
title: "Linux git install"
categories:
- linux
tags:
- linux 

--- 

{% highlight console %}
[xjliao@li539-59 ~]$ wget https://www.kernel.org/pub/software/scm/git/git-2.1.2.tar.xz
[xjliao@li539-59 ~]$ tar xJvf ./git-2.1.2.tar.xz
[xjliao@li539-59 ~]$ cd ./git-2.1.2
[xjliao@li539-59 ~]$ sudo ./configure --prefix=/usr/local/git-2.1.2
[xjliao@li539-59 ~]$ sudo make && sudo make install
{% endhighlight %}

git依赖zlib-devel，openssl-devel，perl，cpio，expat-devel，gettext-devel这些包，如果出错基本上也是这些包造成的。我在安装时出现了如下错误。
    
出现错误一：  
usr/bin/perl Makefile.PL PREFIX='/usr/local/git' INSTALL_BASE='' --localedir='/usr/local/git/share/locale'
Can't locate ExtUtils/MakeMaker.pm in @INC (@INC contains: /usr/local/lib64/perl5 /usr/local/share/perl5 /usr/lib64/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib64/perl5 /usr/share/perl5 .) at Makefile.PL line 3.
BEGIN failed--compilation aborted at Makefile.PL line 3.
make[1]: *** [perl.mak] Error 2
make: *** [perl/perl.mak] Error 2
执行：  
yum install perl-ExtUtils-MakeMaker package.  
行进安装  
  
出现错误二：  
 /bin/sh: msgfmt: command not found
yum install gettext-devel
可解决！

