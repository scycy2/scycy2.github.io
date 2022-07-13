---
title: Transactions
date: 2022-07-13 14:11:06
tags: ['SQL', 'Big Data']
categories: ['self-study notes']
cover: 
summary: Transactions in MySql
img: /medias/featureimages/36.jpg
---

### 1. Definition

* A group of SQL statements that represent a single unit of work
* All the statements should be completed successfully, or the transaction will fail

### 2. ACID Properties

* **Atomicity**
* **Consistency**
* **Isolation**
* **Durability**

### 3. Create Transactions

```sql
START TRANSACTION;

INSERT(DELETE, UPDATE) ...;

COMMIT;
```

### 4. Concurrency Problems

* Lost Updates

  * Transaction commits last will overwrite changes make earlier
  * Use Lock Machenism

* Dirty Reads

  * When a transaction reads data that hasn't been committed

* Non-repeating Reads

  <img src="Transactions/Screen Shot 2022-07-05 at 12.49.29.png" style="zoom:50%;" />

* Phantom Reads

  * When reading some lines, several lines are changed or new lines are inserted

  <img src="Transactions/Screen Shot 2022-07-05 at 12.52.59.png" style="zoom:50%;" />

### 5. Transaction Isolation Levels

* <img src="Transactions/Screen Shot 2022-07-05 at 12.56.04.png" style="zoom:50%;" />

* **READ UNCOMMITTED**
  * 如果一个事务已经开始写数据，则另外一个事务不允许同时进行写操作，但允许其他事务读此行数据，该隔离级别可以通过“排他写锁”，但是不排斥读线程实现。这样就避免了更新丢失，却可能出现脏读，也就是说事务B读取到了事务A未提交的数据
* **READ COMMITTED**
  * 如果是一个读事务(线程)，则允许其他事务读写，如果是写事务将会禁止其他事务访问该行数据，该隔离级别避免了脏读，但是可能出现不可重复读。事务A事先读取了数据，事务B紧接着更新了数据，并提交了事务，而事务A再次读取该数据时，数据已经发生了改变。
* **REPEATABLE READ**
  * 可重复读取是指在一个事务内，多次读同一个数据，在这个事务还没结束时，其他事务不能访问该数据(包括了读写)，这样就可以在同一个事务内两次读到的数据是一样的，因此称为是可重复读隔离级别，读取数据的事务将会禁止写事务(但允许读事务)，写事务则禁止任何其他事务(包括了读写)，这样避免了不可重复读和脏读，但是有时可能会出现幻读。(读取数据的事务)可以通过“共享读镜”和“排他写锁”实现。
* **SERIALIZABLE**
  * 提供严格的事务隔离，它要求事务序列化执行，事务只能一个接着一个地执行，但不能并发执行，如果仅仅通过“行级锁”是无法实现序列化的，必须通过其他机制保证新插入的数据不会被执行查询操作的事务访问到。序列化是最高的事务隔离级别，同时代价也是最高的，性能很低，一般很少使用，在该级别下，事务顺序执行，不仅可以避免脏读、不可重复读，还避免了幻读
