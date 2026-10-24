2026-10-03 15:43

Tags: [[3 - Tags/database|database]] [[3 - Tags/sql|sql]] [[3 - Tags/concurrency|concurrency]]

# PostgreSQL Transactions and Isolation Levels

## Core Idea
- 事务把操作放在同一提交或回滚边界内，但不是自动覆盖全部请求的互斥锁。
- 原子性确保不只执行一半；一致性要求结果保持约束与业务不变量。
- 隔离级别控制并发交互，持久性关注配置保证下的提交结果保留。
- [[Race Conditions]] 中的最后一件商品案例，展示了为什么只有事务仍可能超卖。

## Database Transactions：数据库事务

事务把一组数据库操作作为一个工作单元，一起提交或一起撤销。

购买商品包含扣库存和创建订单。如果扣库存成功而创建订单失败，就不应留下“库存减少但没有订单”的半成品状态。

- BEGIN：开始事务。
- COMMIT：提交事务。
- ROLLBACK：撤销尚未提交的事务修改。

### 事务的边界

数据库事务不等于“这段代码只有一个请求能执行”。并发行为由隔离级别、查询写法和锁等共同决定。

数据库事务通常也不覆盖外部系统：

- 数据库回滚不会自动撤回已经发出的邮件。
- 数据库回滚不会自动退回支付平台已经扣除的钱。

涉及外部操作时，需要额外的幂等、消息或补偿设计，不能把数据库回滚视为通用撤销按钮。

## ACID Properties：事务的四项性质

ACID 描述正确、可靠事务应具备的性质，不应简单理解为数据库自动赠送的四个万能功能。

### A — Atomicity：原子性

事务内的数据库操作要么整体生效，要么整体撤销。

它回答的是：**有没有只做一半？**

“原子”在这里并不意味着执行期间绝不与其他事务交错。

### C — Consistency：一致性

事务应把数据从符合约束和业务不变量的状态，带到另一个符合这些规则的状态。

它回答的是：**做完后，状态还合法吗？**

#### 为什么这还需要单独列为性质？

假设账户 A 和 B 各有 100 元，总额为 200 元。转账代码错误地执行：

1. A 扣除 20 元。
2. B 增加 30 元。

数据库可以把这两步整体提交，因此满足原子性；但总额变成 210 元，违反了“这次转账不创造或销毁资金”的业务不变量。

**完整执行错误逻辑，仍然是错误。** 这就是 A 与 C 的区别。

#### 谁负责保证 C？

- 数据库负责执行已声明的约束，例如 CHECK、UNIQUE 和 FOREIGN KEY。
- 应用负责正确表达业务逻辑，并采用能维持业务规则的并发控制方式。
- 数据库不会自动理解需求文档中的全部规则，也不会自动发现“应加 20，却写成加 30”。

例如，余额减到 -50 在数值类型上完全合法。如果声明 CHECK (balance >= 0)，数据库就会拒绝该非法更新；如果没有这种约束，事务本身不会凭空知道“余额不能为负”。

即使库存受 CHECK (stock >= 0) 保护，前面的超卖例子仍可能留下库存 0 和两笔订单。因此，一个字段合法不代表整个业务状态正确。

这里的 Consistency 是数据完整性和业务不变量，**不是副本同步，也不是分布式系统中的最终一致性。**

### I — Isolation：隔离性

规定多个事务并发运行时，可以看见什么，以及允许怎样相互影响。

它回答的是：**别人同时操作，会怎样影响我？**

隔离有不同强度，使用事务不代表默认获得最高隔离。

### D — Durability：持久性

事务确认提交后，其结果应在故障恢复后仍然保留。

它回答的是：**确认完成后，会不会丢失？**

这一保证依赖正确的数据库持久化配置和可靠存储。它不代表磁盘全部损毁后仍然可恢复，也不能代替备份。

## Isolation Levels：隔离级别

隔离级别决定事务能看见哪些并发变化，以及数据库要防止哪些并发异常。以下实现细节以 PostgreSQL 为准。

### Read Uncommitted：读未提交

按照该级别允许的最低标准，可能读到其他事务尚未提交、以后也可能撤销的数据，即脏读。

PostgreSQL 中，选择这个级别实际上按 Read Committed 执行，不提供这种脏读行为。

### Read Committed：读已提交

这是 PostgreSQL 的默认隔离级别。普通查询使用语句级快照，看到该查询开始前已经提交的数据，也能看见自己事务之前的修改。

例如，在同一个事务里：

