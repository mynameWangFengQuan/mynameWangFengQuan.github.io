---
layout:     post
title:      每日一面
subtitle:   Java面试题
date:       2022-01-21
author:     王凤权
header-img: img/the-first.png
catalog:   true
tags:
    - 面试题
    - JAVA 

---

# springmvc 和 spring-boot 区别 

### springmvc 和 spring-boot 区别 

#### 什么是Spring？它解决了什么问题？

我们说到Spring，一般指代的是Spring Framework，它是一个开源的应用程序框架，提供了一个简易的开发方式，**通过这种开发方式，将避免那些可能致使代码变得繁杂混乱的大量的业务/工具对象，说的更通俗一点就是由框架来帮你管理这些对象，包括它的创建，销毁等**，比如基于Spring的项目里经常能看到的`Bean`，它代表的就是由Spring管辖的对象。

而在被管理对象与业务逻辑之间，Spring通过IOC（控制反转）架起使用的桥梁，IOC也可以看做Spring最核心最重要的思想，**它有效的实现松耦合**，通过单例减少创建无用的对象，通过延迟加载优化初始化成本等。

其实不通过Spring框架依然可以实现这些功能特定，但是Spring 提供了更优雅的抽象接口以方便对这些功能的组装，同时又给予每个具体实现以灵活的配置；另外，基于Spring，你可以方便的与其他框架进行集成，如`hibernate`，`ibatis`等，Spring官方的原则是绝不重复造轮子，有好的解决方案只需要通过Spring进行集成即可。**纵览Spring的结构，你会发现Spring Framework 本身并未提供太多具体的功能，它主要专注于让你的项目代码组织更加优雅，使其具有极好的灵活性和扩展性，同时又能通过Spring集成业界优秀的解决方案**

#### 什么是Spring MVC？它解决了什么问题？

Spring MVC是Spring的一部分，Spring 出来以后，大家觉得很好用，于是按照这种模式设计了一个 MVC框架（一些用Spring 解耦的组件），**主要用于开发WEB应用和网络接口，它是Spring的一个模块，通过Dispatcher Servlet, ModelAndView 和 View Resolver，让应用开发变得很容易**，一个典型的Spring MVC应用开发分为下面几步：
 首先通过配置文件声明Dispatcher Servlet：

通过配置文件声明servlet详情，如MVC resource，data source，bean等

若需添加其它功能，如security，则需添加对应配置：

增加业务代码，如controller，service，model等，最后生成war包，通过容器进行启动

#### 什么是Spring Boot？它解决了什么问题？

初期的Spring通过代码加配置的形式为项目提供了良好的灵活性和扩展性，但随着Spring越来越庞大，其配置文件也越来越繁琐，太多复杂的xml文件也一直是Spring被人诟病的地方，为此Spring社区推出了Spring Boot，它的目的在于**实现自动配置，降低项目搭建的复杂度**

你甚至都不用额外的WEB容器，直接生成jar包执行即可，因为`spring-boot-starter-web`模块中包含有一个内置tomcat，可以直接提供容器使用；基于Spring Boot，不是说原来的配置没有了，而是Spring Boot有一套默认配置，我们可以把它看做比较通用的约定，而Spring Boot遵循的也是**约定优于配置**原则，同时，如果你需要使用到Spring以往提供的各种复杂但功能强大的配置功能，Spring Boot一样支持

在Spring Boot中，你会发现你引入的所有包都是*starter*形式，如：

- `spring-boot-starter-web-services`，针对SOAP Web Services
- `spring-boot-starter-web`，针对Web应用与网络接口
- `spring-boot-starter-jdbc`，针对JDBC
- `spring-boot-starter-data-jpa`，基于hibernate的持久层框架
- `spring-boot-starter-cache`，针对缓存支持
- 等等

#### Spring，Spring MVC，Spring Boot 三者比较

三者专注的领域不同，解决的问题也不一样；总的来说，Spring 就像一个大家族，有众多衍生产品例如 Boot，Security，JPA等等。但他们的基础都是Spring 的 IOC 和 AOP，IOC提供了依赖注入的容器，而AOP解决了面向切面的编程，然后在此两者的基础上实现了其他衍生产品的高级功能；Spring MVC是基于 Servlet 的一个 MVC 框架，主要解决 WEB 开发的问题，因为 Spring 的配置非常复杂，各种xml，properties处理起来比较繁琐。于是为了简化开发者的使用，Spring社区创造性地推出了Spring Boot，它遵循约定优于配置，极大降低了Spring使用门槛，但又不失Spring原本灵活强大的功能，下面用一张图来描述三者的关系：

![image](https://user-images.githubusercontent.com/43489916/150894010-efec339c-0eba-4721-90bd-4b9b08999d3b.png)

最后一句话总结：**Spring MVC和Spring Boot都属于Spring，Spring MVC 是基于Spring的一个 MVC 框架，而Spring Boot 是基于Spring的一套快速开发整合包**

> 参考：https://www.jianshu.com/p/42620a0a2c33

