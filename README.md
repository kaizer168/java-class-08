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
<img width="135" alt="image" src="https://user-images.githubusercontent.com/96624836/197510717-3eeb5490-4437-49b4-868a-d02d59aca70a.png">

<img width="116" alt="image" src="https://user-images.githubusercontent.com/96624836/197511134-5b78a182-e39c-4ae0-a75d-92a19b87aff4.png">

insert 初始数据是刘备  
<img width="291" alt="image" src="https://user-images.githubusercontent.com/96624836/197511661-65b8323e-9bdb-414b-8a99-cd5bbd879250.png">  
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


### 结论

2. 案例 02- 可重复读 RR 隔离级别下的可见性分析

目标
操作步骤
实践过程  
Session 1: set session transaction isolation level repeatable read;
Session 2: set session transaction isolation level repeatable read;
Session 3: set session transaction isolation level repeatable read;
确认 transaction isolation 为 RR  

Session 1: set session transaction isolation level read committed;
Session 2: set session transaction isolation level read committed;
Session 3: set session transaction isolation level read committed;
确认 transaction isolation 为 RC  

结论
结论分析

## 题目 02- 什么是索引？
要点：

优点是什么？
缺点是什么？
索引分类有哪些？特点是什么？
索引创建的原则是什么？
有哪些使用索引的注意事项？
如何知道 SQL 是否用到了索引？
请你解释一下索引的原理是什么？【重点】
- 说清楚为什么要用 B+Tree

## 题目 03- 什么是 MVCC？
要点：

Redo 日志
ReadView
如何判断可见性
