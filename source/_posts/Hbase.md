---
title: Hbase
date: 2020-08-19 00:06:34
index_img: /resource/img/hbase.jpeg
category: 框架
tags: Hbase
---

# 概念

base 是分布式、面向列的开源数据库(其实准确的说是面向列族)。HDFS 为 Hbase 提供可靠的底层数据存储服务,MapReduce 为 Hbase 提供高性能的计算能力,Zookeeper 为 Hbase 提供稳定服务和 Failover 机制,因此我们说 Hbase 是一个通过大量廉价的机器解决海量数据的高速存储和读取的分布式数据库解决方案。


# 列式存储

列方式所带来的重要好处之一就是,由于查询中的选择规则是通过列来定义的,因此整个数据库是自动索引化的。