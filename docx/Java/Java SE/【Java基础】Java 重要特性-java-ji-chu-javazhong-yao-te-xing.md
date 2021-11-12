---
title: 【Java基础】Java 重要特性
date: 2021-07-27 17:58:31.104
updated: 2021-07-27 18:00:32.492
url: https://pumpkn.xyz/archives/java-ji-chu-javazhong-yao-te-xing
categories: 
tags: 学习 | Java
---

# Java重要特点

## 1. Java语言时面向对象的(OOP)

## 2. Java语言时健壮的。Java的强类型机制、异常处理、垃圾的自动收集等时Java程序健壮性的重要保证。

## 3. Java语言是跨平台性的。（即一个编译好的.class文件可以在多个系统下运行，这种特性称为跨平台）

## 4. Java语言是解释型的
> *解释型语言：javascript,PHP,Java</br>编译型语言：C/C++*

> *区别是：解释型语言，编译后的代码，不能直接被机器运行，而需要解释器来执行；编译型语言编译后的代码，可以直接被机器执行*

# Java运行机制及运行过程
Java核心机制——Java虚拟机[JVM]

- JVM是一个虚拟的计算机、具有指令集并使用不同的存储区域。负责执行指令，管理数据、内存、寄存器。包含在**JDK**中
- 对于不同的平台，有不同的虚拟机。
- Java虚拟机机制屏蔽了底层运行平台的差别，实现了“一次编译，到处运行”。
![image.png](https://pumpkn.xyz/upload/2021/07/image-8fa1ae1d8dd5469f88389e99b65b9b5d.png)

# 什么是JDK、JRE

## JDK
- JDK全称(Java Development Kit Java开发工具包)
- JDK = JRE + Java的开发工具(java , javac等)
- JDK是提供给Java开发人员使用的，其中包含了Java的开发工具，也包含了JRE。所以安装了JDK，就不用再单独安装JRE了

## JRE

- JRE(Java Runtime Environment Java运行环境)
- JRE = JVM + Java核心类库
- 包括Java虚拟机和Java程序所需的核心类库等，如果想要运行一个开发好的Java程序，计算机中只需要安装JRE即可。

## JDK,JRE和JVM的包含关系

- JDK = JRE + 开发工具
- JRE = JVM + Java SE标准类库