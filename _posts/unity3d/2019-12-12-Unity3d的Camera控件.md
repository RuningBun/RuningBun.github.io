---
layout: post
title: "Unity3d Camera 控件"
categories: unity3d
tags: unity3d
---

* content
{:toc}


## 属性

1·Clear Flags 清除标记
>    决定屏幕的哪部分将被清除。当使用多个相机来描绘不同的游戏景象时，利用它是非常方便的 
```
Skybox：把颜色缓冲设置为天空盒，并完全清空深度缓冲 
Solid：和天空盒一样，只是把颜色缓冲设置为纯色 
Depth only：这个选项会保留颜色缓冲，但会清空深度缓冲 
Don’t Clear：不清除任何缓冲 
这个给我的感觉就是，空（没有物体）的部分用什么来填充，如果是Skybox，就用天空盒填充，如果是Solid，就用纯色填充，如果是Depth only取决于其他相机的深度，未渲染部分显示什么由深度小于本摄像机的内容来决定。
```

2·Background 背景
>    在镜头中的所有元素描绘完成且没有天空盒的情况下，将选中的颜色应用到剩余的屏幕 

3·Culling Mask 剔除遮罩
>    包含或忽略相机渲染对象层。在检视视图中为你的对象指派层 
```
Culling Mask是按层（即GameObject.layer）选择性的渲染部分场景
当我们Culling Mask是Everything的时候，Scene右下角的Camera Preview会把背景也显示。 
但当我们把Culling Mask设置为只显示UI层的时候，背景这时候就不会显示在此Camera中了。 
```

4·projection 投射
>   切换摄像机的模拟透视功能    
>   (1).Perspective 透视
        相机将用完全透视的方式来渲染对象。 

>   (2).Orthographic 正交
        相机将用没有透视感的方式均匀地渲染对象 

5·Size 大小
> 当设置了正交时摄像机的视口大小。

6·Field of view 视野范围
> 相机的视角宽度，以及纵向的角度尺寸。 

7·Clipping Planes 剪裁平面
>从相机到开始渲染和停止渲染之间的距离。 

>(1). Near 近点
开始描绘的相对于相机最近的点。 

>(2). Far 远点
开始描绘的相对于相机最远的点。 

8·归一化视口矩形
> 用四个数值来表示这个相机的视图将绘制在屏幕的什么地方，使用屏幕坐标系（值0-1）。 

>(1) X
相机视图将进行绘制的水平位置的起点 

>(2) Y
相机视图将进行绘制的垂直位置的起点 

>(3) W (Width) 宽度
相机输出到屏幕上的宽度

>(4) H (Height) 高度 

>相机输出到屏幕上的高度

9·Depth 深度
>绘图顺序中的相机位置，具有较大值的相机将被绘制在具有较小值的相机的上面 
```
Depth决定相机在渲染顺序上的深度，具有较低深度的相机将在较高深度的相机之前渲染。 
而如果把Main Camera的Depth设置为2（大于等于Camera的Depth）：可以发现，运行后只能看到Main Camera拍摄的东西。
```

10·Rendering Path 渲染路径 
>该选项定义相机将要使用的渲染方法 

>(1) Use Player Settings
     使用播放器设置 
该相机将使用任意一个播放器设置中所设置的渲染路径

>(2) Vertex Lit 顶点光照
本相机对所有对象的渲染会作为顶点光照对象来渲染 

>(3) Forward 快速渲染
所有对象将按每种材质一个通道的方式来渲染，如同在Unity2.x中的标准 

>(4) Deferred Lighting 延迟照明
所有对象将无照明绘制一次，然后所有对象的照明将一起在渲染队列的末尾被渲染。 

11·Target Texture 目标纹理
>(Unity Pro/Advanced only)
请参考Render Texture，该页包含了相机视图的输出。这个引用属性将禁用相机渲染到屏幕的功能。