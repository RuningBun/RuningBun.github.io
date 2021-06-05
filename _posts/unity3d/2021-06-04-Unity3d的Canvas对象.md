---
layout: post
title: "Unity3d Canvas对象"
categories: unity3d
tags: unity3d
---

* content
{:toc}

# Canvas 对象

## Canvas 组件     

#### 1.1: Canvas 组件作用
    1.它是UGUI中所有元素能够被显示的根本
    2.它主要负责渲染自己所有的子对象
    3.场景上允许有多个Canvas ,一般情况有一个即可  
        如果UI控件对象不是Canvas对象，那么控件将不能被渲染  Canvas控件上带有渲染方式

#### 1.2: Canvas 渲染方式
    1.Screen Space - Overlay：屏幕空间，覆盖模式，UI始终在前
        1.1 Pixel Perfect：是否开启无锯齿精确渲染（性能换效果）
        1.2 SortOrder：排序层编号（用于控制多个Canvas时的渲染先后顺序）
        1.3 TargetDisplay：目标设备（在哪个显示设备上显示）
        1.4 Additional Shader Channels：其他着色器通道，决定着色器可以读取哪些数据
    2.Screen Space - Camera：屏幕空间，摄像机模式，3D物体可以显示在UI之前
        2.1 RenderCamera：用于渲染UI的摄像机（如果不设置将类似于覆盖模式）
        2.2 Plane Distance：UI平面在摄像机前方的距离，类似整体Z轴的感觉
        2.3 Sorting Layer：所在排序层
        2.4 Order in Layer：排序层的序号
    3.World Space：世界空间，3D模式
        3.1 3D模式，可以把UI对象像3D物体一样处理，常用于VR或者AR
        3.2 Event Camera：用于处理UI事件的摄像机（如果不设置，不能正常注册UI事件）

#### 1.3: Canvas组件 总结
-    Canvas组件用来干啥 — 画布组件，用于渲染显示UI控件，UI控件必须作为子对象
-    场景中可以有多个Canvas对象 — 不同的渲染和分辨率适应方式（不常用）
-    Canvas组件的3种渲染方式
        覆盖模式：UI始终显示在最前面
        摄像机模式：3D物体可以显示在UI之前
        3D模式：用于制作3DUI，在VR和AR中常用，游戏中的3D UI效果才使用

##  Canvas Scaler 组件
-    CanvasScaler意思是画布缩放控制器
-    它是用于分辨率自适应的组件
-    它主要负责在不同分辨率下UI控件大小自适应
-    它并不负责位置，位置由之后的RectTransform组件负责
-    它主要提供了三种用于分辨率自适应的模式
-    我们可以选择符合我们项目需求的方式进行分辨率自适应 选中Canvas对象后在RectTransform组件中看到的宽高和缩放

宽高*缩放系数 = 屏幕分辨率   
参考分辨率 在缩放模式的宽高模式中出现的参数，参与分辨率自适应的计算
####  Reference Resolution:
-     Constant Pixel Size（恒定像素模式）：无论屏幕大小如何，UI始终保持相同像素大小
-     Scale With Screen Size（缩放模式）：根据屏幕尺寸进行缩放，随着屏幕尺寸放大缩小
-     Constant Physical Size（恒定物理模式）：无论屏幕大小和分辨率如何，UI元素始终保持相同物理大小

    
##### Constant Pixel Size 恒定像素模式
-    Scale Factor：
        缩放系数，按此系数缩放画布中的所有UI元素
-    Reference Pixels Per Unit：
        单位参考像素，多少像素对应Unity中的一个单位（默认一个单位为100像素）
        图片设置中的Pixels Per Unit设置，会和该参数一起参与计算

    恒定像素模式计算公式
    -    UI原始尺寸 =图片大小（像素）/ (Pixels Per Unit / Reference Pixels Per Unit)        

    总结
        恒定像素模式
        它不会让UI控件进行分辨率大小自适应
        会让UI控件始终保持设置的尺寸大小显示
        一般在进行游戏开发极少使用这种模式
        除非通过代码计算来设置缩放系数

##### Scale With Screen Size 缩放模式
-   Reference Resolution：参考分辨率（美术同学出图的标准分辨率）。
缩放模式下的所有匹配模式都会基于参考分辨率进行自适应计算
-   Screen Match Mode：屏幕匹配模式，当前屏幕分辨率宽高比不适应参考分辨率时，用于分辨率大小自适应的匹配模式

Expand：水平或垂直拓展画布区域，会根据宽高比的变化来放大缩小画布，可能有黑边
*   缩放系数 = Mathf.Min(屏幕宽/参考分辨率宽，屏幕高/参考分辨率高);
*   画布尺寸 = 屏幕尺寸 / 缩放系数
*   表现效果：最大程度的缩小UI元素，保留UI控件所有细节，可能会留黑边

Shrink：水平或垂直裁剪画布区域，会根据宽高比的变化来放大缩小画布，可能会裁剪
*   缩放系数 = Mathf.Min(800/1920，600/1080)= Mathf.Min(0.41667，0.5555) ≈ 0.41667
*   画布尺寸 =（800,600） / 0.41667≈（1920，1440）
*   表现效果：最大程度的放大UI元素，让UI元素能够填满画面，可能会出现裁剪
  
Match Width Or Height：以宽高或者二者的平均值作为参考来缩放画布区域  
```    
    //在取平均值之前，我们先取相对宽度和高度的对数
    float logWidth = Mathf.Log(屏幕宽 / 参考分辨率宽, 2);
    float logHeight = Mathf.Log(屏幕高 / 参考分辨率高, 2);
    //在对数空间中变换是为了获得更好的性能以及更准确的结果
    float logWeightedAverage = Mathf.Lerp(logWidth, logHeight, m_MatchWidthOrHeight);
    scaleFactor = Mathf.Pow(2, logWeightedAverage); 
```    
```
主要用于只有横屏模式或者竖屏模式的游戏
竖屏游戏：Match = 0
将画布宽度设置为参考分辨率的宽度
并保持比例不变，屏幕越高可能会有黑边
横屏游戏：Match = 1
将画布高度设置为参考分辨率的高度
并保持比例不变，屏幕越长可能会有黑边
```

##### Constant Physical Size 恒定物理模式
-   Physical Unit：物理单位，使用的物理单位种类
-   Fallback Screen DPI：备用DPI，当找不到设备DPI时，使用此值
-   Default Sprite DPI：默认图片DPI

DPI：（Dots Per Inch，每英寸点数）图像每英寸长度内的像素点数

根据DPI算出新的
Reference Pixels Per Unit （单位参考像素）
-   新单位参考像素 =  单位参考像素 * Physical Unit / Default Sprite DPI
再使用模式一：恒定像素模式的公式进行计算
-   原始尺寸 = 图片大小（像素）/ (Pixels Per Unit / 新单位参考像素
  
总结
-   会让UI控件始终保持设置的尺寸大小显示
-   而且会根据设备DPI进行计算，让在不同设备上的显示大小更加准确