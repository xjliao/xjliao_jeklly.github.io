---
layout: post
title: "Enable the Safari debug menu  Web Browsers"
categories:
- Safari
tags:
- Safari


---
##Enable the Safari debug menu  Web Browsers
While I look around Safari binary code, I found there is a hidden Debug menu. Type the following command in Terminal (while Safari is NOT running):
 % defaults write com.apple.Safari IncludeDebugMenu 1
Then launch Safari, and enjoy the new Debug menu. 

[Editor's note: The debug menu has some useful options on it, so you may find this a very useful hack. If you ever wish to disable it again, just repeat the command with a "0" instead of a "1".]