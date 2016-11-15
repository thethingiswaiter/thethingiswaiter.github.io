---
layout: post
title: Java课程
date: 2016-09-07
categories: blog
tags: [java]
description: 一句话描述。

---


## eclipse

- java在G盘，X86，eclipse可打开

- 工作空间-代码的位置-可以自定义
  -可选择默认
- jre属于基本环境

- 包可以为空

- 文件夹的.classpath和.project是项目文件，.class为字节码文件和.java为原文件

- 代码提示的快捷键(Alt+/)，也可以设置<b>智能识别</b> ，具体步骤如下：
    - 窗口->java->编辑器->内容辅助双击->将第一行的radiobutton改为覆盖首选项，延迟time值的200ms可改小一些->在下一行输入abc，然后在文件下导出首选项到桌面，然后用记事本打开，按Ctrl+F，并输入abc，找到后，在你的abc后面加上其他所有字母，包含大小写，用java的文件下导入将本文件导入，即可实现智能识别

- 包(package)近似于文件夹

- 项目文件的拷贝及使用

  - 项目的右键复制可以完整复制
  
  - 必须使用“导入”打开项目(直接放到空间目录下可能会产生bug)
  
- 窗体首选项

- main方法args

---

## 第三次课

- /r表示将光标移到本行的最前面
- /n表示将光标移到下一行，但不能确定是行的最前面
- string.tocharArray可以将string转化为char[]
- java.util(工具类包)
- import：在一个包中调用另一个包的类

---

## 第四次课

- ^(异或运算符) 两侧的表达式的布尔值一样，结果为假；不一样，结果为真
- &(短路与) 效果与&&相同，但可以减少运算 
- |(短路或) 效果与||相同，但可以减少运算
- block：一对相邻的{}之间的代码称之为代码块
- switch的穿透利用如下：

```C
public class Switch{
	public static void mian(Sttring [] args){
		double d = Math.random();
		int e= 1+(int)(d*12);
		System.out.printIn(e);
		switch(e){
		case 1:
		case 3:
		case 5:
		case 7:  
		case 8:  
		case 10:  
		case 12:
			System.out.printIn(e+"月是31天"); 
			break; 
		case 2:  
			System.out.printIn(e+"月是28天"); 
			break; 
		case 4:  
		case 6:  
		case 9:  
		case 11:  
			System.out.printIn(e+"月是30天"); 
			break;
		default： 
			System.out.printIn(e+"不是合法的");
		}	
	}
}
```
## 第六次课

- 这里缺了第五节课，是因为第五节课的内容太少，懒的往上加。
- 面向对象
  - state状态由属性表示
  - behavior行为由方法表示
  - class类是一类对象行为和状态
  - opp(produce-oriented programming)：依照某种次序进行逐一编程
  - oop(object-oriented programming)：将信息以上帝视角进行划分，将其处理。达到先静后动的方式搭建。首先搭建环境，然后再将其联系。
  
## 已经不晓得是第几节课了
- 里氏替换原则（多态）
- 子类对象的私有方法不属于多态

## 字符串的有关API
- String为字符常量，而Stringbuffer为字符变量，stringtokenizer可用于分解字符串；
- String.append(string string);在字符串的后面追加一个字符串；
- StringBuffer.insert(int i,object object);在字符串的第i个位置插入一个对象；
- StringTokenizer 是出于兼容性的原因而被保留的遗留类（虽然在新代码中并不鼓励使用它）。建议所有寻求此功能的人使用 String 的 split 方法或 java.util.regex 包

## File I/O
- File("string string");string =Url;
- Boolean file.exists();
- file.createNewFile();//根据定义时的文件后缀名自行创建文件；
- file.getName();
- file.getAbsolutePath();
- file.lastModified();//上次修改时间；Date（file.lastModified()）；//更改日期格式；
- Boolean file.canread();
- Boolean file.canWrite();
- Boolean file.canExcute();
- file.isDirectory()
- File[] fileList=file.listFiles();//包含路径下的所有文件和文件夹；
- scanner(new File(String string))string=Url;
- out(new File(String string)) string=Url; 

## File Stream
- Outputstream
  - FileOutputStream out =new FileOutputStream(new File("c:/file.txt"));  
    - 输入进去的文件是转化为二进制进行存储的，
- http://blog.csdn.net/waterxcfg304/article/details/24252627 Oracle导入数据的的七种方法;
- 
----


















