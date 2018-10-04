---
layout: post
title: 设计模式之单例模式(Sigleton)
date: 2018-10-01
categories: blog
tags: [Design_Pattern]
description: 一句话描述。
---

[TOC]

## 定义

> 保证一个类仅有一个实例,并提供一个访问他的全局访问点

## UML图


## 示例代码

- 懒汉式

```java 
    public class Singleton {
        private static Singleton instance = null;
        private Singleton (){}
        public static Singleton getInstance(){
            if (instance == null ){
                instace = new Singleton();
            }
            return instance;
        }
    }
```

- 饿汉式

```java
    public class Singleton {
        private static Singleton instance = new Singleton();
        private Singleton (){}
        public static Singleton getInstance(){
            return instance;
        }
    }
```
- 双重锁检查

```java 
    public class Singleton {
        private volatile static Singleton instance = null;
        public Singleton (){}
        public statci Singleton getInstance() {
            if (instance == null ){
                synchronized(Singleton.class){
                    if (instace == null ){
                        instance = new Singleton ();
                    }
                }
            }
            return instance;
        }
    }
```
- 类级内部类
```java
    public class Singleton {
        private statci class SingletonHolder{
            private staticSingleton instance = new Singleton();
        }
        private Singleton (){}
        public static Singleton getInstance(){
            return SingletonHolder.instance;
        }
    }
```

## 认识适配器模式

- 单例模式是用来保证这个类在运行期间智慧被创建一个类实例,另外单例模式还提供了一个全局唯一访问这个类实例的访问点, 就是getInstance方法.
- 目前实现的的单例是一个虚拟机的范围.因为装载类的功能是虚拟机的,所以一个虚拟机在通过自己的ClassLoader装载饿汉式实现蛋类的时候就会创建一个类的实例.

## 适配器模式相关

> 单例模式的本质是：控制实例数目

### 何时使用适配器模式

- 当需要控制一个类的实例只能有一个,而且客户只能从一个全局访问点访问它时,可以选用单例模式