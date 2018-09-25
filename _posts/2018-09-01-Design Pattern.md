---
layout: post
title: 设计模式简介(Design Pattern)
date: 2018-09-01
categories: blog
tags: [Design_Pattern]
description: 一句话描述。
---
[TOC]
### jdk 中的设计模式：

- 1）单例，比如 Runtime 类；
- 2）静态工厂 Interger a=Integer.valueOf(int or String);
- 3) 迭代器模式 Collection.interator();
- 4) 原型设计模式,clone 方法；
- 5) 适配器inputStreamReader 和 outputStreamWriter；
- 6) 桥接模式，jdbc，抽象部分与实现相分离；
- 7) 装饰模式 Reader 和 bufferedBeader；
- 8) 代理，jdk 动态代理；
- 9) 观察者 observable 和 observer；
- 10)责任链，classloader 的双亲委派模型； 
- 11)组合 ，某个类型的方法同时也接收自身类型作为参数java.util.list.addall(collection);
- 12)抽象工厂，一个创建新对象的方法，返回的是接口或抽象类Connection c=DriverManager.getConnection();
- 13)工厂方法，返回一个具体对象的方法Proxy.newProxyInstance;
- 14)解释器模式，该模式通常定义了一个语言的语法,java.util.pattern。