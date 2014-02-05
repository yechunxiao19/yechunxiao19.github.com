---
layout: post
title: "ViewController生命周期"
date: 2014-02-04 20:38:21 +0800
comments: true
categories: ios
---
编程实现ViewController初始化时，创建一个自定义的初始化方法，调用父类的init方法，然后进行类的特定初始化。一般情况下，初始化方法不写过于复杂的。
  
ViewController加载View对象的过程：  

1. ViewController调用`loadView`方法，loadView的默认实现执行以下两个方法之一  
	* 如果ViewController与storyboard关联，它会从storyboard加载  
	* 如果ViewController不与storyboard关联，则会创建一个空UIView对象分配给View属性  
2. ViewController调用`viewDidLoad`方法，执行子类的额外加载任务

<img src="https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/Art/loading_a_view_into_memory_2x.png" alt="Drawing" width="600px"/>

编程创建View，而不使用storyboard，应该重载loadView方法。在方法中实现以下步骤：

1. 创建root view对象
	* root view应该包含ViewController关联的所有其他view。一般情况下，root view的大小应该充满整个屏幕，但也可以根据需要调整。[“Resizing the View Controller’s Views.”](https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/AdoptingaFull-ScreenLayout/AdoptingaFull-ScreenLayout.html#//apple_ref/doc/uid/TP40007457-CH13-SW2)
	* 你可以使用通用的UIView对象，也可以使用自定义的UIView，以及任何view的扩展来填充屏幕
2. 创建subviews添加到root view
	* 对于每个subview都应该创建并初始化
	* 添加到父view使用`addSubview:`方法
3. 如果你使用auto layout，对于你创建的每个view设置足够的约束，否则，实现`viewWillLayoutSubviews`和`viewDidLayoutSubviews `来调整subview的大小
4. 分配root view给ViewController的view属性   

编程创建view示例 

	- (void)loadView
	{
    	CGRect applicationFrame = [[UIScreen mainScreen] applicationFrame];
    	UIView *contentView = [[UIView alloc] initWithFrame:applicationFrame];
    	contentView.backgroundColor = [UIColor blackColor];
    	self.view = contentView;
 
    	levelView = [[LevelView alloc] initWithFrame:applicationFrame 		viewController:self];
    	[self.view addSubview:levelView];
	}

有效的内存管理：

* `Initialization methods`  为ViewController分配关键的数据结构
* `loadView`  创建你的view对象，如果使用storyboard，则不需要重载此方法
* `Custom properties and methods` 创建自定义对象
* `viewDidLoad` 分配或者加载view所需数据
* `didReceiveMemoryWarning` 使用这个方法来释放所有ViewController相关联的非必要对象，ios6以后，也可以在这个方法释放view
* `dealloc` 重载这个方法只执行ViewController的最后清理，实例变量和属性对象被自动释放，不需要显式地释放它们

对于ios6以后的版本，按需要释放ViewController的view对象。对于释放view对象来获取内存空间的必要性，应该由开发者来判断。

释放ViewController不显示的view

	- (void)didReceiveMemoryWarning
	{
    	[super didReceiveMemoryWarning];
    	// Add code to clean up any of your own resources that are no longer necessary.
    	if ([self.view window] == nil)
    	{
        	// Add code to preserve data stored in the views that might be
        	// needed later.
 
        	// Add code to clean up other strong references to the view in
        	// the view hierarchy.
        	self.view = nil;
    	}
    }

整个ViewController的生命周期中，其余方法调用如图所示
![](http://h.hiphotos.bdimg.com/album/s%3D550%3Bq%3D90%3Bc%3Dxiangce%2C100%2C100/sign=8668cf68233fb80e08d161d206ea5e13/3801213fb80e7becd366c0b82d2eb9389b506b2f.jpg?referer=d1cf6431d309b3deb2a8d058d5e5&x=.jpg)






