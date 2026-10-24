2026-10-24 21:00

Tags: [[3 - Tags/concurrency|concurrency]] [[3 - Tags/database|database]]

# Deadlocks
多个事务互相等待对方持有的资源，形成无法自行前进的等待环。

### 例子：以相反顺序处理两件商品

1. 事务 A 锁住商品 X。
2. 事务 B 锁住商品 Y。
3. A 想修改 Y，但 Y 被 B 锁住，于是等待。
4. B 想修改 X，但 X 被 A 锁住，也开始等待。

普通的 UPDATE 就可能取得行锁，不需要显式写 LOCK 才会产生死锁。

PostgreSQL 会检测死锁并中止其中一个事务，使另一个能够继续。不能依赖某个固定事务总是被选中中止。

### 常见处理方式

- 按统一顺序获取锁，例如始终按商品 ID 从小到大处理，避免上述反向获取模式。
- 缩短事务，不要持锁等待用户输入或慢速外部 API。
- 应用识别死锁失败，必要时进行有界重试，重新执行整个事务及其业务判断。
- 重试必须考虑外部副作用，不能重复扣款或重复发出不可撤销的操作。

### 与竞争条件的区别

- **竞争条件：** 交错执行导致结果错误。
- **死锁：** 相互等待导致事务无法继续。

加锁可以解决某些竞争条件，但锁的获取顺序不当又可能引入死锁。单纯等待一个锁不等于死锁，关键是是否形成等待环。


## Related Concepts
- [[Race Conditions]]
- [[PostgreSQL Transactions and Isolation Levels]]
- [[Concurrency vs Parallelism]]

# References
[[2 - Source Materials/Course/Backend Fundamentals 01 - Transactions and Concurrency|Backend Fundamentals 01 - Transactions and Concurrency]]
- [PostgreSQL Deadlocks](https://www.postgresql.org/docs/current/explicit-locking.html#LOCKING-DEADLOCKS)
