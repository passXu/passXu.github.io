---
layout: post
title:  "android里面的动画"
date:   2018-12-24 10:10 +0800
categories: android view animation
catalog: true
excerpt:
tags:
    - android
    - view
---

# 自定义 animation

## view 分类

view 的动画（帧动画，属性动画）
viewitem 的出现，离开动画（比如recycleview 的item的出现离开动画等，总之是多个item的之间的动画）
界面跳转动画（比如activity跳转动画，fragment）

配合手势的动画，这里还要研究多个view如果通过手势互相交互

----

看了 android dev的分类感觉他的分类更合适

### 单个ui元素的动画
这个就是属性动画
#### layoutupdate animation
- 默认layout animation
- 自定义layout animationqd

### drawable 的动画
- drawable 动画中的frame动画，就是把多个drawable包装成了一个drawable，当成drawable用
- VectorDrawable,将矢量图封装为vectordrawable，网上找矢量图，然后用as的vector asset导入即可。至于矢量图的兼容性，没有研究。[1](https://blog.csdn.net/xunmeng_93/article/details/73850351)，[矢量图动画没有研究](https://blog.csdn.net/zwlove5280/article/details/73650801)

### layout改变的动画

#### scene 与 transition, transition framework实现的

- 要求
- 目标layout得有root标签
- transition的view在start和end的layout中的id得相同
- transition framework相关文章 
- https://www.jianshu.com/p/89a9cdde42ae
- https://www.jianshu.com/p/e497123652b5
- 上面两篇文章已经比较全面了


### 物理运动的动画？
这个是Google I/O ‘17 新推出的物理动画库，

基本所有的动画需求都是在需要显示或者隐藏显示，移动显示时发生

next

android里面的各种bar statuebar之类的东西，