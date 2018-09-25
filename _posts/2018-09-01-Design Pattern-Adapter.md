---
layout: post
title: 设计模式之适配器模式(Facade)
date: 2018-09-01
categories: blog
tags: [Design_Pattern]
description: 一句话描述。
---

[TOC]

## 定义

> 将一个类的接口转换成客户希望的另一个接口。适配器模式使得原本由于接口不能兼容而不能一起工作的类可以一起工作

## 示例代码

- 接口的定义

```java 
    public interface Target{
        public void request();
    }
```

- 已经存在的接口实现方法

```java
    public class Adaptee{
        public void specificRequest(){
            //do something
        }
    }
```
- 适配器对象的实现

```java 
    public class Adapter implements Target{
        private Adapree adaptee;
        public Adapter(Adaptee adaptee){
            this.adaptee = adaptee;
        }
        public void request(){
            adaptee .specificRequest();
        }
    }
```
- client客户使用
```java
    public class Client{
        Adaptee adaptee = new Adaperr();
        Target target = new Adapter(adaptee);
        target.request();
    }
```

## 认识适配器模式

- 适配器的主要功能是进行转换匹配，目的是复用已有的功能，而不是实现新的接口。

## 适配器模式的优点缺点

- 优点
    - 更好的复用性
    - 更好的扩展性
- 缺点
    - 过多的使用会使系统冗余

## 适配器模式相关

> 适配器模式的本质是：转换匹配，复用功能

### 何时使用适配器模式

- 已经存在一个类，但她的接口不符合要求
- 创建一个可以复用的类，这个类可能和一些不兼容类一起工作
- 如果存在一些已存在的子类，可以直接适配其父类
  ​      