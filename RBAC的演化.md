```toc
```
## 概要

权限系统的演化。

权限系统，要解决的问题，是一个用户，在登录后，拥有那些权限。

这涉及到如下几个小问题

1. 如何定义权限
2. 如何授权
3. 如何鉴权

## 如何定义权限

如何定义权限，就是对权限进行建模。

总的来说，系统内的权限，可以分为菜单权限和数据权限。


## UNIX安全模型-ACLs

Subjects (Who?) Users, processes 
Objects (What?) 
- Files, directories 
- Files: sockets, pipes, hardware devices, kernel objects, process data Access Operations 
(How?) - Read, Write, Execute

Access Control List. The most common method of implementing access control in a computer system is through access control lists.All system resources, such as files, printers, and terminals, have a list of authorized users attached.

```
-rw-r--r--@    1 wanshuo  staff        3515  7 30 14:29 jbr_err_pid94423.log
drwxr-xr-x     5 wanshuo  staff         160  4 18  2022 logs/
-rw-r-----     1 wanshuo  staff     8053492  8 15 16:32 mmi.tar.gz
drwxr-xr-x     5 wanshuo  staff         160  4 18 14:41 nacos/
drwxr-xr-x     5 wanshuo  staff         160  4 13 09:30 nltk_data/
drwxr-xr-x    13 wanshuo  staff         416  4  3 15:32 pic/
drwxr-xr-x     6 wanshuo  staff         192  1 30  2023 projects/
drwxr-xr-x     3 wanshuo  staff          96  3 22  2022 reading/
```

![](../drawing/acls.excalidraw)

给用户授权的整体复杂度：$|N|$
增加一个权限(文件)的复杂度：$|U|$
查找一个用户有哪些权限： $|N * U|$
查找一个权限(文件)有哪些用户拥有：$|U|$
删除一个权限(文件)的复杂度：$1$
删除一个用户的复杂度：$|N*U|$

理想中的 ACLs 受制于 Object 的数量，当系统中存在大量的文件时，查找用户拥有的权限，是极其复杂的。它是面向 Object 的数据模型。

改良后的 ACLs （list 中存放的是用户组）

![](../drawing/acls_group.excalidraw)

给用户授权的整体复杂度：$|U\_Group| * |N|$
增加一个权限(文件)的复杂度：$|U\_Group|$
查找一个用户有哪些权限： $|N| * |U\_Group|$
查找一个权限(文件)有哪些用户拥有：$|U\_Group| * |U|$
删除一个权限(文件)的复杂度：$1$
删除一个用户的复杂度：$|U\_Group|$

总结，ACLs 是面向 objects 的权限模型，其复杂度上限受制于 objects 的数量。（RBAC 是面向 Subject 用户的权限模型）

## 笛卡尔积和RBAC

用户如何与权限绑定，最简单的方法，就是做笛卡尔积。

![](../drawing/authorize.excalidraw)

每一个用户，都绑定指定的权限。复杂度是 $|U|*|P|$，每增加一个用户，就要针对这个用户进行授权。$U$ 和 $P$ 是多对多的关系。

给用户授权的整体复杂度：$|U|*|P|$
增加一个权限的复杂度：$|U|$
查找一个用户有哪些权限： $|P|$
查找一个权限有哪些用户拥有：$|U|*|P|$
删除一个权限的复杂度：$|U|$
删除一个用户的复杂度：$|P|$

----

笛卡尔积的复杂度明显不适合我们的实际应用场景。在企业中，我们通常会把一个公司划分成多个部门，各司其职。同样的，这些部门内部的权限，都可以归成一类。权限归类的好处显而易见，当有新员工加入该部门后，只需要将新员工与归类好的权限组绑定一次即可。$U$ 和 $P\_Group$ 是多对多的关系，$P\_Group$ 和 $P$ 也是多对多的关系，多个部门可以拥有一样的权限，一个部门可以拥有多个权限。

![](../drawing/authorize_group.excalidraw)

对权限进行分组后，我们看一下时间复杂度：

给用户授权的整体复杂度：$|U|*|P\_Group|+  |P\_Group| * |P| = |P\_Group| * |U + P|$
增加一个权限的复杂度：$|P\_Group|$
查找一个用户有哪些权限：$|P\_Group| * |P|$
查找一个权限有哪些用户拥有：$|P\_Group| * |U|$
删除一个权限的复杂度：$|P\_Group|$
删除一个用户的复杂度：$|P\_Group|$

复杂度的变化显而易见，由于权限被分组，时间复杂度整体上被降低了。

给用户授权的整体复杂度：$|U|*|P|$ —> $|P\_Group| * |U + P|$
增加一个权限的复杂度：$|U|$ —> $|P\_Group|$
查找一个用户有哪些权限： $|P|$ —> $|P\_Group| * |P|$
查找一个权限有哪些用户拥有：$|U|*|P|$ —> $|P\_Group| * |U|$
删除一个权限的复杂度：$|U|$ —> $|P\_Group|$
删除一个用户的复杂度：$|P|$ —> $|P\_Group|$


## 总结

![](./drawing/latex_block.pdf)

1. 不管是面向 Object 的 ACLs 还是面向 Subject 的 RBAC，分组后，都能够显著改善复杂度
2. Any problem in computer science can be solved with another layer of indirection. -- David Wheeler

