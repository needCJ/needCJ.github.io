---
title: Debezium
cover: /img/debezium-logo.png
thumbnail: /img/debezium-logo.png
date: 2023-09-07 16:12:54
categories: [Debezium,tutorial]
tags: Debezium
---


### 背景
对于一些大公司，每次到一定阶段都会面临数据积攒带来的架构上的瓶颈，到最后发现当前的数据库性能以及机器的硬件已经满足不了目前整体架构的需求。因此不得不考虑更换数据库，但是更换数据库就要面临一个新的挑战，那就是如何保证最高效以及最安全的**数据迁移**？这就是下面我要介绍的主角**Debezium**

<!-- more -->
### What is it?
[Debezium](https://debezium.io/)是国外一款开源的数据迁移框架，现已支持多种数据库:<u> MySQL</u>, <u> MongoDB</u>, <u> PostgreSQL</u>, <u> Oracle</u>, <u> SQL Server</u>, <u> Db2</u>, <u> Cassandra</u>, <u> Vitess</u>, <u> Spanner</u>，<u> Jdbc</u>。 这套框架最牛B的是支持准实时数据同步，其底层逻辑是读取数据库日志文件(比如redolog，binlog，或者伪装从库来发起同步请求 )。给你的感觉就好像在每一个表中加了触发器一样，自动将你insert，update，delete语句给抓出来。如果是大型公司出身的小伙伴应该都知道要想人家oracle官方人员来公司解决数据迁移这个case人家是要从出门就开始计费的！

#### Debezium架构图
<img src="/img/data_migration.png"/>

- 通过这个架构图可以清晰的看见整个架构及中间件，其大体流程是Debezium读取数据库的log，拿到新进来的数据，然后整合kafka发送到topc中，下游进行消费。相信玩儿过大数据的朋友们对kafka都不会陌生，削峰填谷，大吞吐量。并且发送到kafka后下游消费者是多样性的，本人曾集成flink，对当日数据进行清洗后存入指定表做数据分析，以及再次分发给后端做用户行为分析。

- 本人在公司已带领团队成功完成<u> oracle</u> to <u> PostgreSQL</u> ,单表10亿左右数据量的迁移任务。如果哪个小兄弟有相关业务可以联系我哦，QQ:1214818206。这次先抛个砖，等抽空会拿debezium的源码做分析，后续会在此blog中更新，溜了溜了
