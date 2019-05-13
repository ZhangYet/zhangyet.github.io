---
title: "张晔(dantezy)"
tags: ["go", "python", "cloud", "docker"]
date: 2019/04/04 11:19:53
permalink: /about/
excerpt: 个人简历，不定期更新。沽之哉，沽之哉！我待贾者也。
---

## 联系方式

* zhangyet@gmail.com
* 15101026275

## 学历

* 2009.09-2013.06 本科 中山大学 数学与应用数学专业
* 2013.09-2015.06 硕士 中山大学 计算数学

## 工作与项目经验

* 2019.04-至今 **SixteenMarkets**

    1. [fix simulator](fix_simulator)

* 2017.09-2019.03 **滴滴云** 滴滴云用户系统

    滴滴云用户系统是滴滴云主要入口，具有用户注册、用户管理、资费管理（资源计费与回收）、MIS 等功能。我负责日常功能维护与新产品接入等工作。用户系统作为滴滴云入口平台，主要包括如下模块：

    1. `proxy_pass`: 鉴权、订单创建与流量转发。
    1. `api`: 用户管理（注册、创建组用户以及邀请入组等）。
    1. `metering`: 通过订阅`异步队列`,记录虚拟云资源的状态变化。
    1. `pay`: 询价与充值相关逻辑。
    1. `task_center`: 定时与延时任务。包括资源的计费与回收。资源计费涉及从 metering 数据库中读取资源状态，分割计费周期，计算每个计费周期的费用。

    用户系统自我参加工作以来，经历多轮迭代，接入的产品线从单一的 `zstack` 产品扩展到包括存储产品、安全产品和数据库产品等多种不同形态的产品线。进行了**资源接口规范化**、**分库分表**、**metering模块抽象化**、**计费任务模块化**和**多region改造**等迭代升级，我参与其中所有模块的开发。

* 2015.07-2017.08 **严肃科技** Docker 平台系统 Eru

    平台系统 Eru 是一个集 CI/CD、服务发现和服务治理于一体的综合系统。每个服务更新之后会通过 CI 自动打包，生成镜像。开发人员可以通过界面或命令行更新版本。更新时，通过 Eru 系统的核心组件 eru-core 计算每个服务所需的资源（CPU和内存，可以根据业务内容选择资源维度，即优先考虑CPU资源和优先考虑内存维度），将服务对应的新容器部署到不同的节点上。每个节点上部署了 eru-agent 进行数据收集和节点状态监测。容器更新并通过健康检查之后，在更新 load balancer 组件进行平滑升级。

    主要参与开发：

    1. `eru-core`: go 开发的调度编排核心。
    1. `agent`: 每个节点部署的 agent 组件，负责数据收集和容器监控。
    1. `eru load balancer`: 基于 `openresty` 开发的 load balancer 组件。
    1. `日志系统`: 基于 `rsyslog` 开发的日志收集，展示系统。（这个系统有个缺陷，它没有全链路 trace，不利于定位问题）

## 工作技能

* Go: 2016年开始使用 Go 进行开发，2017年之后主要使用 Go 进行业务开发
* Python: 2015年-2016年使用 Python 进行业务开发
* git: 能熟练使用 git 进行版本控制
* shell: 有一定 shell 脚本开发经验
* 其他：lua, docker, nginx