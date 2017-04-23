---
layout: page
title: About
---

张晔
====

# 工作经历

- 2015-07-02 至今: 严肃科技后台工程师。

# 项目经验

## **prometheus** ENJOY搜索系统

    一个基于 tornado 的服务，为上游的 Java 应用提供 RESTful 接口。第一版的搜索功能基于 sphinx 的倒排索引，后来改用 Elastic Search。我在这个项目中的主要工作是更新、维护索引，实现产品逻辑（搜索排序和产品推荐）。然而这个系统的代码质量很糟糕，相关的推荐部分是数据部门离线算出评分，然后综合索引排序得到的。

## **Eru** ENJOY docker 云平台

    Eru 是目前 ENJOY 使用的平台。

1. **eru-core** Eru 调度编排系统

    `go` 开发的调度编排系统，负责根据使用者的需求创建/删除容器。我在该项目的主要工作是实现了一个动态规划算法（有多个版本），该算法可以保障容器“均匀地”分布在不同的节点上。

1. **eru-agent**

    `go` 开发的 agent 组件。部署在各个 docker 服务器上，收集监控指标、日志和健康检查。我实现了监控指标部分的功能。

1. **spike** graphite proxy

    `python` 开发 graphite proxy。其实就是用 `SWGI` 将 carbon-c-relay 的一致哈希算法，实现成 python 版本，这个 proxy 可以允许 graphite 进行横向扩展。

1. **erulb** Eru load balance

    `openresty` 实现的 load balance。

1. **suramar** redis 调度平台

1. **日志系统**

## **业务相关**

1. **twilight** 回源服务

## **数据相关**

1. **餐馆分类评分**