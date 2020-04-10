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

本篇文章，将学习和探究在Maven项目中有助于管理依赖传递的机制-依赖范围（dependency scopes）

#### 2.名字理解

##### 2.1 classpath

classpath顾名思义，是Java的class文件的路径所在，JVM根据这个配置地址，去找编译的class文件来运行程序。

本文中的classpath指的是依赖的Jar包最终能够打包到项目中，JVM可以在classpath中找到依赖/传递依赖的这个Jar包。

##### 2.2依赖管理

我们知道，一个项目的开发上线，需要使用很多别人提供的Jar包，来减少自身项目的开发成本，比如数据库连接池，Http请求的调用封装等等，早期搭建项目，我们直接将依赖的各个Jar包放在某个目录中，然后打包时讲这里的Jar包打包到项目中，需要手动维护依赖的Jar包的依赖关系，而Maven可以自动处理，并且所有的Jar包统一放在本地的中央仓库，所有项目均使用中央仓库的Jar，免去了每个项目都要导入重复的Jar的工作。

#### 3.依赖传递

简单来讲，Maven的依赖有两种类型：**直接依赖**和**传递依赖**。

直接依赖指的就是项目中依赖某个jar，直接使用<dependency>标签，如：

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```

传递依赖指的是『直接依赖』所依赖的某些jar，也可以传递到项目中，而不需要声明依赖。通俗的来讲，就是项目A中引入依赖B，而B中依赖的C也会引入到A的classpath中（在A项目中可以直接使用C）。

注：最早时Maven只具有直接依赖的特性，这使得引用一个Jar包，不得不手动处理这个Jar所依赖的其他的Jar，深层的依赖关系让实际使用过程中的依赖很难维护。

#### 4. Dependency Scopes

Dependency Scopes（依赖范围）可以帮助我们限制依赖传递以及根据不同的构建任务来修改classpath路径。

Maven一共提供了6种不同的 Dependency Scopes

##### 4.1 Compile

这是maven默认的依赖范围，当我们引入依赖时，可以不指定<scope>标签，默认既是此类型。

这种类型的依赖在所有构建任务的项目类路径（classpath）中均是可用的，并且传播到依赖的项目中。

也就是这些依赖是可以传递的（传递依赖）。

**在编译、测试、打包/运行、都会使用这个依赖。具有传递依赖性**

```xml
<dependency>
    <groupId>commons-lang</groupId>
    <artifactId>commons-lang</artifactId>
    <version>2.6</version>
</dependency>
```

##### 4.2 Provided

这种依赖范围，表示依赖由JDK或者容器在运行时来提供，也就是在打包的时候，不会把依赖打包到项目中，并且不具有『传递依赖』特性。

**使用这种Scope，只有在项目编译以及测试时有效，不会『传递依赖』，不会打包到工程中。**

应用场景是web服务器在运行时会提供Servlet API。

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>2.5</version>
    <scope>provided</scope>
</dependency>
```

##### 4.3 Runtime

**这种依赖范围，只有在运行时/打包以及测试时才会依赖，也就是不会在编译阶段依赖。**

使用这种依赖的例子是JDBC驱动

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>6.0.6</version>
    <scope>runtime</scope>
</dependency>
```

##### 4.4 Test

仅仅作用在测试中，不具有『传递依赖』。

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```

##### 4.5 System

**和provided类似，区别是这种依赖是由系统提供**。也就是项目中已存在的某个jar包的方式（传统jar引用的方式）。

```xml
<dependency>
    <groupId>com.baeldung</groupId>
    <artifactId>custom-dependency</artifactId>
    <version>1.3.2</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/libs/custom-dependency-1.3.2.jar</systemPath>
</dependency>
```

##### 4.6 Import

这种依赖类型，在Maven 2.0.9后开始使用，只能作用在<dependencyManagement>中，<type>是pom的。

这种依赖范围，表示将使用pom中的依赖。

#### 5. Scope and Transitivity(范围和传递依赖)

不同的依赖范围有自己的作用范围，这意味着不同的『传递依赖』可能仅仅作用于本项目（或者具有传递特性）。

**但是Provided 和 Test 是不会将依赖打包到项目中的**

对于Compile的Scope