1. 第一次查询库存，得到 10。
2. 另一个事务把库存改成 9，并提交。
3. 第二次查询可能得到 9。

同一行重复读取发生变化，叫不可重复读。

“读取的是已提交数据”不代表这个数据一直有效，也不保证根据旧读取结果作出的决定仍然正确。

### Repeatable Read：可重复读

PostgreSQL 的事务使用一份稳定快照；快照建立于事务内第一条非事务控制语句，而不一定是 BEGIN 时。事务仍能看见自己的修改。

在上面的例子中，两次普通查询都会读到 10，即使另一个事务已经提交了 9。

PostgreSQL 的 Repeatable Read 还可以防止幻读，即同一查询条件的匹配行集合在重复查询时受其他事务提交影响而变化。但它仍不等于 Serializable。

#### 写偏斜：稳定快照不保证联合决策合理

规则：医生 A、B 至少一人值班。

1. A 的事务看到 B 在值班，把 A 改成休息。
2. B 的事务看到 A 在值班，把 B 改成休息。
3. 两个事务修改不同的行，都可能提交。
4. 最后无人值班，违反规则。

这是一种写偏斜。每个事务基于自己的稳定视图作出的决定看起来合理，合在一起却不合法。

### Serializable：可串行化

要求成功提交的事务的整体效果，等价于按某个顺序逐个执行这些事务。

它不是物理上让所有事务一次只能运行一个。PostgreSQL 可以并发执行，但发现冲突时可能中止事务，应用必须重新执行整个事务。

#### 能否解决抢最后一件商品的问题？

可以，前提是“读库存 → 判断 → 扣库存 → 创建订单”完整地位于同一个 Serializable 事务中，相关并发操作也采用适当的控制方式。

可能发生的过程：

1. A、B 都读取到库存 1。
2. A 完成扣库存与建订单，提交。
3. B 遇到序列化失败，被中止。
4. 应用从头重试 B 的事务，重新读取到库存 0。
5. B 返回售罄，不再创建订单。

**Serializable 不一定阻止两个事务都先读到 1；它阻止两个不符合串行执行结果的购买事务都照原样成功提交。**

#### 重试注意事项

- 重试整个事务，包括查询和业务判断，不只是最后一条失败 SQL。
- 不要把库存检查放在事务外，再把旧判断带入事务。
- 使用有界重试并处理最终失败，不要无限重试或假装成功。
- 不要无保护地重复外部扣款、邮件等副作用。
- Serializable 不能修复单个事务自身的错误逻辑，例如转账减 20 加 30。


## 稳定快照的报表示例
For a report that must keep a stable snapshot across multiple queries:

```sql
BEGIN ISOLATION LEVEL REPEATABLE READ READ ONLY;
SELECT albumid, title
FROM album
WHERE artistid = 1;

SELECT albumid, SUM(milliseconds) AS duration_ms
FROM track
GROUP BY albumid;
COMMIT;
```


## Correctness Decisions
- At read committed, two queries can observe different committed states even inside the same transaction.
- A single set-based query can avoid some cross-query inconsistencies, but it does not replace all concurrency controls.
- Snapshot stability alone does not enforce every cross-row business invariant.
- Keep transactions short and avoid waiting for user input while holding them open.
- Retry the entire transaction after a serialization failure, with bounded retries and safe handling of external side effects.
- PostgreSQL Repeatable Read 也可能发生序列化失败，并非只有 Serializable 才需要准备重试。

## Related Concepts
- [[Race Conditions]]：固定值写回与条件更新的库存示例。
- [[Deadlocks]]：并发事务互相持锁等待。
- [[SQL vs Application Business Logic]]：选择数据库与应用之间的业务规则边界。
- [[psql Workflows]]：理解交互式事务状态。

# References
[[2 - Source Materials/Course/Backend Fundamentals 01 - Transactions and Concurrency|Backend Fundamentals 01 - Transactions and Concurrency]]
[[2 - Source Materials/Books/The Art of PostgreSQL/2 - Software Architecture|2 - Software Architecture]]
[[2 - Source Materials/Books/The Art of PostgreSQL/4 - Business Logic|4 - Business Logic]]
[[2 - Source Materials/Books/The Art of PostgreSQL/6 - Psql|6 - Psql]]
- [PostgreSQL Transactions](https://www.postgresql.org/docs/current/tutorial-transactions.html)
- [PostgreSQL Isolation](https://www.postgresql.org/docs/current/transaction-iso.html)
- [PostgreSQL Glossary](https://www.postgresql.org/docs/current/glossary.html)
