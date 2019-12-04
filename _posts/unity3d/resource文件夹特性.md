---
layout: post
title: "resource文件夹优缺点"
date:   2017-03-31 17:00:00
categories: unity3d
tags: unity3d
description:
---

* content
{:toc}

##不要使用它！
强烈不建议使用Resources系统，原因如下：
	• 使用Resources文件夹将会使细粒度的内存管理变得更难
	• 对Resources文件夹的不恰当使用会导致应用程序构架和启动时间变长 
		○ 随着Resources文件夹数量的增加，在这些文件夹中管理Asset将会变得越来越难
	• 使用Resources系统会降低项目向不同平台提供定制内容的能立，并且导致项目无法进行增量内容更新 
		○ AssetBundle变体是Unity用来在设备层面调整内容的首选工具。[?1]




在下面两种特殊情况中，Resources系统会很有用，而且不会影响开发体验：
	1. Resources系统简单易用的特点使其非常适合用于快速开发原型。不过，当项目进入正式开发阶段时，应该停止使用Resources文件夹。
	2. Resources文件夹可以用于处理一些简单的内容： 
		○ 在项目的整个生命周期中都被使用的内容
		○ 非内存密集型内容
		○ 不太可能添加补丁或者不受平台和设备影响的内容
		○ 用于最小化引导的内容 [?2]


当项目被构建时，所有名为Resources的文件夹中的所有Asset和Object都会合并到同一个序列化文件中。这个序列化文件中还含有元数据（Metadata）和索引（Indexing）信息，类似于AssetBundle。正如AssetBundle文档中所描述的那样，这个索引中包含了一个用于将给定Object名称转换为恰当的File GUID和Local ID的序列化查找树，同时它也用于定位在序列化文件中偏移了指定字节数的Object。
在大多数平台上，用于查找的数据结构是平衡查找树，其时间复杂度为O(nlog(n))。因此，索引加载时间随Resources文件夹内Object数量而增长的速度高于线性增长。
加载Resources文件这一操作无法跳过，它会在应用程序启动显示不可交互的启动画面（Splash Screen）时执行。在一台低端设备上，初始化一个包含10000个资源文件的Resources系统要花费几秒的时间，然而在Resources文件夹中的这些Object很多都不会在应用程序的第一个Scene中使用到，完全没有必要加载。
