---
layout: post
title: 'android issues'
categories:
- Android
tags:
- Android

---

##Permission is only granted to system app

In Eclipse:

Window -> Preferences -> Android -> Lint Error Checking.
In the list find an entry with ID = ProtectedPermission. Set the Severity to something lower than Error. This way you can still compile the project using Eclipse.

In Android Studio:

File -> Settings -> Inspections
Under Android Lint, locate Using system app permission. Either uncheck the checkbox or choose a Severity lower than Error.

参考:[http://stackoverflow.com/questions/13801984/permission-is-only-granted-to-system-app](http://stackoverflow.com/questions/13801984/permission-is-only-granted-to-system-app)  
