---
layout: post
title:  "databinding recycleview"
date:   2018-04-02 15:38:57 +0800
categories: 
catalog:    true
excerp: 2018-04-08 ~ 2018-
tags:
    -week
---
# databinding recycleview（初步理解）

## databinding

本质是自动生成修改ui的代码，对于我们自己写的代码就会简洁很多

## databingding用法

[DataBinding 最强技能](https://zhuanlan.zhihu.com/p/20839659)
[realm databinding guide](https://academy.realm.io/cn/posts/data-binding-android-boyar-mount/)
[]()

## databinding关键点

###  1.layout

使用databinding时，ui xml的写法会改变，能支持`声明`与`引用`变量，还支持一些表达式，还支持自定义bindingclassname，等功能，找到了在xml中写代码的感觉

### 2. bean

数据对象，数据对象不能是一般的bean类，得是databinding支持的特殊bean类，`Observable` `ObservableField` `ObservableFloat` 还有array格式的bean等等 ，本质就是将数据对应到ui

### 3. DataBinding

1. binding方式多种多样，在一个activity中，后binding的会使先binding的无效
1. binding的方式太多，这里还有点云里雾里

## recycleview 

[这篇文章写的很到位，多读几遍](https://www.kymjs.com/code/2016/07/10/01/)

### recycleview 关键点

1. manager 
2. itemanimation
3. adapter
4. itemdecoration
5. viewholer
6. 缓存机制

### recyclerview 与databinding混合使用，主要是在adapter的中将model与viewbinding

