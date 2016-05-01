---
layout: post
title: "函数式编程技术交流会"
comments: true
---

## 张淞：代数数据类型通用编程

    1. 同构：存在从A到B，B到A的映射，两种映射组合成identity， 则A与B同构(自反， 对称，传递)；
    2. 代数类型及运算(0--+的单位元, 1--X的单位元, +--两种类型的不相交并, x--笛卡尔积)
    3. 相等类型类，用基本袋鼠累组合出同构类型;
    4. 问题：想定义一个类型类GEq:
        思路，将a b转换为基本代数类型同构的类A B，再比较A B是否相等
    5. GHC中的Generic(转换成代数类型的时候，meta信息会被丢掉)
    6. 参考文献：
        Less is more, generic programming theory and practice
        A Generic Deriving Mechanism for haskell

## Neo Line: Haskell在渣打的应用

    1. Haskell的优势：
       类型系统＋函数 => 业务需求；跨平台； 组合优于即成； 脚本，框架 系统， 集群； 强类型 => 业务校验；面向客户开发； 工具链完备

## 邵成: Expression Problem

    1. 怎样做DSL解释器
    2. FP: easy to add functions, hard to add constructs; OO is on the other side.

## 范畴论
