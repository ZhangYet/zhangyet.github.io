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