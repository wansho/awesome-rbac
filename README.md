# rbac-learning
resources to learn rbac



## 学习资源

 - [《RBAC权限系统分析、设计与实现》](https://shuwoom.com/?p=3041)
* [尚硅谷RBAC权限实战教程(rbac项目框架实战)](https://www.bilibili.com/video/BV1pp411o7UX)
* 可能是史上最全的权限系统设计 - 老刘的文章 - 知乎 https://zhuanlan.zhihu.com/p/73414693
* https://auth0.com/docs/manage-users/access-control/rbac
* 书：[Role-based Access Control](https://book.douban.com/subject/2586338/)



## 专有名词

CIA：
Confidentiality：机密性
Integrity：完整性
Availability：可用性

session：An instance of a user’s dialog with a system is called a session.

DoD：Department of Defense 国防部

DAC：discretionary access control 自由访问控制
MAC：mandatory access control 强制访问控制

TCSEC：Trusted Computer System Evaluation Criteria 美国国防部发布的基于 DAC 和 MAC 的Access Control 标准。

SoD：separation of duty



## Least Privilege

Security Subjects
Least privilege and privilege separation apply to more than just users!
- UNIX: A User should only be able to read their own files
- UNIX: A Process should not be able to read another process’s memory
- Mobile: An App should not able to edit another app’s data
- Web: A Domain should only be able to read its own cookies
- Networking: Only trusted a Host should be able to access file server

Least Privilege:  Subjects should only have access to access the data and resources needed to perform routine, authorized tasks



## ACLs


ACLs：Access Control List. The most common method of implementing access control in a computer system is through access control lists.All system resources, such as files, printers, and terminals, have a list of authorized users attached.

ACLs，每一个实体，都维护了一个 list，这个 list 中装的是对其有权限的用户。

Unix 的权限模型，采用的就是 ACLs，也就是说，每一个文件，都维护了一个 list，记录了对其有权限的用户。

ACLs 模型可以很方便地知道，对于一个文件来说，有哪些用户可以访问这个文件。但是如果想知道一个指定的用户能访问哪些文件的话，就灾难了，就得遍历所有的文件，遍历这些文件的 ACLs。

UNIX’s permission model is a simple implementation of a generic access control strategy known as Access Control Lists (ACLs).Every object has an ACL that identifies what operations subjects can perform. Each access to an object is checked against the object’s ACL.



ACLs 的 list 中装的也可以是用户组。（UNIX 用户组的概念）



## RBAC0定义

U(User)：用户集合

P(Persmission)：权限集合

R(Role)：角色集合

S(Session)：会话集合

PA(Permission Assignment)：角色和权限的关联关系集合

UA(User Assiginment)：用户和角色的关联关系集合



一次会话 $s_i$ 具有的权限集合为:
$$
P_{s_i} = \bigcup_{r \in roles(s_i)}\{p|(p,r)∈PA\}
$$




## UNIX Security Model

subjects: users, processes,

objects: file, data

operation: read, write, execute



