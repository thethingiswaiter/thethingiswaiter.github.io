---
layout: post
title: 设计模式之工厂方法模式(Factory Method)
date: 2018-10-01
categories: blog
tags: [Design_Pattern]
description: 一句话描述。
---

[TOC]

## 定义

> 定义一个用于创建对象的接口,让子类决定实例化哪一个类,Factory Method使一个类的实例化延迟到其子类.

## UML图


## 示例代码

- Product 定义

```java 
    public interface Product {
        //Product的方法和属性
    }
```

- Product 具体对象

```java
    public class ConcreteProduct implements Product  {
        //实现Product
    }
```
- 创建器的定义

```java 
    public class Singleton {
        protected abstract Product factoryMethod();
        public void someOperation (){
            Product procuct = factoryAMethod();
        }
    }
```
- 类级内部类
```java
    public class Concretev Creator extends Creator {
        Product factoryMethod()P{
            return new ConcreteProduct();
        }
    }
```

## 认识工厂方法模式

- 单例模式是用来保证这个类在运行期间智慧被创建一个类实例,另外单例模式还提供了一个全局唯一访问这个类实例的访问点, 就是getInstance方法.
- 目前实现的的单例是一个虚拟机的范围.因为装载类的功能是虚拟机的,所以一个虚拟机在通过自己的ClassLoader装载饿汉式实现蛋类的时候就会创建一个类的实例.

## 工厂方法模式的优缺点
- 优点
	- 可以在不知道具体实现的情况下编程
	- 更容易扩展对象的新版本
	- 连接平行的类层次
- 缺点
	- 具体产品对象和共产方法的耦合性

## 工厂方法模式相关

> 工厂方法模式的本质是：延迟到子类来选择实现

### 何时使用工厂方法模式

- 如果一个类都需要创建某个接口的随想,但是又不知道具体的实现,这种情况就可以把创建对象的工作延迟到子类中去实现.
- 如果一个类本身就希望由它的子类来创建的时候