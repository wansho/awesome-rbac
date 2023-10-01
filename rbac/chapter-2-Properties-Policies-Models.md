
```toc
```

## Access Control Matrix

[[access-control-matrix.excalidraw]]

![access control matrix](../drawing/access-control-matrix.excalidraw)


Example Access Control Matrix

| Subjects/Objects | File_1     | File_2  | File_3 | File_4  |
| ---------------- | ---------- | ------- | ------ | ------- |
| Chris            | Read,Write |         | Write  |         |
| Janet            |            | Execute |        | Suspend |
| Barbara          |            | Read    | Read   |         |
| Frank            | Read       |         |        |         |


For a system with a large number of users and objects, the matrix will become very large and will be sparsely populated. As such, an access control system is rarely implemented as a matrix but is almost implemented as a representation of the matrix. There are two primary representations of the access control matrix as implemented in computer systems today: Capability List and ACLs.

Matrix 按照 Subject 拆解：Capability List
Matrix 按照 Objects 拆解：ACLs

## Capability List

Capability List 

| Subject |                                     |
| ------- | ----------------------------------- |
| Chris   | File_1: Read, Write  File_3: Write  |
| Janet   | File_2: Execute  Process_1: Suspend |
| Barbara | File_2: Read  File_3: Read          |
| Frank   | File_1: Read                                    |


## ACLs

Access Control Lists


| Object |                                     |
| ------- | ----------------------------------- |
| File_1   | Chris: Read, Write  Frank: Read  |
| File_2   | Janet: Execute  Barbara: Read |
| File_3 | Chris: Write  Barbara: Read          |
| Process_1   | Janet: Suspend                                   |




## DAC

DAC：每个人对自己的文件，都拥有处理权。（有点分布式区块链的味道）

但是，DAC 的缺点也很明显，DAC 赋权具有传递性，容易导致特洛伊木马攻击（获取了当前用户的权限，然后以当前用户的权限干坏事）。

Chris 赋予 Frank 读的权限，Frank 则可能对其进行拷贝，使其成为自己的文件，然后公开，相当于公开了 Chris 的文件。

Frank 在拿到了 Chris 的授权（可能是通过特洛伊的方式拿到的）后，对 Chris 的文件可读可写。 

DAC 模型不能保证

## Bell-LaPadula model

贝尔-拉普杜拉模型

Simple Security Property(no read up)

(Star) Security Property(no write down) 

1. The Simple Security Property states that a subject at a given security level may not read an object at a higher security level.
2. The * (Star) Security Property states that a subject at a given security level may not write to any object at a lower security level.

no write down 的意思是，高级别的不能往低级别的写入，否则容易导致机密泄露。这样就保证了，Frank 只能读取 Chris（在 Chris 的授权下，或者通过）的机密文件，但是不能 write 到低级别的地方。

但是该模型还是有问题，Frank 虽然不能将机密文件拷贝到低级别的地方，但是 Frank 在拿到 Chris 的授权后，还是在同级别篡改甚至删除 Chris 的文件的。

该模型保证了 Confidentiality，但是不能保证 Integrity。

## Clark-Wilson Model

两个贡献：
1. 发现在商业环境中，完整性 Integrity 比机密性 Confidentiality 更加重要
2. SoD Separation of Duty，分权，例如管理员、审计员、操作员三权分立，能够天然地防止信息被篡改

完整性 Integrity 要求对象只能被授权过的用户修改。