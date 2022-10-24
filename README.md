## 题目 01- 完成 ReadView 案例，解释为什么 RR 和 RC 隔离级别下看到查询结果不一致
要求：

<img width="451" alt="image" src="https://user-images.githubusercontent.com/96624836/197509777-4b0e511f-fed1-444b-ac95-0586f24eb16a.png">


### 完成案例 01- 读已提交 RC 隔离级别下的可见性分析
### 完成案例 02- 可重复读 RR 隔离级别下的可见性分析
用通俗易懂的方式记录整个案例过程，可以画图与截图
做完案例给出结论，并对结论进行分析
### 回答范式：
### 1. 案例 01- 读已提交 RC 隔离级别下的可见性分析

### 目标
### 操作步骤
### 实践过程

Session 1: set session transaction isolation level read committed;  
Session 2: set session transaction isolation level read committed;  
Session 3: set session transaction isolation level read committed;  
确认 transaction isolation 为 RC  
Session 1: select @@tx_isolation;  
<img width="116" alt="image" src="https://user-images.githubusercontent.com/96624836/197511134-5b78a182-e39c-4ae0-a75d-92a19b87aff4.png">  
Session 2: select @@tx_isolation;  
<img width="116" alt="image" src="https://user-images.githubusercontent.com/96624836/197511134-5b78a182-e39c-4ae0-a75d-92a19b87aff4.png">  
Session 3: select @@tx_isolation;  
<img width="116" alt="image" src="https://user-images.githubusercontent.com/96624836/197511134-5b78a182-e39c-4ae0-a75d-92a19b87aff4.png">

insert 初始数据是刘备  
Session 1: insert into tab_user values(1, '刘备', 18, '蜀国');  
Session 1: select * from tab_user;  
<img width="222" alt="image" src="https://user-images.githubusercontent.com/96624836/197512073-ae65e299-cad4-4e48-8939-5293bd29b972.png">


开启事务 1, 2, 3  
Session 1: begin;  
Session 2: begin;  
Session 3: begin;

Session 1: update tab_user set name = '关羽' where id = 1;

检查事务  
<img width="680" alt="image" src="https://user-images.githubusercontent.com/96624836/197514575-241bbd8b-5642-43f8-81e4-b84197f25a5a.png">

Session 1: update tab_user set name = '张飞' where id = 1;

检查事务  
<img width="677" alt="image" src="https://user-images.githubusercontent.com/96624836/197515445-e22b16d8-78d2-4403-abe2-6f7940e02594.png">

Session 2: update tab_user set name = '赵云' where id = 1;  
检查事务  
<img width="684" alt="image" src="https://user-images.githubusercontent.com/96624836/197515722-1d8dff41-ca71-4757-8ac5-37db1e1d9a9e.png">

Session 3: select * from tab_user;  
<img width="231" alt="image" src="https://user-images.githubusercontent.com/96624836/197516012-54ab66be-078e-4945-abef-4c8ed884a202.png">

Session 1: commit;  
检查事务  
<img width="679" alt="image" src="https://user-images.githubusercontent.com/96624836/197528820-23929c18-b19e-4d1b-b5a1-e4c46ed9d9ca.png">

Session 3: select * from tab_user;  
<img width="225" alt="image" src="https://user-images.githubusercontent.com/96624836/197529040-c6fc6e37-65ec-483e-a92a-6e96fa5581cf.png">

Session 2: update tab_user set name = '诸葛亮' where id = 1;
检查事务  
<img width="679" alt="image" src="https://user-images.githubusercontent.com/96624836/197531424-04fb8b08-48c4-4810-9b8a-3dfcf4c2442e.png">

Session 3: select * from tab_user;  
<img width="222" alt="image" src="https://user-images.githubusercontent.com/96624836/197531606-df9d5c41-866c-4520-872b-64fdeec05ad2.png">

Session 2: commit;  
检查事务  
<img width="679" alt="image" src="https://user-images.githubusercontent.com/96624836/197534317-e81466a2-62ac-4bcd-a820-eacb35557a6e.png">
  
Session 3: select * from tab_user;  
<img width="221" alt="image" src="https://user-images.githubusercontent.com/96624836/197534430-35dce708-2245-41e6-8eb3-d28d33e5881c.png">

Session 3: commit;  
检查事务  
<img width="681" alt="image" src="https://user-images.githubusercontent.com/96624836/197534759-de7e74b7-760f-4517-bdd7-ed7953626d51.png">

Session 3: select * from tab_user;  
<img width="225" alt="image" src="https://user-images.githubusercontent.com/96624836/197554047-4cba4138-4f5d-4457-93e8-ffd4644cb7ac.png">

### 结论  
在 RC 隔离级别下，一个已提交的事务可以被另一个事务读取。

### 2. 案例 02- 可重复读 RR 隔离级别下的可见性分析  

