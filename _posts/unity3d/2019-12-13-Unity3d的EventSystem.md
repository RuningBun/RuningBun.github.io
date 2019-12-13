---
layout: post
title: "Unity3d EventSystem 控件"
categories: unity3d
tags: unity3d
---

* content
{:toc}


## 组成
>系统生成的Event System里面主要有两个Components，分别是Event System和Standalone Input Module。
>其中Standalone Input Module是派生自BaseInputModule。

## 作用
### 1. EventSystem
    负责处理输入、射线投射以及发送事件
    一个场景中只能有一个EventSystem，否则EventSystem会失效

### 2. BaseInputModule
负责处理输入（点击、拖拽等），把输入事件发送到具体的对象
>1. Standalone Input Module：基本的键盘和鼠标输入系统，并跟踪鼠标的位置，以及鼠标/键盘所按下的按键。
>2. Touch Input Module：基本的触摸输入系统，用于处理触摸、拖拽以及位置数据，并可在其实现中模拟鼠标行为。
>3. Pointer Input Module：提供上面两者的基本功能，同时还可以通过代码进行访问。
可以自己继承BaseInputModule来实现自己的交互方式。

### 3. BaseRaycaster
>负责确定目标对象

>在UI中：GraphicRaycaster

>非UI中：Physics Raycaster 和 Physics 2D Raycaster

>此类系统均依赖于Event Camera，并用作全部光线投射的源。
可以自己继承BaseRaycaster来创建自己的光线投射系统。

## 小结
EventSystem负责管理，BaseInputModule负责输入，BaseRaycaster负责确定目标对象。


## 工作流

1. BaseInputModule接收用户的输入
2. BaseRaycaster根据用户的输入，找到目标物体
3. 根据用户的输入事件，调用目标物体上的对应接口实现。

## 内部工作机制

1. EventSystem把其GameObject上的所有BaseInputModule放到一个内部列表中。
2. 在每一帧调用UpdateModules方法，该方法会去调用列表中所有BaseInputModule的UpdateModule方法。
3. 在UpdateModule方法中，BaseInputModule会把自己的状态修改为"Updated"，之后BaseInputModule的Process接口才会被调用

## 事件
事件都必须在对象内操作才会发生

### 拖拉事件

接口|事件|作用|注意事项
- | :-: | :-: | :-: | -:
IBeginDragHandler|OnBeginDrag|开始拖拉|必须同时实现IDragHandler，否则失效
IDragHandler|OnDrag|正在拖拉|每当移动一定距离，就发生一次拖拉事件
IEndDragHandler|OnEndDrag|结束拖拉|必须同时实现IDragHandler，否则失效
IInitializePotentialDragHandler|OnInitializePotentialDrag|可能发生拖拉|必须同时实现IDragHandler。在对象内点击就会发生

### 点击事件

接口|事件|作用|注意事项
- | :-: | :-: | :-: | -:
IPointerEnterHandler|OnPointerEnter|鼠标进入对象
IPointerExitHandler|OnPointerExit|鼠标离开对象
IPointerDownHandler|OnPointerDown|按下鼠标
IPointerClickHandler|OnPointerClick|点击中该对象|在OnPointerUp后发生
IPointerUpHandler|OnPointerUp|松开鼠标

### 其他事件

接口|事件|作用|注意事项
- | :-: | :-: | :-: | -:
IScrollHandler|OnScroll|操作鼠标中间的滚轮
ISelectHandler|OnSelect|当EventSystem选中该对象|使用EventSystem中的SetSelectedGameObject方法来选中
IDeselectHandler|OnDeselect|不再选中该对象|点击对象外的地方就会变成不选中
IUpdateSelectedHandler|OnUpdateSelected|当对象被选中，则每帧都会发生|对象被选中才会发生
ISubmitHandler|OnSubmit|点击Submit键(默认是Enter键)|对象被选中才会发生
ICancelHandler|OnCancel|点击Cancel键(默认是Esc键)|对象被选中才会发生
IMoveHandler|OnMove|点击方向键|对象被选中才会发生
IDropHandler|OnDrop|拖拉结束|拖拉开始的地方必须先实现IDragHandler，该事件在拖拉结束的对象上发生(但不能是拖拉开始的对象)


>OnDrop的一个例子：物体A上实现了IDragHandler（和IDropHandler），物体B上实现了IDropHandler。从物体A上开始Drag，在物体A上结束Drag，没有触发OnDrop；从物体A上开始Drag，在物体B上结束Drag，可以触发OnDrop。

