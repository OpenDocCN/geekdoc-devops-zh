# 数据库版本化——你可以对它们进行版本化？！装具

> 原文：<https://www.harness.io/blog/database-versioning-you-can-version-those>

我们将每天部署多次，以至于我们将在 [Accelerate](https://www.amazon.com/Accelerate-Software-Performing-Technology-Organizations/dp/1942788339) 中脱颖而出，成为 [DevOps 企业峰会](https://events.itrevolution.com/us)上所有人羡慕的对象。有一天，一个想法出现了，需要在数据库中增加一列。很简单，让我们用一个 [DDL](https://en.wikipedia.org/wiki/Data_definition_language) 来修改模式，我们就都准备好了。没那么快。一些人现在可能正在咬牙切齿地考虑引入数据库变更，放弃连续交付的敏捷性。你的数据通常被视为[致命弱点](https://en.wikipedia.org/wiki/Achilles'_heel)是有充分理由的。数据库版本控制工具的兴起旨在为您的关系数据库这一薄弱环节增加灵活性。

## (关系型)数据为什么硬？

在一天结束的时候，为了让一个应用程序有价值，必须在某个地方存储一些东西。应用程序状态可能存在于[内存解决方案](https://hazelcast.com/glossary/memory-caching/)中，但是持久数据井需要持久存储在某个地方。如果对底层模式进行更改，实际上可能会失去向后兼容性。添加一个新的表，甚至添加新的列，必须恢复可能意味着麻烦。想象一下直接在数据库上执行脚本的次数(不要告诉你的 [DBA](https://en.wikipedia.org/wiki/Database_administrator) )。尽管我们努力实现环境之间的配置控制，但是有人对存储过程、索引、模式等进行了更改。通常有两种类型的状态/数据。用户会话所需的事务状态/数据，以及更持久/静态的数据，如用户记录。交易数据非常简单。如果用户没有会话，则没有数据。使用内存解决方案非常适合这种情况。挑战在于更持久的记录。还记得 [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) 【创建，读取，更新，删除】吗？您的永久记录会受到这些操作的影响。大多数人都有一个 DDL 来描述数据在开始时的样子。尽管随着时间的推移，想象一下会发生多少次更新和删除。如果您必须以编程方式重新创建，您将看到从最后一个 DDL 开始，或者可能从备份中恢复并应用更改会更慢。对于数据库，回滚也面临同样的挑战。作为一种良好的做法，您将在接近迁移的时间点进行备份以进行恢复。根据不同的更改，如果您的更改包括执行[语句，那么](https://en.wikipedia.org/wiki/Prepared_statement)可以保存在您的 [RDBMS 的](https://en.wikipedia.org/wiki/Relational_database) [撤销空间](https://docs.oracle.com/cd/B19306_01/server.102/b14231/undo.htm)中。与向前推进类似，以编程方式进行回滚可以提高自动化程度。数据库版本管理工具允许程序化迁移。

## 变化的潮汐，你好 DBs 的版本控制

数据世界的潮流肯定在改变。随着 DevOps 运动对应用程序堆栈的推动，数据库堆栈也被迫跟上。现代数据库架构正在进行一些实践。新的事务性数据已经从关系型解决方案转移到[NoSQL](https://en.wikipedia.org/wiki/NoSQL)/非结构化/内存解决方案，采用现代集群和复制机制，允许以更安全的方式传播更改。另一个实践是开始将[版本控制](https://en.wikipedia.org/wiki/Version_control)引入到数据库中。您的应用程序堆栈已经享受了多年的相同版本控制，例如合并和增量现在正在成为您的数据库的选项。两个流行的工具是 [Liquibase](https://www.liquibase.org/) 和 [Flyway](https://flywaydb.org/) ，允许对你的数据库进行编程版本控制。尽管版本化工具通常要求所有团队都采用该工具；如果一个团队不这样做，那么订单/版本控制肯定会被抛弃。 [DBMS Tools 有一个可靠的数据库版本管理工具列表](https://dbmstools.com/version-control-tools)。Flyway by [Redgate](https://www.red-gate.com/) 有关于[开始使用](https://flywaydb.org/getstarted/how)关系数据库的优秀文档。随着 NoSQL 数据库(如 [Cassandra](https://cassandra.apache.org/) )的更新，人们将更多地关注 [Cassandra 环](https://cassandra.apache.org/doc/latest/operating/topo_changes.html)中的拓扑变化(如增加/减少节点),以实现容量和扩展目的，这当然可以实现自动化。无论您选择哪一级别的自动化，Harness 都会支持您。

## 用马具为你的旅程增压

在 Harness，我们不会解决您的数据库版本挑战，但会为您提供拥有世界级管道所需的所有工具。通过能够轻松创建管道，您/您的团队可以专注于自动化最后的前沿领域，例如，您的数据很可能比您在组织中存在的时间更长。当您经历数据库版本化之旅时，Harness 平台可以协助[工作流障碍](https://developer.harness.io/docs/continuous-delivery/cd-deployments-category/controlling-deployments-with-barriers-resource-constraints-and-queue-steps/)或[批准步骤](https://developer.harness.io/docs/platform/approvals/adding-harness-approval-stages/)，这是在版本化解决方案之前应用数据库更改的绝佳时机。随着您继续以编程方式对关系数据库进行版本控制，Harness 可以帮助编排版本控制所需的 API。干杯！拉维