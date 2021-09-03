---
layout: post
title: "Unity3d UnityWebRequest 和 HttpClient 区别"
categories: unity3d
tags: unity3d
---

* content
{:toc}

### 优势
- UnityWebRequest 不允许您使用更改某些标题。使用 HttpClient ，您几乎可以更改任何标题。
- UnityWebRequest 用于使用而无需担心线程或异步内容。你所做的就是使用协程来等待请求。整个线程的东西已经在 native 端为你完成了。
- 某些平台不支持 System.Net 命名空间中的任何内容。其中之一是 WebGL 。这意味着当您将平台切换到 WebGL 时，HttpClient 甚至不会编译。 UnityWebRequest 适用于 WebGL。
- UnityWebRequest 旨在更轻松地下载内存中的数据并将数据转换为 Unity 资源，例如 AudioClip 、 VideoClip 、 AssetBundle 、 Texture2D 等。使用 HttpClient ，您将不得不编写
-  大量代码来检索此类数据，或者可能必须在收到数据后将数据保存在光盘上才能将它们转换为 Unity 资源形式。

### 劣势
- UnityWebRequest 对SSL 支持不是很好 请求返回错误不明确 ， HttpClient 使用 SSL 会更方便。 

