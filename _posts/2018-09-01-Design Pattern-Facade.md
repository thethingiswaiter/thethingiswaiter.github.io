---
layout: post
title: 设计模式之外观模式(Facade)
date: 2018-09-01
categories: blog
tags: [Design_Pattern]
description: 一句话描述。
---
[TOC]
## 定义

> 为子系统中的一组接口提供一个一致的界面，Facade模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。

## 代码示例

定义接口

```java
      public interface AmoduleApi{
          public void testA();
      }
      
      public interface BmoduleApi{
          public void testB();
      }
      
      public interface CmoduleApi{
          public void testC();
      }
```
实现接口

```java
      public class AmoduleImpl imolements AModuleApi{
          public void testA(){
              //sout(do something);
          }
      }
      
      public class BmoduleImpl imolements BModuleApi{
          public void testB(){
              //sout(do something);
          }
      }
      
      public class CmoduleImpl imolements CModuleApi{
          public void testC(){
              //sout(do something);
          }
      }
```
Facade类 

```java
      public class Facade {
          public boic test (){
              AmoduleApi a = new AmoduleImpl();
              a.testA();
              BmoduleApi b = new BmoduleImpl();
              b.testB();
              CmoduleApi c = new CmoduleImpl();
              c.testC();
          }
      }
```
client使用

```java
      public class Client{
          public static void main(String[] args){
              new Facade.test();
          }
      }
```

## 认识外观模式

      目的是让外部减少与子系统内多个模块的交互，松散耦合，从而让外部能够更简单的使用子系统

## 外观模式的优缺点
- 优点
    - 松散耦合
    - 简单易用
    - 更好的划分访问的层次
- 缺点
    - 用不好会有冗余感，易产生迷惑

## 外观模式相关

  > 外观模式的本质是：封装交互，简化调用

    外观模式体现了“最少知识原则”

### 何时使用外观模式

    - 当复杂的子系统提供一个简单的接口时
    - 需要让客户程序和抽象类的实现部分嗽散耦合
    - 如果需要构建多层结构的系统
    - 管理和控制，简单工厂可以创建很多的、不相关的对象。