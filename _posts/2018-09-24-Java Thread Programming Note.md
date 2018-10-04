---
layout: post
title: Java并发编程实战笔记
date: 2018-09-24
categories: blog
tags: [java,Thread]
description: 自觉java并发是一个极其重要的点,需要努力学习。
---

### 基础线程

#### 线程安全性

- 安全性
	- 当多个线程访问某个类的时,不管运行时环境采用何种调度方式或者这些编程将如何交替执行,并且在主调代码中不需要任何额外的同步或协同,这个类都能表现出正确的行为,那么就称这个类是线程安全的.
	- 无状态对象一定是线程安全的.
- 原子性
	- 竞态条件会使得线程不安全,当某个计算的正确性取决于多个线程的交替执行时序时,就会发生竞态条件.
	- 最常见的竞态条件类型就是"先检查后执行(Check-Then-Act )"操作,即通过一个可能失效的观测结果来决定下一个条件.延迟初始化属于先检查后操作.
	- 要保持状态的一致性,就需要在单个院子操作中更新所有相关的状态变量.
- 可见性
	- 当线程在没有同步的情况下读取变量时,可能会得到一个失效值,但至少这个值是由之前某个线程设置的值,而不是一个随机值.这种安全性保证也被称之为最低安全性(out of thin air safety).基本变量不用volatile的double和long的64位数值变量不能保证最低安全性.
	- volatile变量影响了可见性和重排序
	- 当且仅当满足以下所有条件时,才应该使用volatile变量:
		- 对变量的写入操作不依赖变量的当前值,或者能保证只有单个线程更新变量的值.
		- 该变量不会与其他状态变量一起纳入不变性条件中.
		- 在访问变量时不需要加锁.
- 发布和逸出
```java
	public class SageListener{
        private final EventListener listener ;
        private SafeListener(){
            listener = new EventListener (){
                public void onEvent(Event e){
                    doSomething(e);
                }
            };
        }
        prblic static SafeLIstener newInstance(EventSource source ){
            SageListener safe = new SafeListener();
            source.registerLIstener(safe.listener);
            return safe;
        }
	}
```

