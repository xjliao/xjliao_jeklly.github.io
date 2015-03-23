---
layout: post
title: "Cordova debugging"
categories:
- Cordova
tags:
- Cordova


---
##Cordova debugging
Debugging your Application

With the desktop browser, you can leverage the browser's debugging tools. In a WebKit browser, you can open WebInspector to inspect the DOM and debug JavaScript. Most other browsers have an equivalent debugging tool.


Additional Tools

PhoneGap Emulator uses Ripple to emulate the PhoneGap API.
Ripple Emulator emulates the PhoneGap API.
ScreenQueri.es to preview exact device screen sizes.
thumbs.js is a TouchEvent polyfill.
PhoneGap Desktop is a mocking library for the PhoneGap API.
##Remote Web Inspector

After you've developed the first stage of your application with your desktop browser, you will want to test it on a mobile device. The main hurdle with testing on a mobile device is that you will not have access to WebInspector. This is where a remote web inspector steps in.

##Safari Remote Debugging

If you are doing iOS PhoneGap debugging and have the Safari Develop Menu enabled, you can access the currently active session through the built-in Safari Web Inspector. To activate, go to Develop -> (iPad || iPhone) Simulator (normally, the third menu item) and click the active session you want to connect to. Voila!

##Chrome Remote Debugging

If you are doing Android PhoneGap debugging and have an Android 4.4 device and Chrome 30+, you can use the new WebView Debugging tools added in Android 4.4. If you are using Cordova 3.3 or higher, this is already supported, and only requires the Debuggable flag in your AndroidManifest.xml. For Cordova 3.2, you will need to enable WebView debugging using some code, or by use of a plugin.

##GapDebug

A complete iOS and Android debugging experience for PhoneGap and Cordova apps, debug on both Windows and Mac platforms. Automated plug-and-go debugging with no requirement to build debug support into your app. GapDebug integrated versions of the Safari Webkit Inspector for iOS debugging and Chrome Dev Tools for Android debugging. GapDebug is maintained by Genuitec and is a always free tool.

Try GapDebug here.

##Weinre

Weinre is a remote WebInspector that will debug any browser remotely. It is not quite a full-fledged debugger, but gives you a live view of the DOM and access to the JavaScript console. Unfortunately, no breakpoints or stack traces are available, but the JavaScript console can lend clues about errors.

You can run Weinre locally or use debug.phonegap.com.

WebKit's Remote Web Inspector

WebKit has the ability to connect it's WebInspector to remote WebKit browsers, although few mobile platform support this feature.

BlackBerry supports Remote Web Inspector
iOS has unofficial support for Remote Web Inspector
jsHybugger

jsHybugger is a (commercially available) tool that lets you debug PhoneGap apps on Android using Chrome DevTools. A plugin for PhoneGap 3.x and library for PhoneGap 2.x is available. Install jsHybugger from GitHub with the following command. This adds the plugin to your PhoneGap 3.x application.

phonegap local plugin add https://github.com/jsHybugger/cordova-plugin-jshybugger.git
After installing and starting the app on the device, you can use Chrome DevTools on a desktop machine to debug the app.

Example workflow

phonegap create <name-of-project>
cd <name-of-project>
phonegap local plugin add https://github.com/jsHybugger/cordova-plugin-jshybugger.git
phonegap build android
phonegap run android --device
See the jsHybugger home page for details.

Additional Debugging Resources

JSBin
##Log Messages

When all else fails, you can always use console.log('...'); or alert('...'); to mobile web application. I know it's not pretty, but it can help when you're in a jam.

Advanced log message handling can be found on Exception Log Handling.

Android

On Android, all console.log('...'); messages will appear as printouts in the command-line tool logcat, which is bundled with the Android SDK and integrated into the Android Eclipse plugin.

BlackBerry

On BlackBerry, all console.log('...'); are printed to the BlackBerry's Event Log. The Event Log can be accessed by pressing ALT + LGLG.

iOS

On iOS, all console.log('...'); are output to the Xcode Debug Area console.