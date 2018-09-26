---
layout: post
title:  "Thread In Java"
date:   2018-03-12 15:38:57 +0800
categories: Thread
catalog:    true
excerp: 2018-03-12 ~ 2018-
tags:
    - Thread
    - Memoery
---
# Thread In Java

## java内存模型的好文

[好文一](http://blog.csdn.net/sunxianghuang/article/details/51920794)
[好文二](http://blog.csdn.net/javazejian/article/details/72772461)
两个大佬的文章，没事的时候就多大佬博客看看，zejian厉害

## 缓存一致性

多核计算机中，为了解决内存和cpu速度矛盾，引入了高速缓存，每个cpu都有高速缓存，但是当多个cpu都处理内存中的同一块区域时，到底将哪个cpu高速缓存的内容写入内存中，所有cpu访问内存的时候需要遵循一个协议，`缓存一致性协议`。

java虚拟机规范希望定义一个种，使java程序能屏蔽平台硬件和系统带来了内存访问差异，`使得在任何平台上，java程序都能有一致的内存访问效果`

## [Object的wait和notify方法](http://turbosky.iteye.com/blog/2314144)

## thread join

## [线程中断](http://www.infoq.com/cn/articles/java-interrupt-mechanism)

对于线程sleep，等常见throw中断的类，将中断throw后，会自动把中断重置为false