### 目标  
### 操作步骤  
### 实践过程  
Session 1: set session transaction isolation level repeatable read;  
Session 2: set session transaction isolation level repeatable read;  
Session 3: set session transaction isolation level repeatable read;  
确认 transaction isolation 为 RR  
Session 1: select @@tx_isolation;  
<img width="125" alt="image" src="https://user-images.githubusercontent.com/96624836/197538530-25edeb56-8d38-4aeb-a8fa-2d021fd40b82.png">  
Session 2: select @@tx_isolation;  
<img width="125" alt="image" src="https://user-images.githubusercontent.com/96624836/197538530-25edeb56-8d38-4aeb-a8fa-2d021fd40b82.png">  
Session 3: select @@tx_isolation;  
<img width="125" alt="image" src="https://user-images.githubusercontent.com/96624836/197538530-25edeb56-8d38-4aeb-a8fa-2d021fd40b82.png">  


还原初始数据是刘备  
Session 1: delete from tab_user where id = 1;  
Session 1: insert into tab_user values(1, '刘备', 18, '蜀国');  
Session 1: select * from tab_user;  
<img width="227" alt="image" src="https://user-images.githubusercontent.com/96624836/197548069-e2f776ef-2760-4a5b-b851-294891551659.png">  

开启事务 1, 2, 3  
Session 1: begin;  
Session 2: begin;  
Session 3: begin;

Session 1: update tab_user set name = '关羽' where id = 1;

检查事务  
<img width="681" alt="image" src="https://user-images.githubusercontent.com/96624836/197548419-9e14edd6-cee3-4201-a1d6-2f5ac55774e2.png">  

Session 1: update tab_user set name = '张飞' where id = 1;

检查事务  
<img width="685" alt="image" src="https://user-images.githubusercontent.com/96624836/197548707-df1d813f-0ee6-470e-9766-80a6834e85c7.png">  

Session 2: update tab_user set name = '赵云' where id = 1;  
检查事务  
<img width="682" alt="image" src="https://user-images.githubusercontent.com/96624836/197549188-08910b44-edaa-438d-a164-7cf0f90c16f0.png">  

Session 3: select * from tab_user;  
<img width="226" alt="image" src="https://user-images.githubusercontent.com/96624836/197552222-68cd111a-14a0-4018-ae42-2e6e88e1a253.png">  

Session 1: commit;  
检查事务  
<img width="682" alt="image" src="https://user-images.githubusercontent.com/96624836/197552411-13c6d206-727f-43cc-b1d3-2f865a13fd80.png">  

Session 3: select * from tab_user;  
<img width="228" alt="image" src="https://user-images.githubusercontent.com/96624836/197552525-324a4991-e5bf-4784-acca-64e871d3fbfd.png">

Session 2: update tab_user set name = '诸葛亮' where id = 1;  
检查事务  
<img width="683" alt="image" src="https://user-images.githubusercontent.com/96624836/197552934-7b8ad11c-e09a-4434-8fd3-8a76933d46cd.png">

Session 3: select * from tab_user;  
<img width="229" alt="image" src="https://user-images.githubusercontent.com/96624836/197553117-887ac071-90f8-40b1-b975-b15da2b2239f.png">

Session 2: commit;  
检查事务  
<img width="680" alt="image" src="https://user-images.githubusercontent.com/96624836/197553342-ca7e806b-ef11-4e53-a2cb-e3b276076f93.png">

Session 3: select * from tab_user;  
<img width="222" alt="image" src="https://user-images.githubusercontent.com/96624836/197561203-e18534eb-bd8b-4e99-8776-d39a52a34619.png">  

Session 3: commit;  
检查事务  
<img width="679" alt="image" src="https://user-images.githubusercontent.com/96624836/197553657-9ed452a4-a7d5-48e8-b9c9-31e39323f6ea.png">  

Session 3: select * from tab_user;  
<img width="235" alt="image" src="https://user-images.githubusercontent.com/96624836/197561409-576b4e7f-13cd-410e-b268-2472497898b1.png">  


### 结论
在 RR 隔离级别下，一个事务始终保持一致，可以从复被读，无论另一个事务是否提交。

### 结论分析

## 题目 02- 什么是索引？
### 要点：
索引是数据库快速获取数据的数据结构。  
### 优点是什么？
提高检索效率，降低磁盘IO，降低数据排序成本。  
### 缺点是什么？
占用磁盘空间，降低更新效率。  
### 索引分类有哪些？特点是什么？
聚簇索引，辅助索引，组合索引，唯一性索引，主索引，二级索引  
### 索引创建的原则是什么？
### 有哪些使用索引的注意事项？
### 如何知道 SQL 是否用到了索引？
在SQL语句前加上explain  
### 请你解释一下索引的原理是什么？【重点】
### 说清楚为什么要用 B+Tree

## 题目 03- 什么是 MVCC？
### 要点：
MVCC是多版本并发控制，通过不加锁而对历史数据保存快照以控制访问每个版本的可见性，以实现事务。

### Redo 日志  
记录数据库操作的日志，用以重建数据库。 

### ReadView
可见性视图，一个保存历史数据快照可见性的数据结构，以供MVCC使用。  

### 如何判断可见性
通过记录数据快照版本和隔离级别来判断。
