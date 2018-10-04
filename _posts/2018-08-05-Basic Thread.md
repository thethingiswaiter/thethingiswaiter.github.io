---
layout: post
title: 线程基础
date: 2018-08-05
categories: blog
tags: [java]
description: 刚开始学习线程,需努力。
---

[TOC]

## 1线程的概述

* 进程：正在运行的程序，负责了这个程序的内存空间分配，代表了内存中的执行区域。
* 线程：就是在一个进程中负责一个执行路径。
* 多线程：就是在一个进程中多个执行路径同时执行。
   ![](https://i.imgur.com/nUkkmoQ.png)
   ![](https://i.imgur.com/b7g3Gzc.png) 


   图上的一键优化与垃圾清除同时在运行，在一个进程中同时在执行了多个任务。
   假象：
   电脑上的程序同时在运行。“多任务”操作系统能同时运行多个进程（程序）——但实际是由于CPU分时机制的作用，使每个进程都能循环获得自己的CPU时间片。但由于轮换速度非常快，使得所有程序好象是在“同时”运行一样。
#### 并发性：有多个指令在多个处理器上同时执行；
#### 并行性：在同一时刻只有一个指令执行，但由于处理器性能较高，多个指令被快速轮换执行，使得在红宏观上具有多个指令同时执行的效果。

### 多线程的好处：

* 1.	解决了一个进程里面可以同时运行多个任务（执行路径）。
* 2.	提供资源的利用率，而不是提供效率。
### 多线程的弊端:
* 1.	降低了一个进程里面的线程的执行频率。
* 2.	对线程进行管理要求额外的 CPU开销。线程的使用会给系统带来上下文切换的额外负担。
* 3.	公有变量的同时读或写。当多个线程需要对公有变量进行写操作时,后一个线程往往会修改掉前一个线程存放的数据，发生线程安全问题。
* 4.	线程的死锁。即较长时间的等待或资源竞争以及死锁等多线程症状。
## 2创建线程的方式
### 2.1	创建线程的方式一：继承Thread类
```java
	public class NumberThread extends Thread{
	    //main 线程执行
	    private String name;
	    //main 线程执行
	    static{  
	        System.out.println(Thread.currentThread().getName() + "static ");
	    }
	
	    @Override
	    public void run() {  // 子线程中的运行代码
	        for(int i=100;i<=200;i++){
	            System.out.println(Thread.currentThread().getName() + ":" + i);
	            try {
	                Thread.sleep(2);
	            } catch (InterruptedException e) {
	                e.printStackTrace();
	            }
	        }
	    }
	}
```
Thread.currentThread().getName()是获取当前运行线程的名字。
需要复写run方法，把要执行的任务放在run方法中。 

#### 调用start()方法启动线程

```java
  	public static void main(String[] args) {
        //创建一个子线程  Thread-0
        NumberThread thread1 = new NumberThread();
        //子线程启动
        thread1.start();

        for (int i = 1; i <= 100; i++) {
            System.out.println(Thread.currentThread().getName() + ":" + i);
            try {
                Thread.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
```

#### 线程的使用细节：

1.	线程的启动使用父类的start()方法
2.	如果线程对象直接调用run()，那么JVN不会当作线程来运行，会认为是普通的方法调用。
3.	线程的启动只能由一次，否则抛出异常
4.	可以直接创建Thread类的对象并启动该线程，但是如果没有重写run()，什么也不执行。
5.	匿名内部类的线程实现方式

### 2.2	线程的状态
![](https://i.imgur.com/DOwSZRa.jpg) 

* 初始状态：新创建了一个线程对象。
* 可运行（就绪）：线程对象创建后，其他线程调用了该对象的start()方法。该状态的线程位于可运行线程池中，变得可运行，等待获取cpu的执行权。
* 运行：就绪状态的线程获取了CPU执行权，执行程序代码。
* 阻塞: 阻塞状态是线程因为某种原因放弃CPU使用权，暂时停止运行。直到线程进入就绪状态，才有机会转到运行状态。
* 终止：线程执行完它的任务时。

### 2.3	常见线程的方法
​	 Thread(String name)     初始化线程的名字
​	 getName()             返回线程的名字
​	 setName(String name)    设置线程对象名
​	 getPriority()             返回当前线程对象的优先级   默认线程的优先级是5
​	 setPriority(int newPriority) 设置线程的优先级    虽然设置了线程的优先级，但是具体的实现取决于底层的操作系统的实现（最大的优先级是10 ，最小的1 ， 默认是5）。
​	 currentThread()      返回CPU正在执行的线程的对象

**线程与状态相关的方法：**
 *	（1）start: 线程处于就绪状态,等待抢占CPU时间片，抢到了就能执行run方法
 *	（2）run: 线程的核心工作任务，说明该线程处于运行状态，若单独调用run,thread.run() ,就不存在多线程，是单线程顺序执行，run方法成为普通方法
 *	（3）sleep(ms)： 线程休眠，进入阻塞状态，该线程放弃占有的资源
 *	（4）join(): 等待某一个线程全部执行结束  
         join(ms) : 等待某一个线程执行一段时间
 *	（5）yield():当前线程退出CPU执行,进入就绪状态; 让优先级>= 本线程的其他线程（包括本线程）一起继续抢占CPU时间片
 *	
 *	（6）wait()： 当前线程处于等待状态
 *	（7）notify():唤醒在等待队列的线程，进入锁池状态

示例
```java
	class ThreadDemo1 extends Thread{
		public ThreadDemo1(){
		  
		}
		public ThreadDemo1( String name ){
		   super( name );
		}
	    
		public void run(){
		   int i = 0;
		   while(i < 30){
			  i++;
		      System.out.println( this.getName() + " "+ " : i = " + i);
			  System.out.println( Thread.currentThread().getName() + " "+ " : i = " + i);
			  System.out.println( Thread.currentThread() == this );
			  System.out.println( "getId()" + " "+ " : id = " + super.getId() );
			  System.out.println( "getPriority()" + " "+ " : Priority = " + super.getPriority() );
		   }
		}
	}
```
示例  
```java
	class Demo3 {
		public static void main(String[] args) {
	        ThreadDemo1 th1 = new ThreadDemo1("线程1");
			ThreadDemo1 th2 = new ThreadDemo1("线程2");
	        // 设置线程名
	        th1.setName( "th1" );
			th2.setName( "th2" );
	        // 设置线程优先级  1 ~ 10
			th1.setPriority( 10 ); 
			th2.setPriority( 7 ); 
			// 查看API定义的线程优先级范围
			System.out.println("max : " + Thread.MAX_PRIORITY );
			System.out.println("min : " + Thread.MIN_PRIORITY );
	        System.out.println("nor : " + Thread.NORM_PRIORITY );
			th1.start();
			th2.start();
			System.out.println("Hello World!");
		}
	}
```

### 2.4	创建线程的方式二:implements Runnable

#### 该类中的代码就是对线程要执行的任务的定义.  
* 1：定义了实现Runnable接口
* 2：重写Runnable接口中的run方法，就是将线程运行的代码放入在run方法中
* 3: new Thread(Runnable runnable)	

```java	
	public class CharThread implements Runnable{  
	    private String name;  	
	    public CharThread(){}	
	
	    @Override
	    public void run() {
	        for(char i='A';i<='Z';i++){
	            System.out.println(Thread.currentThread().getName() + ":" + i);
	            try {
	                Thread.sleep(2);
	            } catch (InterruptedException e) {
	                e.printStackTrace();
	            }
	        }
	    }
	}
```
#### 使用Runnable接口实现
​	在上面的代码中故意照成线程执行完后，执行Thread.sleep（100），以让cpu让给别的线程，
​	该方法会出现非运行时异常需要处理，这里必须进行try{}catch（）{}，
​	因为子类不能比父类抛出更多的异常，接口定义中没有异常，实现类也不能抛出异常。

### 银行账户存款取款案例
**假设有1000个终端同时给一个账户存100，取100元。**

```java
	//银行账户类
	public class Account {
		 //存款
	    public   Account  depoist(double money){
	        double temp =this.balance;
	        temp+= money;
	        try {
	            Thread.sleep(2);
	        } catch (InterruptedException e) {
	            e.printStackTrace();
	        }
	        this.balance = temp;
	        System.out.println("-----" + Thread.currentThread().getName() + "： 存款后的余额：" + this.balance);
	        return this;
	    }
	
	    //取款
	    public  Account  withdraw(double money){
	        double temp =this.balance;
	        temp-= money;
	        try {
	            Thread.sleep(2);
	        } catch (InterruptedException e) {
	            e.printStackTrace();
	        }
	        this.balance = temp;
	        System.out.println(Thread.currentThread().getName() + "： 取款后的余额：" + this.balance);
	        return this;
	    }
	
	}

	//模拟终端线程类
	public class AccountThread extends Thread {
	    private Account account;
	    public AccountThread(Account account) {
	         this.account= account;
	    }
	    @Override
	    public void run(){
	        account.depoist(100);
	        account.withdraw(100);
	    }
	
	}


	//测试
	public  static void main(String [] args){
         //共享资源
         Account account = new Account("Tom",2000);
		//1000个终端
         for(int i =1;i<=1000;i++){
             AccountThread thread = new AccountThread(account);
             thread.start();
         }

         try {
			//让1000个终端有足够的时间运行结束，主线程再统计操作后的金额
             Thread.sleep(10000);
         } catch (InterruptedException e) {
             e.printStackTrace();
         }
              //获得账户的余额
         System.out.println("-----" + account.getBalance()+"-------------");
	}
```

**运行结果：账户余额多次运行是小于 正确金额**  

**分析问题：**

| 线程     | time1                       | time2                            | time3         |
| -------- | --------------------------- | -------------------------------- | ------------- |
| t1线程： | 调用存款2000 +100(时间片到) |                                  | balance =2100 |
| t2线程： |                             | 调用存款balance =2000 +100 =2100 |               |


	其他线程类似，就会产生数据错误。

**解决：**  
- 一、为了保证数据的准确性，可以要求在同一时刻只有一个线程在访问存款/取款方法。
- 二、然可以访问该object中的非synchronized(this)同步代码块。
- 三、尤其关键的是，当一个线程访问object的一个synchronized(this)同步代码块时，其他线程对object中所有其它synchronized(this)同步代码块的访问将被阻塞。
- 四、当一个线程访问object的一个synchronized(this)同步代码块时，它就获得了这个object的对象锁。结果，其它线程对该object对象所有同步代码部分的访问都被暂时阻塞。
- 五、以上规则对其它对象锁同样适用.

### 银行账户存取款重构：

```java
  	 //存款
    public  synchronized Account  depoist(double money){
        double temp =this.balance;
        temp+= money;
        try {
            Thread.sleep(2);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        this.balance = temp;
        System.out.println("-----" + Thread.currentThread().getName() + "： 存款后的余额：" + this.balance);
        return this;
    }
    //取款
    public  synchronized   Account  withdraw(double money){
        double temp =this.balance;
        temp-= money;
        try {
            Thread.sleep(2);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        this.balance = temp;
        System.out.println(Thread.currentThread().getName() + "： 取款后的余额：" + this.balance);
        return this;
    }
    
```

**再运行程序，数据正确。**	 
## 4死锁

### 经典的“哲学家就餐问题”，
	5个哲学家吃中餐，坐在圆卓子旁。每人有5根筷子（不是5双），每两个人中间放一根，哲学家时而思考，时而进餐。
	每个人都需要一双筷子才能吃到东西，吃完后将筷子放回原处继续思考，如果每个人都立刻抓住自己左边的筷子，
	然后等待右边的筷子空出来，同时又不放下已经拿到的筷子，这样每个人都无法得到1双筷子，无法吃饭都会饿死，
	这种情况就会产生死锁：每个人都拥有其他人需要的资源，同时又等待其他人拥有的资源，
	并且每个人在获得所有需要的资源之前都不会放弃已经拥有的资源。

### 当多个线程完成功能需要同时获取多个共享资源的时候可能会导致死锁。  
* 1：两个任务以相反的顺序申请两个锁，死锁就可能出现
* 2：线程T1获得锁L1，线程T2获得锁L2，然后T1申请获得锁L2，同时T2申请获得锁L1，此时两个线程将要永久阻塞，死锁出现  
	
	如果一个类可能发生死锁，那么并不意味着每次都会发生死锁，只是表示有可能。要避免程序中出现死锁。
	例如，某个程序需要访问两个文件，当进程中的两个线程分别各锁住了一个文件，那它们都在等待对方解锁另一个文件，而这永远不会发生。
* 3：要避免死锁
```java
	public class DeadLock {
		public static void main(String[] args) {
			new Thread(new Runnable() { // 创建线程, 代表中国人
						public void run() {
							synchronized ("刀叉") { // 中国人拿到了刀叉
								System.out.println(Thread.currentThread().getName()
										+ ": 你不给我筷子, 我就不给你刀叉");
								try {
									Thread.sleep(10);
								} catch (InterruptedException e) {
									e.printStackTrace();
								}
								synchronized ("筷子") {
									System.out.println(Thread.currentThread()
											.getName() + ": 给你刀叉");
								}
							}
						}
					}, "中国人").start();
			new Thread(new Runnable() { // 美国人
						public void run() {
							synchronized ("筷子") { // 美国人拿到了筷子
								System.out.println(Thread.currentThread().getName()
										+ ": 你先给我刀叉, 我再给你筷子");
								try {
									Thread.sleep(10);
								} catch (InterruptedException e) {
									e.printStackTrace();
								}
								synchronized ("刀叉") {
									System.out.println(Thread.currentThread()
											.getName() + ": 好吧, 把筷子给你.");
								}
							}
						}
					}, "美国人").start();
		}
	}
```

## 5	线程的通讯
### 线程间通信其实就是多个线程在操作同一个资源，但操作动作不同



### 生产者消费者需求  
	我们需要生产者生产一次，消费者就消费一次,然后这样有序交替的循环执行。
	这就需要使用线程间的通信了。Java通过Object类的wait，notify，notifyAll这几个方法实现线程间的通信。

### 生产者消费者代码实现	
```java
	/**
	 * 生产者线程
	 * @author chixing
	 *
	 */
	public class Producer implements Runnable{
		private PublicResource resource;
		
		public Producer(PublicResource resource){
			this.resource = resource;
		}
		/**
		 * 生产一个面包
		 */
		public void run() {
			 while(true){
				 
				 resource.produce();
			 }
			
		}
	} 
	
	/**
	 * 消费者线程
	 * @author chixing
	 *
	 */
	public class Consumer  implements Runnable{
		private PublicResource resource;
		
		public Consumer(PublicResource resource){
			this.resource = resource;
		}
		/**
		 * 消费一个面包
		 */
		public void run() {
			while(true){
				resource.consume();
			}
		}
	
	}
	
	//共享资源，模拟表示生产与消费的资源
	public class PublicResource {
		private int number = 0;	
		
		//资源生产
		public synchronized void produce(){
	//		System.out.println("<><><><><><><>生产者线程抢到CPU<><><><><><><><><> number = " + number);
			if(number ==1){
				try {//不生产，生产者线程进入等待队列，等待消费者消费
					wait();
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}else{
				//执行生产，叫消费者来消费
			   number++;
			   System.out.println("生产者生产了一个面包，number = " + number);
			   notify();//唤醒在等待队列的消费者线程来消费
			}
		}
		
		//资源消费
		public synchronized void consume(){
	//		System.out.println("**********************消费者线程抢到CPU..................... number = " + number);
			if(number ==0){
				try {//不能消费，消费者线程进入等待队列，等待生产者生产
					wait();
				} catch (InterruptedException e) {
					e.printStackTrace();
				}		
			}else{
				//执行消费，叫生产者来生产
				number--; 
				System.out.println("【消费者】消费了一个面包，number = " + number);
				notify();//唤醒在等待状态的生产者来生产
			}	
		}
	}
	
	//测试
	public static void main(String[] args) {	
		PublicResource resource = new PublicResource();
		new Thread(new Producer(resource)).start();
		new Thread(new Consumer(resource)).start();
		 
	}
```

### 生产者消费者需求修改：
	篮子中最多能放10个面包，若不满10个面包，生产者可以生产；篮子中只要有面包，消费者可以消费面包,然后这样无限生产与消费执行
```java
	/**
	 * 生产者线程
	 * @author 驰星 
	 */
	public class Producer implements Runnable{
		private PublicResource resource;
		
		public Producer(PublicResource resource){
			this.resource = resource;
		}
		/**
		 * 生产面包
		 */
		public void run() {
			 while(true){
				for(int i =0;i<10;i++) {
				 resource.produce();
				 try {
						Thread.sleep(5);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
			 }
			
		}
	} 
	/**
	 * 消费者线程
	 * @author 驰星 
	 */
	public class Consumer  implements Runnable{
		private PublicResource resource;
		
		public Consumer(PublicResource resource){
			this.resource = resource;
		}
		/**
		 * 消费面包
		 */
		public void run() {
			while(true){
				for(int i =0;i<10;i++) {					
					resource.consume();
					try {
						Thread.sleep(15);//5-15随机数
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
			}
		}
	
	}
	//共享资源，模拟表示生产与消费的资源
	public class PublicResource {
		private int number = 0;	
		//资源生产
		public synchronized void produce(){
			if(number ==10){
				try {//不生产，生产者线程进入等待队列，等待消费者消费
					wait();
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}else{
				//执行生产，叫消费者来消费
			   number++;
			   System.out.println("生产者生产了一个面包，number = " + number);
			   notify();//唤醒在等待队列的消费者线程来消费
			}
		}		
		//资源消费
		public synchronized void consume(){
			if(number ==0){
				try {//不能消费，消费者线程进入等待队列，等待生产者生产
					wait();
				} catch (InterruptedException e) {
					e.printStackTrace();
				}				 
			}else{
				//执行消费，叫生产者来生产
				number--; 
				System.out.println("【消费者】消费了一个面包，number = " + number);
				notify();//唤醒在等待状态的生产者来生产
			}
		}
	}	
	测试：
	public static void main(String[] args) {		
		PublicResource resource = new PublicResource();
		new Thread(new Producer(resource)).start();
		new Thread(new Consumer(resource)).start();
		
	}
```

#### 等待唤醒机制
* wait：告诉当前线程放弃执行权，并放弃监视器（锁）并进入阻塞状态，直到其他线程持有获得执行权，并持有了相同的监视器（锁）并调用notify为止。
* notify：唤醒持有同一个监视器（锁）中调用wait的第一个线程，例如，餐馆有空位置后，等候就餐最久的顾客最先入座。注意：被唤醒的线程是进入了可运行状态。等待cpu执行权。
* notifyAll：唤醒持有同一监视器中调用wait的所有的线程。

线程间通信其实就是多个线程在操作同一个资源，但操作动作不同，wait，notify（），notifyAll()都使用在同步中，因为要对持有监视器（锁）的线程操作，所以要使用在同步中，因为只有同步才具有锁。
#### 为什么这些方法定义在Object类中
	因为这些方法在操作线程时，都必须要标识他们所操作线程持有的锁，只有同一个锁上的被等待线程，可以被统一锁上notify唤醒，
	不可以对不同锁中的线程进行唤醒，就是等待和唤醒必须是同一个锁。而锁由于可以使任意对象，所以可以被任意对象调用的方法定义在Object类中  

**wait() 和 sleep()有什么区别？**  
* wait():释放资源，释放锁。是Object的方法
* sleep():释放资源，不释放锁。是Thread的方法
* 定义了notify为什么还要定义notifyAll，因为只用notify容易出现只唤醒本方线程情况，导致程序中的所有线程都在等待。


#### 后台线程
**后台线程**：就是隐藏起来一直在默默运行的线程，直到进程结束。

**实现：**  
​	setDaemon(boolean on)  
**特点：**  
​	当所有的非后台线程结束时，程序也就终止了同时还会杀死进程中的所有后台线程，也就是说，只要有非后台线程还在运行，程序就不会终止，执行main方法的主线程就是一个非后台线程。
​	必须在启动线程之前（调用start方法之前）调用setDaemon（true）方法，才可以把该线程设置为后台线程。
​	一旦main（）执行完毕，那么程序就会终止，JVM也就退出了。
​	可以使用isDaemon() 测试该线程是否为后台线程（守护线程）。
​	该案例：开启了一个qq检测升级的后台线程，通过while真循环进行不停检测，当计数器变为100的时候，表示检测完毕，提示是否更新，线程同时结束。
​	为了验证，当非后台线程结束时，后台线程是否终止，故意让该后台线程睡眠一会。发现只要main线程执行完毕，后台线程也就随之消亡了。

```java
	class QQUpdate implements Runnable {
		int i = 0;
	
		@Override
		public void run() {
			while (true) {
	
				System.out.println(Thread.currentThread().getName() + " 检测是否有可用更新");
				i++;
				try {
					Thread.sleep(10);
				} catch (InterruptedException e) {
	
					e.printStackTrace();
				}
				if (i == 100) {
					System.out.println("有可用更新，是否升级？");
					break;
				}
			}
		}
	}
	public class Demo9 {
		public static void main(String[] args) {
			QQUpdate qq = new QQUpdate();
			Thread th = new Thread(qq, "qqupdate");
			th.setDaemon(true);
			th.start();
			System.out.println(th.isDaemon());
			System.out.println("hello world");
		}
	}
```
*Thread的join方法**  
​	当A线程执行到了B线程Join方法时A就会等待，等B线程都执行完A才会执行，Join可以用来临时加入线程执行
```java	
	public class JoinDemo {
	    public static void main(String[] args){
	        B b = new B("B线程");
	        A a = new A("A线程",b);
	
	        a.start();
	        b.start();
	    }
	}
	class A extends  Thread{
		private String name;
		private B b;
		public A(){}
		public A(String name,B b) {
	       super(name);
	         this.b = b;
	     }
	     @Override
	     public void run() {
	        //1-100 当i = 50 时候， b.join()
	         for(int i=1;i<=100;i++){
	             if(i == 50 ){
	                 try {
	                     b.join(); //等待b线程执行结束后，a线程再执行 
	                 } catch (InterruptedException e) {
	                     e.printStackTrace();
	                 }
	             }

	             System.out.println(Thread.currentThread().getName() + ":" + i);
	             try {
	                 Thread.sleep(2);
	             } catch (InterruptedException e) {
	                 e.printStackTrace();
	             }
	         }
	     }
	 }

	 class B extends  Thread{
	     private String name;
	     public B(){}
	     public B(String name) {
	         super(name);
	     }
	     @Override
	     public void run() {
	         for(int i=1;i<=100;i++){
	             System.out.println(Thread.currentThread().getName() + ":" + i);
	             try {
	                 Thread.sleep(2);
	             } catch (InterruptedException e) {
	                 e.printStackTrace();
	             }
	         }
	     }
	 }
```

**API中其他常用的线程安全类**

* 字符串  
	String:字符串常量，适用于字符串内容不经常发生改变的时候
	StringBuiler:字符串序列可变，适用于字符串内容经常发生改变的时候，线程不安全，适用于单线程
	StringBuffer: 是StringBuilder的等价类，线程安全，适用于多线程（StringBuffer中的数据被多个线程同时修改数据）

* ArrayList、Vector
	ArrayList：可动态增长的数组，线程不安全，适用于单线程
	Vector:可动态增长的数组，线程安全，适用于多线程 （Vector集合中的数据被多个线程同时修改数据）
* HashMap、Hashtable
	HashMap: 是key-value形式存储数据，线程不安全，适用于单线程
	Hashtable: 是key-value形式存储数据，线程安全，适用于多线程（Hashtable映射中的数据被多个线程同时修改数据）