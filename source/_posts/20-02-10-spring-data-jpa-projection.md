---
title: 20-02-10 spring data jpa projection
mathjax: true
date: 2020-02-10 10:38:54
tags:
- spring data jpa
categories:
- springboot
---

需求

+ 查询数据时只需要实体的部分字段
+ 一对多关联`@OneToMany`的多端一起查出来，并且**One 端只需要部分字段**
+ 解决循环依赖

使用技术

1. `spring data jpa` 投影 Projection : 获取部分属性，包括集合类型的属性
2. `@JsonIgnoreProperties("xxx")` 注解 : 解决序列化循环引用
3. 查询方法需要是`findxxxBy`   e. g. : `List<xxxProjection> findxxxBy();`

<!--more-->