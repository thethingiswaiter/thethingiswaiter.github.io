---
layout: post
title: Java并发编程实战笔记
date: 2018-09-24
categories: blog
tags: [java,Thread]
description: 自觉java并发是一个极其重要的点,需要努力学习。
---

### 基础线程

#### 安全性
- 当多个线程访问某个类的时,不管运行时环境采用何种调度方式或者这些编程将如何交替执行,并且在主调代码中不需要任何额外的同步或协同,这个类都能表现出正确的行为,那么就称这个类是线程安全的.
- 无状态对象一定是线程安全的.
#### 原子性
- 竞态条件会使得线程不安全,当某个计算的正确性取决于多个线程的交替执行时序时,就会发生竞态条件.
- 最常见的竞态条件类型就是"先检查后执行(Check-Then-Act )"操作,即通过一个可能失效的观测结果来决定下一个条件.延迟初始化属于先检查后操作.
- 要保持状态的一致性,就需要在单个院子操作中更新所有相关的状态变量.
#### 可见性
- 当线程在没有同步的情况下读取变量时,可能会得到一个失效值,但至少这个值是由之前某个线程设置的值,而不是一个随机值.这种安全性保证也被称之为最低安全性(out of thin air safety).基本变量不用volatile的double和long的64位数值变量不能保证最低安全性.
- volatile变量影响了可见性和重排序
- 当且仅当满足以下所有条件时,才应该使用volatile变量:
	- 对变量的写入操作不依赖变量的当前值,或者能保证只有单个线程更新变量的值.
	- 该变量不会与其他状态变量一起纳入不变性条件中.
	- 在访问变量时不需要加锁.
#### 发布和逸出
```java
	//使用工厂方法来防止this引用在构造过程中逸出
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

#### 线程封闭
- 当数据共享的可变数据时,通常需要使用同步.一种避免使用同步的方式就是不共享数据.如果仅在单线程内访问数据,就不需要同步.这种计划就成为线程封闭.

##### 栈封闭
- 栈封闭是线程封闭的一种特例,在栈封闭中,只能通过局部变量才能访问对象.正如封装能使得代码更容易维持不变性条件那样,同步变量也能使对象更易于封闭在线程中.
##### ThreadLocal类

```java
	// 使用ThreadLocal来维持线程封闭性
	private static ThreadLocal<Connection> connectionHolder=new ThreadLocal<Connection>(){
        public Connection initialValue(){
            return DriverManager.getConnection(DB_URL);
        }
	};
	public static Connection getConnection(){
        return connectionHolder.get();
	}
```
#### 不变性
- 不可变对象一定是线程安全的.
- 当满足一下条件时,对象才是不可变的:
	- 对象创建以后其状态就不能修改.
	- 对象的所有域都是final类型.
	- 对象是正确创建的(在创建的期间,this引用没有逸出)
```java
	//在可变对象基础上构建的不可变类
	public final class ThreeStooges {
        private final Set<String > stooges = new HashSet<String>();
        
        public ThreeStooges(){
            stooges.add("Moe");
            stooges.add("Larry");
        }
        public boolean isStooge(String name){
            return stooges.contains(name);
        }
	}
```
#### 安全发布
**安全发布对象**
- 在静态初始化函数中初始化一个对象引用.
- 将对象的引用保存到volatile类型的域后者AtomicReferance对象中.
- 将对象的引用保存到某个正确的构造对象的final类型域中.
- 将对象的引用保存到一个由一个锁保护的域中.

**对象的发布需求取决于它的可变性:**
- 不可变对象可以通过任意机制来发布.
- 事实不可变对象必须通过安全方式来发布.
- 可变对象必须通过安全方式发布,并且必须是线程安全的后者由某个锁保护起来.

**在并发线程中使用和共性对象时,可以使用一些策略:**

- **线程封闭** 线程封闭的对象智能由一个线程拥有,对象被封闭在该线程中,并且只能由这个线程修改.
- **只读共享** 在没有额外同步的情况下,共享的只读对象可以由多个线程并发访问,但任何线程都不能修改,共享的只读对象包括不可变对象和事实不可变对象.
- **线程安全共享** 线程安全的对象在其内部实现同步,因此多个线程可以通过对象的公有接口来进行访问而不需要进一步的同步.
- **保护对象** 被保护的对象只能通过持有特定的锁来访问.保护对象包括封装在其他线程安全的对象,以及已发布的并且由某个特定锁保护的对象.

#### 线程安全的类
**在设计线程安全类的过程中,需要包含以下三个要素:**
- 找出构成对象状态的所有变量.
- 找出约束状态变量的不变性条件.
- 建立对象状态的并发访问管理策略.

> 将数据封装在对象的内部,可以将数据的访问限制在对象的方法上,从而更容易确保线程在访问数据时能持有正确的锁.

