2026-10-24 21:00

# Backend Fundamentals 01 - Transactions and Concurrency

## Headline
事务保证一组操作的提交边界，不自动保证业务代码正确，也不自动消除竞争条件。

## Summary
这份课程笔记保存对竞争条件、死锁、数据库事务、ACID 和隔离级别的讲解要点。实现细节以 PostgreSQL 官方文档为依据；重点是分清原子性、业务不变量与并发隔离，避免把“支持事务”误当成“无需并发设计”。这里保存技术内容，不记录私人对话。

## 抢最后一件商品
- 库存为 1，A、B 都先读到 1，再分别写回固定值 0 并创建订单，会留下两笔订单。
- 在 Read Committed 下，只加 BEGIN/COMMIT 不能消除这个竞争条件。
- 针对单商品分配，可以执行“stock > 0 才 stock = stock - 1”的条件更新；检查确实更新一行后才创建订单。
- 扣库存和创建订单应在同一事务内，避免订单失败但库存已经减少。

## 死锁与锁顺序
- A 持有 X 等 Y，B 持有 Y 等 X，形成循环等待。
- PostgreSQL 检测后会中止一个事务。应用需要处理失败，必要时安全地重试整个事务。
- 按统一顺序获取锁并缩短事务，可以降低风险；普通等待不等于死锁。

## ACID 的责任边界
- A：整组执行或撤销，不留下只完成一半的修改。
- C：事务前后的数据符合约束及业务不变量。
- I：规定并发事务可以看到什么以及如何相互影响。
- D：在配置的持久化保证下，已确认提交的结果在故障恢复后仍保留。
- 反例：转账扣 20 却加 30，即使整体提交满足原子性，仍违反资金守恒。
- 数据库执行已声明的约束，应用负责正确表达业务逻辑；数据库不会自动理解所有业务规则。
- ACID 的 C 不等于副本同步或最终一致性，D 也不能代替备份。

## PostgreSQL 隔离级别与重试
- Read Uncommitted 实际按 Read Committed 执行。
- Read Committed 的普通查询使用语句级已提交快照，同一事务两次查询可能不同。
- Repeatable Read 使用稳定快照，但仍可能发生跨行的写偏斜，例如两个值班医生同时退出。
- Serializable 要求成功事务的结果等价于某种串行顺序，不是强制物理上逐个执行。
- 两个购买事务仍可能都读到 1，但不能都以不符合串行结果的方式成功提交。
- 失败后应从头重试，包括重新读取和判断；不能只重发最后一条 SQL。
- 数据库回滚不能撤销已经发生的外部扣款或邮件，重试需要考虑幂等性。

## Key Ideas
- [[Race Conditions]]：检查与行动之间的状态变化。
- [[Deadlocks]]：锁形成的循环等待。
- [[PostgreSQL Transactions and Isolation Levels]]：事务、ACID、隔离强度和重试边界。
- 完整执行不等于正确执行，稳定读取不等于联合决策正确。

## Remarks
- 这是技术教学的来源记录；可持续更新的理解已整合到以上 Full Notes。
- 官方文档链接作为可核验依据，不将私人聊天链接发布到公开仓库。

Co-authored by ChatGPT

# References
- [PostgreSQL Transactions](https://www.postgresql.org/docs/current/tutorial-transactions.html)
- [PostgreSQL Transaction Isolation](https://www.postgresql.org/docs/current/transaction-iso.html)
- [PostgreSQL Deadlocks](https://www.postgresql.org/docs/current/explicit-locking.html#LOCKING-DEADLOCKS)
- [PostgreSQL Glossary](https://www.postgresql.org/docs/current/glossary.html)
