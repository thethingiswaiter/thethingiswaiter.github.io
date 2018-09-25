---
layout: post
title: 设计模式之简单工厂(Simple Factory)
date: 2018-09-01
categories: blog
tags: [Design_Pattern]
description: 一句话描述。
---
[TOC]
## 定义

> 提供一个创建对象实例的功能，而无需关心其具体实现。被创建实例的类型可以是接口、抽象类，也可以是具体的类。

> 简单工厂的本质是：选择实现
## 示例代码

- 创建接口类
```java
    //接口的定义
    public interface Api(){
        public void operate(String s);
    }

```
- 多个接口的实现
```java
    //实现接口的A对象
    public class ImplA implement Api(){
        public void operate(String s){
            //实现代码的具体功能
        }
    }
```
```java
    //实现接口的B对象
    public class ImplB implement Api(){
        public void operate(String s){
            //实现代码的具体功能
        }
    }
```
- 简单工厂的实现
```java
    //工厂类，用于创建和分配api
    public class Factory {
        public static Api createApi(T condition){
            //分情况选择不同的实现
            Api api= new Api();
            if(condition1){
                api = new ImplA();
            }else if(condition2){
                api = new ImplB();
            }
            return api;
        }
    }
```
- main客户端获取对象

```java
    public static void main(String[] args){
        Api api = Factory.createApi(condition);
        api.operation();
    }
```


## 认识简单工厂

    简单工厂也可以使用配置文件实现，这样更加灵活，利用反射和配置文件可以提供极大地灵活性

## 简单工厂的优缺点
- 优点
    - 封装
    - 解耦
- 缺点
    - 可能增加客户端的复杂度，让用户给定条件挑选增加了使用难度，也暴露了内部实现
    - 不方便扩展，不仅要添加接口和实现，还需要修改factory类

## 何时使用简单工厂

- 如果要完全封装隔离具体实现，让外部只能通过接口来操作，让client通过工厂获取接口，无需关心具体实现
- 需要将对外创建对象的职责集中管理和控制，简单工厂可以创建很多的、不相关的对象。