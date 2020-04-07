---
layout: post  
title: "Maven Dependency Scopes"  
date: 2020-04-07 14:53:50 +0800  
categories: 个人笔记  
tag: "Maven"  
---

* content
{:toc}  


#### 1.简介

Maven是Java生态系统中最流行的构建工具，其中一个核心的特性就是依赖管理。

本篇文章，将学习和探究在Maven项目中有助于管理依赖传递的机制-dependency scopes

#### 2.依赖传递

简单来讲，Maven的依赖有两种类型：**直接依赖**和**传递依赖**。

直接依赖指的就是项目中依赖某个jar，直接使用<dependency>标签，如：

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```

传递依赖指的是『直接依赖』所依赖的某些jar，也可以传递到项目中，而不需要声明依赖。

#### 3.Dependency Scopes

Dependency Scopes（依赖范围）可以帮助我们限制依赖传递以及根据不同的构建任务来修改classpath路径。

Maven一共提供了6种不同的 Dependency Scopes

##### 3.1 Compile

这是maven默认的依赖范围，当我们引入依赖时，可以不指定<scope>标签，默认既是此类型。

这种类型的依赖在所有构建任务的项目类路径（classpath）中均是可用的，并且传播到依赖的项目中。

也就是这些依赖是可以传递的（传递依赖）

```xml
<dependency>
    <groupId>commons-lang</groupId>
    <artifactId>commons-lang</artifactId>
    <version>2.6</version>
</dependency>
```

##### 3.2Provided

