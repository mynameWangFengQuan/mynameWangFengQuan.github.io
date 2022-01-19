---
layout:     post
title:      配置文件加载顺序
subtitle:   SpringBoot
date:       2022-01-19
author:     王凤权
header-img: img/the-first.png
catalog:   true
tags:
    - Spring
    - SpringBoot
    - JAVA 
---


# 配置文件加载顺序

## 问题原因

在部署测试环境时，连不上配置的zookeeper，经排查怀疑是配置文件没有加载。

## spring配置文件加载顺序

SpringBoot可以从以下位置加载配置，优先级从高到低，高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配置

1. 命令行参数

> 所有的配置都可以在命令行上进行指定
>
> java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --server.port=8087 --server.context-path=/abc
>
> 多个配置用空格分开； --配置项=值
>

2. 来自java:comp/env的JNDI属性

3. Java系统属性（System.getProperties()）

4. 操作系统环境变量

5. RandomValuePropertySource配置的random.*属性值

由jar包外向jar包内进行寻找（*.properties>*.yml）

优先加载带profile

6. jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件

7. jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件

8. --spring.config.location=C:/application.properties

再来加载不带profile

9. jar包外部的application.properties或application.yml(不带spring.profile)配置文件

10. jar包内部的application.properties或application.yml(不带spring.profile)配置文件

11. @Configuration注解类上的@PropertySource

12. 通过SpringApplication.setDefaultProperties指定的默认属性
    
