---
layout: post
title: "Unity3d 使用Rider 编辑器"
categories: unity3d
tags: unity3d rider 
---

* content
{:toc}


## 工欲善其事必先利其器

### Rider比VS的优点

调试Unity更加方便，在我使用Unity 2018.4.7+vs2017 专业版/企业版，经常出现无法断点的问题，尤其对于使用partial关键词的文件（一个类拆分在多个文件中）
安装包没有VS大，Rider2019.1约500MB，而VS2017接近20GB。
对于习惯使用Resharper来说，Rider的快捷键和使用体验是一致的，文件跳转和查找引用更加方便。
个人感觉Rider相对没有VS那么卡顿，在路径中查找非常快速
对于C#类中方法的提示，Rider鼠标移上去有方法提示，而实测vs2017和vs2019都没有，比如List.Add，Dictionary.Add
对于某些过时的UnityAPI或有更加好的代码写法，会有很友好的黄色提示，在每个文件的右侧都有warings和suggestions帮助优化代码。
Rider中集成了unity support插件无须安装
集成对shader的部分语法支持

- Rider对于Unity的支持介绍：[https://www.jetbrains.com/zh-cn/dotnet/promo/unity/](https://www.jetbrains.com/zh-cn/dotnet/promo/unity/)

- 使用vs和rider开发unity的对比：[https://www.jetbrains.com/rider/compare/unity-rider-vs-visual-studio/](https://www.jetbrains.com/rider/compare/unity-rider-vs-visual-studio/)

- Rider官方和vs的对比文章：[https://www.jetbrains.com/rider/compare/rider-vs-visual-studio/](https://www.jetbrains.com/rider/compare/rider-vs-visual-studio/)


### 修改单行字符的长度
使用情景：当使用快捷键格式化代码时，如果一行代码的长度(字符个数)太多，编辑器会自动换行。同时在编辑器的右侧会有一条坚立分隔线，超过这条线的在格式化时会自动换行

修改方法：Settings - Editor - Code Style - C#(可以换成其它语言) - Line Break and Wrapping - Hard wrap at 修改这个值就可以(默认是120可以修改成180，在1920x1280的分辨率下180会比满屏一行长一些)。从字段的描述来看，它是超过X个字符就会换行。

### 避免每次修改代码都进行编译
遇到问题：每当在Rider中按下Ctrl+S保存代码时，就会感觉Rider卡卡的，因为此时Rider正在和Unity同步，让Unity编译代码

修改方法： Settings - Languages&Frameworks - Unity Engine - 取消勾选 Automatically refresh assets in Unity

### 复制Rider的智能提示
在代码中的警告信息，或者可以优化的写法，Rider会显示黄色波浪线，鼠标移上去，会显示警告的信息，那要怎样复制这些提示文字呢？

鼠标移到波浪线代码上，按下Alt键+用鼠标点击提示文字，如果有很多的文字可以按下Ctrl+F1显示提示的全部内容。

### Rider常见提示
第一次用Rider打开项目时会提示，这是代码命名规范化的提示，一般的有驼峰法命名

rider detects naming conventions in opend soultions and updates setting accordingly