2026-10-24 21:00

Tags: [[3 - Tags/concurrency|concurrency]] [[3 - Tags/database|database]]

# Race Conditions
程序的结果取决于多个操作的交错顺序，某些顺序会产生错误。

### 例子：抢最后一件商品

初始库存为 1。程序先读取库存，在应用中减 1，然后把计算好的固定值写回。

| 顺序 | 请求 A | 请求 B |
| --- | --- | --- |
| 1 | 读取库存，得到 1 | |
| 2 | | 读取库存，得到 1 |
| 3 | 把库存写为 0，创建订单 | |
| 4 | | 把库存写为 0，创建订单 |

结果：库存是 0，却创建了两笔订单。每个请求单独执行都正常，交错执行则可能超卖。

问题不要求两个操作发生在同一纳秒：**检查条件与采取行动之间，其他请求可能已经改变了数据。**

### 不安全的读后写

以下是应用交互流程示意，不是可以直接复制运行的完整业务脚本：

```sql
BEGIN;

SELECT stock FROM products WHERE id = 42;
-- 应用读到 1，判断允许购买，在应用中算出 new_stock = 0。

UPDATE products SET stock = 0 WHERE id = 42;
INSERT INTO orders (product_id, quantity) VALUES (42, 1);

COMMIT;
```

在 PostgreSQL 默认的 Read Committed 下，两个事务可能都先读到 1。第二个更新即使等待第一个事务释放行锁，之后仍可能把固定值 0 再写一次，两笔订单也都提交。

因此，**仅加 BEGIN/COMMIT 并未修复这段逻辑。**

### 一种修复：条件更新并检查结果

把“还有库存”和“扣减库存”放入同一条数据库更新：

```sql
BEGIN;

UPDATE products
SET stock = stock - 1
WHERE id = 42 AND stock > 0
RETURNING id;

-- 应用必须检查结果：
-- 返回一行：才执行下面的订单插入。
-- 返回零行：停止购买流程，ROLLBACK，并返回售罄或商品不存在。

INSERT INTO orders (product_id, quantity) VALUES (42, 1);

COMMIT;
```

这里的分支由应用执行，不能忽略注释后无条件运行 INSERT。

在 Read Committed 下，另一个更新等待后，PostgreSQL 会针对更新后的目标行重新判断 WHERE 条件。库存已经变成 0 时，条件不再满足，第二次扣减不会成功。

扣库存与创建订单仍应处于同一个事务：订单创建失败时，也要撤销扣库存。这是针对单商品库存分配的解决方案，不是所有并发业务的通用答案。


## Related Concepts
- [[PostgreSQL Transactions and Isolation Levels]]：事务边界与 Serializable 是另一种处理上述竞态的方式。
- [[Deadlocks]]：加锁也可能带来循环等待。
- [[Concurrency vs Parallelism]]：竞争条件来自操作交错，不要求真正并行。

# References
[[2 - Source Materials/Course/Backend Fundamentals 01 - Transactions and Concurrency|Backend Fundamentals 01 - Transactions and Concurrency]]
- [PostgreSQL Read Committed](https://www.postgresql.org/docs/current/transaction-iso.html#XACT-READ-COMMITTED)
