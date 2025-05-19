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
* 2021.10-至今 **Shopee Singapore**

	转岗到 Shopee Singapore 之后，主要负责各种系统工具的开发，与 Shopee 日常业务有一定距离。
    1. Ksnoop 系统可观测性项目。该项目主要包含两个组件：使用 go 语言开发的 os-agent 以及混合使用 go 和 bash script 的命令行工具 sysadmin 。
	   其中 os-agent 负责采集系统指标，包括使用 **ebpf** 采集的内核数据以及其他硬件信息。sysadmin 是 os-agent 的客户端以及其他系统管理工具的入口。
	   os-agent 监控了包括 CPU、磁盘和网卡在内的指标，os-agent 通过上报 prometheus 兼容的指标，可以无缝接入监控系统。ysadmin 提供了内核和热补丁管理功能。
* 2020.04-2021.10 **Shopee Shenzhen**

    2020年加入 Shopee Shenzhen 前期维护 Shopee Airpay (后合并进 Shopeepay)的促销功能, 后期维护 Shopeepay 的 core 模块。

    1. 维护 Airpay promotion 系统，采用 go 开发，包括促销管理工具，为支付流程提供促销计算 rpc 接口（上游传入订单，该接口校验用户使用的促销优惠并计算优惠后的价格）。
	2. 维护 SPM(shopeepay module) core 模块，采用 python 开发，主要管理订单生成以及支付、发货前后的状态管理。

* 2019.04-2020.01 **SixteenMarkets**
	2019年应朋友邀请加入 SixteenMarkets 这是一家股票公司，很可惜的是，入职不到一年，它倒闭了。我开发的产品来不及上线。

    1. [fix simulator](https://zhangyet.github.io/archivers/fix_simulator): 熟识 [FIX 协议引擎](https://zhangyet.github.io/archivers/quickfixgo)，在 gRPC 服务中嵌入 lua runtime 实现。

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

## 开源社区贡献
### bcc 社区贡献
1. [Add cpu filter to hardirqs/softirqs tools](https://github.com/iovisor/bcc/pull/5107)
2. [libbpf-tools: add cpu filter for hardirqs/softirqs](https://github.com/iovisor/bcc/pull/5055)
### bpftrace 社区贡献：
1. [Add -d option in aot](https://github.com/bpftrace/bpftrace/pull/3363)
2. [Opti `path` buildin in bpftrace](https://github.com/bpftrace/bpftrace/pull/3401)
3. [Fix cgroup.path* unit test](https://github.com/bpftrace/bpftrace/pull/3434)


## 工作技能

* Go: 2016年开始使用 Go 进行开发，2017年之后主要使用 Go 进行业务开发
* Python: 2015年-2016年使用 Python 进行业务开发
* git: 能熟练使用 git 进行版本控制
* shell: 有一定 shell 脚本开发经验
* 其他：lua, docker, nginx
