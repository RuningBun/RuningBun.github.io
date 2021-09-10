---
layout: post
title: "Unity3d shader学习笔记"
categories: unity3d
tags: unity3d shader 
---

* content
{:toc}

-  1.表面着色器：表面着色器是Unity特有的一种着色器代码类型，表面着色器定义在SubShader中。表面着色器需要编写的代码量很少，Unity会自动处理一些细节。但是表面着色器的本质和顶点、片元着色器是一样的，当我们定义一个表面着色器的时候，Unity会在背后将其转换成一个顶点、片元着色器。虽然使用表面着色器Unity会做很多的处理工作，使开发更为简单，但是其带来的缺点也是很明显的，如：灵活性很低，无法控制渲染的细节等。

-  2.顶点、片元着色器：顶点、片元着色器定义在SubShader中的Pass块中，编写顶点、片元着色器更为复杂，但是灵活性更高，我们可以控制的细节更多。


## Unity Shader 光照类型
-  #pragma surface surfaceFunction lightModel [optionalparams]

- - lightModel是所采用的光照模型 
- - - 官方提供两种光照模型 Lambert BlinnPhong 也可以自定义

```
        surfaceFunction     函数名
        lightModel          是所采用的光照模型
        optionalparams:     可选参数，一堆可选包括透明度，顶点与颜色函数，投射贴花shader等等。具体用到可以细选
```


## Unity Shader 常用函数介绍

- mul()  矩阵和位置坐标相乘的方法
- - UNITY_MATRIX_MVP 完成从模型空间到剪裁空间的转换 
- - mul(UNITY_MATRIX_MVP,v）=> UnityObjectToClipPos(v) 这两个方法是等价的


## Unity Shader 固定结构体

- SurfaceOutput
```
        输出结构体
        struct SurfaceOutput {
        
            half3 Albedo;       是漫反射的颜色值。
            
            half3 Normal;       法线坐标
            
            half3 Emission;     自发光颜色
            
            half Specular;      镜面反射系数
            
            half Gloss;         光泽系数
            
            half Alpha;         透明度系数
        
        };


```

