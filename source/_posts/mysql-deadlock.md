---
title: 数据库死锁Deadlock
date: 2018-07-04 17:53:54
categories: [database, mysql]
tags: [mysql]
---

数据库死锁怎么形成？遇到死锁该怎么应对解决？

<!-- more -->

---

> 小引：之前一直只是对数据库死锁略有耳闻，没有深入的了解，也没机会遇上这类问题，今天查看`Nifi`更新数据仓库的错误日志，有幸遇到一起“死锁事故”，经大神分析了一波，终于略有收获。

#### 起因
- `Nifi`是`apache`推出的一款专门用来做数据拉取和转换以构建数据仓库的开源项目，我们项目也是基于这个目的在做数据拉取和增量同步更新。

#### 错误信息
```
Unable to execute SQL select query update xxx set col_2=xxx where col_1=xxx and col_2=xxx and col_3<xxx
due to com.mysql.jdbc.exceptions.jdbc4.MySQLTransactionRollbackException: 
    Deadlock found when trying to get lock; try restarting transaction; routing to failure: 
com.mysql.jdbc.exceptions.jdbc4.MySQLTransactionRollbackException: 
    Deadlock found when trying to get lock; try restarting transaction
```

#### 初步分析
- 这是一条`update`语句，`where`子句里三个字段`col_1、col_2、col_3`都分别加了索引，于是按照这个子句，我去数据仓库查了一下，发现没有一条数据……
    ```sql
    select * from xxx where col_1=xxx and col_2=xxx and col_3<xxx
    ```
- 没有数据，怎么会出现死锁呢？最初我的理解就是：没有数据也就意味着没有更新操作，也就没必要加写锁……很显然并不是这样，并不是没有数据


#### InnoDB的加锁
- 如果我们对上面的语句`EXPLAIN`一下，会发现，InnoDB只用了一个索引`col_1`（PS：并不是因为这个字段在第一个位置，应该是这个字段的区分度比较明显，InnoDB会对给定的几个独立索引字段进行筛选，选出最优化的索引进行查询）
- 也就是说，对于InnoDB而言，它只负责找到索引能找到的行，交给你`Mysql`自己再根据别的条件`col_2、col_3`筛除
- 于是，我按`col_1`查询了一下，发现有三条数据
    ```sql
    select * from xxx where col_1=xxx
    ```
- 于是，InnoDB就对这三行数据加了排他锁

#### 死锁的形成
- 至此可能还没有明确死锁出现的根本原因，因为所谓死锁，只有当出现多个事务同时竞争同一资源的相同锁的时候才会出现
- 实际上，是由我们`Nifi`的流程导致的，因为出现了一个ID对应有三条更新数据，所以`Nifi`会开启三个线程对该数据进行更新，三个线程开启三个事务，如果这时出现下面的情形，则会出现死锁：
  - 事务A刚拿到第一条数据的写锁，要去拿第二条数据的写锁
  - 事务B也刚拿到第二条数据的写锁（因为第一条数据的写锁在事务A那里，事务B只能先拿第二条），要去拿第一条数据的写锁
  - 这个时候，两个事务对这两条数据的锁还没释放，就出现了锁竞争——死锁
- 理论上，数据拉取和更新会依据数据唯一ID来严格执行，也就意味着同一个ID的数据一次只允许出现一条。
- 也就是说，实际上还是因为我们本身流程上的BUG导致的，所幸，我们找到了原因——数据为什么一次会出现三条的原因，这是后话了……