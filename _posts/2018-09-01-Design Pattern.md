---
layout: post
title: 设计模式简介(Design Pattern)
date: 2018-09-01
categories: blog
tags: [Design_Pattern]
description: 一句话描述。
---
[TOC]

## 一、概述
## 二、创建型
- 1. 单例（Singleton）
- 2. 简单工厂（Simple Factory）
- 3. 工厂方法（Factory Method）
- 4. 抽象工厂（Abstract Factory）
- 5. 生成器（Builder）
- 6. 原型模式（Prototype）
## 三、行为型
- 1. 责任链（Chain Of Responsibility）
- 2. 命令（Command）
- 3. 解释器（Interpreter）
- 4. 迭代器（Iterator）
- 5. 中介者（Mediator）
- 6. 备忘录（Memento）
- 7. 观察者（Observer）
- 8. 状态（State）
- 9. 策略（Strategy）
- 10. 模板方法（Template Method）
- 11. 访问者（Visitor）
- 12. 空对象（Null）
## 四、结构型
- 1. 适配器（Adapter）
- 2. 桥接（Bridge）
- 3. 组合（Composite）
- 4. 装饰（Decorator）
- 5. 外观（Facade）
- 6. 享元（Flyweight）
- 7. 代理（Proxy）
### jdk 中的设计模式：
- 1)单例，比如 Runtime 类；
- 2)静态工厂 Interger a=Integer.valueOf(int or String);
- 3) 迭代器模式 Collection.interator();
- 4) 原型设计模式,clone 方法；
- 5) 适配器inputStreamReader 和 outputStreamWriter；
- 6) 桥接模式，jdbc，抽象部分与实现相分离；
- 7) 装饰模式 Reader 和 bufferedBeader；
- 8) 代理，jdk 动态代理；
- 9) 观察者 observable 和 observer；
- 10)责任链，classloader 的双亲委派模型； 
- 11)组合 ，某个类型的方法同时也接收自身类型作为参数java.util.list.addall(collection);
- 12)抽象工厂，一个创建新对象的方法，返回的是接口或抽象类Connection c=DriverManager .getConnection();
- 13)工厂方法，返回一个具体对象的方法Proxy.newProxyInstance;
- 14)解释器模式，该模式通常定义了一个语言的语法,java.util.pattern。

