---
date: 2019-01-03T10:00:00+08:00
title: 基准代码
menu:
  main:
    parent: "factor"
weight: 201
description : "云原生12 factor之基准代码(Codebase)"
---


### 官方介绍

Codebase

One codebase tracked in revision control, many deploys

基准代码

一份基准代码（Codebase），多份部署（deploy）

12-Factor应用(译者注：应该是说一个使用本文概念来设计的应用，下同)通常会使用版本控制系统加以管理，如Git, Mercurial, Subversion。一份用来跟踪代码所有修订版本的数据库被称作 代码库（code repository, code repo, repo）。

在类似 SVN 这样的集中式版本控制系统中，基准代码 就是指控制系统中的这一份代码库；而在 Git 那样的分布式版本控制系统中，基准代码 则是指最上游的那份代码库。

基准代码和应用之间总是保持一一对应的关系：

- 一旦有多个基准代码，就不能称为一个应用，而是一个分布式系统。分布式系统中的每一个组件都是一个应用，每一个应用可以分别使用 12-Factor 进行开发。
- 多个应用共享一份基准代码是有悖于 12-Factor 原则的。解决方案是将共享的代码拆分为独立的类库，然后使用 依赖管理 策略去加载它们。

尽管每个应用只对应一份基准代码，但可以同时存在多份部署。每份 部署 相当于运行了一个应用的实例。通常会有一个生产环境，一个或多个预发布环境。此外，每个开发人员都会在自己本地环境运行一个应用实例，这些都相当于一份部署。

![](https://12factor.net/images/codebase-deploys.png)

所有部署的基准代码相同，但每份部署可以使用其不同的版本。比如，开发人员可能有一些提交还没有同步至预发布环境；预发布环境也有一些提交没有同步至生产环境。但它们都共享一份基准代码，我们就认为它们只是相同应用的不同部署而已。



### 12-factor apps using Kubernetes

12 Factor App的原则1是“版本控制中的一个代码基准，许多部署”。

对于Kubernetes应用程序，这个原则实际上嵌入了容器编排本身的本质。通常，您使用源控制存储库（如git repo）创建代码，然后在Docker Hub中存储特定版本的映像。当您将要编排的容器定义为Kubernetes Pod，Deployment，DaemonSet的一部分时，您还可以指定镜像的特定版本，如：

```
...
 spec：
       containers：
       -  name：AcctApp 
         image：acctApp：v3
 ...
```

通过这种方式，您可以在不同的部署中运行多个版本的应用程序。

应用程序的行为也可能有所不同，具体取决于它们运行的配置信息。


