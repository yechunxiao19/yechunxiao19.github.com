---
layout: post
title: "应用生命周期"
date: 2014-02-04 17:58:47 +0800
comments: true
categories: ios
---
###ios应用有5种状态：   

*  Not Running（非运行状态）应用未运行
*  Inactive（前台非活动状态）应用正在进入前台，此时不接受事件处理
*  Active（前台活动状态）前台正常运行状态
*  Background（后台状态）不存在后台run loop，则进入Suspended状态
*  Suspended（挂起状态）不执行代码，内存不够时，应用将终止

###整个应用的生命周期如图所示

![](http://www.cocoanetics.com/files/Bildschirmfoto-2012-03-05-um-5.26.29-PM.png)


###总结：
  
* didFinishLaunching整个生命周期只会调用一次。  
* 应用能进行后台运行，首先SDK必须在4.0以上的版本，其次得在info.plist中不禁用后台。
* 内存不足情况下，以及用户自行关闭应用的情况下，不会执行applicationWillTerminate:，所以必须要在applicationWillResignActive事件里保存数据。
* becomeActive和resignActive配对操作进行UI数据等的恢复。
* enterBackground和enterForeground配对操作进行用户数据等的恢复。