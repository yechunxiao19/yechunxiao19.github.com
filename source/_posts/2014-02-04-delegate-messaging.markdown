---
layout: post
title: "应用生命周期"
date: 2014-02-04 17:58:47 +0800
comments: true
categories: ios
---
####ios应用有5种状态：   

*  Not Running（非运行状态）应用未运行
*  Inactive（前台非活动状态）应用正在进入前台，此时不接受事件处理
*  Active（前台活动状态）前台正常运行状态
*  Background（后台状态）不存在后台run loop，则进入Suspended状态
*  Suspended（挂起状态）不执行代码，内存不够时，应用将终止

####整个应用的生命周期如图所示

<img src="http://www.cocoanetics.com/files/Bildschirmfoto-2012-03-05-um-5.26.29-PM.png" alt="Drawing" width="600px"/>

####总结：
  
* `didFinishLaunching`整个生命周期只会调用一次。  
* 应用能进行后台运行，首先SDK必须在4.0以上的版本，其次得在info.plist中不禁用后台。
* 内存不足情况下，以及用户自行关闭应用的情况下，不会执行`applicationWillTerminate:`，所以必须要在`applicationWillResignActive`事件里保存数据。
* `becomeActive`和`resignActive`配对操作进行UI数据等的恢复。
* `enterBackground`和`enterForeground`配对操作进行用户数据等的恢复。
* 在以前，当app被按home键退出后，app仅有最多5秒钟的时候做一些保存或清理资源的工作。但是应用可以调用UIApplication的`beginBackgroundTaskWithExpirationHandler`方法，让app最多有10分钟的时间在后台长久运行。这个时间可以用来做清理本地缓存，发送统计数据等工作。