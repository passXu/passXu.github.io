---
layout: post
title:  "android handler虚引用与内存溢出"
date:   2017-09-7 10:10 +0800
categories: android handler reference
catalog:    true
excerpt: android handler虚引用与内存溢出
tags:
    - jvm
    - 内存管理
---
# android handler虚引用与内存溢出

## java内部非静态类会持有外部类的引用

都知道handler中持有activity的引用，会导致内存溢出，所以网上推荐使用静态内部handler类，和弱引用activity来防止内存溢出，但是项目中用了弱引用activity之后，发现经常activity会被回收，导致handler无法正常工作，所以现在hadler改为直接引用activity，只要在activity的destroy中removeCallbacksAndMessages(null)即可。

## handler remove 相关方法

1. removeCallback(runnable); 移除消息队列中的runnable，看网上的说法这个好像只能移除队列中的runnable，如果是已经运行的runnable，则不能移除 `remove pending posts of runnable r that are in the message queue`

1. removeCallbacksAndMessages(null)

![handler](https://developer.android.com/reference/android/os/Handler.html)

## handler提供的方法供我们管理message以及callback、