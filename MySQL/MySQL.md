# 																			MySQL

## #基础语法

 > ### DDL 

​	**数据定义语言 ==定义==数据库以及数据库表**

```mysql
show databases	查询所有数据库

create database	创建数据库

use database 	切换数据库

drop database	删除数据库

show tables		查询所有数据库表

create table 	创建数据库表

alter table add/change/...	修改数据库表

drop table 		删除数据库表
```

 > ### DML 

​	**数据操作语言 对表中的数据记录进行 ==增删改==**

```mysql
insert	添加数据 

update	修改数据
	
delete	删除数据
```

 > ### DQL

​	**数据查询语言 对表中的数据记录进行 ==查询==**

```mysql
select	查询数据 

group by 分组

group by having 分组加条件

order by 排序 #默认asc升序  desc降序

limit 起始索引,每页记录数量	分页查询
```
 > ### DCL

​	**数据控制语言 用来管理数据库用户，控制数据库的访问 ==权限==**

```mysql
create user '用户名@允许访问的主机名%' identified by '密码'	创建用户

alter user '用户名@允许访问的主机名%' identified with mysql_native_password by '密码'	修改用户密码

drop user '用户名@允许访问的主机名%'	删除用户

grant 权限 on 数据库.表 to '用户名@允许访问的主机名%'	授予权限

revoke 权限 on 数据库.表 from '用户名@允许访问的主机名%'	授予权限
```



## #函数

函数 是指一段可以直接被调用的程序或者代码

> ### 字符串函数

![image-20240302163905257](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302163905257.png)

-------

> ### 数值函数

![image-20240302164118982](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302164118982.png)

-------

> ### 日期函数

![image-20240302164506145](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302164506145.png)

-------

> ### 流程函数

![image-20240302165219409](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302165219409.png)



## #约束

约束是作用于表中字段的规则，用于限制存储在表中的数据 保证数据的准确性、一致性和完整性

![image-20240302170522838](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302170522838.png)

> ### 例
> ![image-20240302170552887](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302170552887.png)
> ![image-20240302170618647](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302170618647.png)

-------

> ## 外键约束
>
> 把两张表之间建立起连接，从而保证数据的一致性和完整性
>
> ![image-20240302171033465](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302171033465.png)

```mysql
constraint '外键名称' foreign key('外键字段名') reference '主表(主表列名)'	在创建表时添加外键约束

alter table '表名' add constraint '外键名称' foreign key('外键字段名') reference '主表(主表列名)'	后续添加外键约束

alter table '表名' drop foreign key '外键名称'	删除外键
```

> ### 外键的约束行为
>
> 默认为 `no action` 即 `restrict`

![image-20240302172157969](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302172157969.png)

```mysql
on update '行为' on delete '行为'	设置外键行为
```



## #多表查询

> ### 多表关系
>
> 1、 一对多  例如：部门和员工	在多的一方建立外键，指向另外一方的主键
>
> 2、 多对多  例如：学生与课程	建立中间表，通过中间表关联两方主键
>
> 3、 一对一  例如：用户基本信息与用户受教育信息	一方添加主键关联另一方主键，并设置为唯一约束

​	***进行多表查询：***

```mysql
select * from '表1','表2'		查询出笛卡尔积 表1和表2的所有组合情况
```

​	不是我们想要的 需要***消除无效的笛卡尔积：***

```mysql
select * from '表1','表2' where 表1.id = 表2.id
```

----------

> ### 内连接
>
> 也被称为自然连接，只有两个表相匹配的行才能在结果集中出现。返回的结果集选取了两个表中所有相匹配的数据，舍弃了不匹配的数据。

​	**隐式内连接：**如上

​	**显式内连接：**

```mysql
#减少字段的扫描 执行速度更快
select * from '表1' (inner) join '表2 ' on 表1.id = 表2.id
```

-------

> ### 外连接
>
> 外连接不仅包含符合连接条件的行，还包含左表(左连接时)、右表(右连接时)或两个边接表(全外连接)中的所有数据行。

​	**左外连接：**

```mysql
select * from '表1' left (outer) join '表2' on 表1.id = 表2.id
```

​	查询表1的所有数据 以及表1和表2的交集	（包含表1中外键为空的数据）

​	**右外连接：**

```mysql
select * from '表1' right (outer) join '表2' on 表1.id = 表2.id
```

​	查询表2的所有数据 以及表1和表2的交集	（包含表2中外键为空的数据）

-------

> ### 自连接
>
> 自连接通常用来查询同一个表中符合条件的数据		例如 在员工表中查询附属员工
>
> ***#必须起别名***	可以内连接也可以外连接

```mysql
select * from '表1' '别名' join '表1' '别名' on '条件'		#必须起别名

select * from '表1' a left (outer) join '表1' b on a.ident = b.id
```

-----

> ### 联合查询
>
> 将查询的结果合并 （即>>添加到末尾）	
>
> union 合并后去重		union all 直接合并
>
> ***#需要查询返回的字段相同***

```mysql
select * from emp where salary < 5000
union all
select * from emp where salary > 50
```

-----

> ### 子查询
>
> 又称为嵌套查询

​	**标量子查询：**

![image-20240302192855563](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302192855563.png)

```mysql
select * from t1 where dept = (select id from t2 where name = '销售部')	#查询销售部的数据
```

------------

​	**列子查询：**

![image-20240302192938218](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302192938218.png)

```mysql
#查询年龄为18和20的数据
select * from t1 where id in (select id from t2 where age = '18' or age = '20')
```

-----------------

**行子查询：**

![image-20240302193633544](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302193633544.png)

```mysql
select * from emp where salary = 12500 and managerid = 1;
==
select * from emp where (salary,managerid) = (12500,1);

#查询与张三的薪资及直属领导相同的员工
select * from emp where (salary,managerid) = (select salary,managerid from emp where name = '张三')
```

---------------

**表子查询：**

![image-20240302194330083](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302194330083.png)

```mysql
#查询与张三或者李四薪资和工作相同的员工 （in 包含 即多项匹配） 
select * from emp where (salary,job) in (select salary,job from emp where name = '张三' or name = '李四')
```



## #事务

> ### 事务	是一组操作的集合 集合内部的操作同成功同失败

***事务操作：***默认自动提交事务	默认@@autocommit = 1

```mysql
#···转账操作	默认@@autocommit = 1
#1.查询张三余额
select * from account where name = '张三';
#2.扣除张三余额
update account set money = money - 1000 where name = '张三';
#3.增加李四余额
update account set money = money + 1000 where name = '李四';
```
*方法一、*

```mysql
#···转账操作
#1.设置手动提交事务 @@autocommit = 0 只针对当前窗口有效
set @@autocommit = 0 ;
#2.查询张三余额
select * from account where name = '张三';
#3.扣除张三余额
update account set money = money - 1000 where name = '张三';
#4.增加李四余额
update account set money = money + 1000 where name = '李四';

#5.提交事务
commit;
#6.回滚事务
rollback;
```
*方法二、*

```mysql
#···转账操作
#1.开启事务
start transaction
#2.查询张三余额
select * from account where name = '张三';
#3.扣除张三余额
update account set money = money - 1000 where name = '张三';
#4.增加李四余额
update account set money = money + 1000 where name = '李四';

#5.提交事务
commit;
#6.回滚事务
rollback;
```

------------------

> ## 事务的四大特性 ACID

![image-20240302200409296](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302200409296.png)

-----------------------

> ## 并发事务问题

![image-20240302200906544](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302200906544.png)

> ## 事务隔离级别

![image-20240302201107477](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240302201107477.png)

- Read uncommitted 读未提交 *效率最高*

- Read committed 读已提交

- Repeatable Read 可重复读

- Serializable 串行化 进行并发操作时只允许一个事务进行操作 *最安全*

  ```mysql
  #查看事务隔离级别
  select @@transaction_isolation
  
  #设置事务隔离级别 session当前会话窗口有效 global所有会话窗口有效
  set [session/global] transaction isolation level 隔离级别
  ```




## #MySQL存储引擎

> 存储引擎	即如何存储数据、如何为存储的数据建立索引和如何更新、查询数据等技术的实现方法。
>
> 因为在关系数据库中数据的存储是以表的形式存储的，所以存储引擎也可以称为表类型（即存储和操作此表的类型）。
>
> 默认engine = InnoDB

-----------------------------------

### MySQL体系结构

> - 连接层　连接池
> - 服务层    SQL接口　解析器　查询优化器　缓存
> - 引擎层    存储引擎
> - 存储层　磁盘数据　文件和日志

-----------------------

### InnoDB存储引擎

> 兼顾高可靠性和高性能的通用存储引擎	
>
> 表空间存储文件为xxx.ibd
>
> 参数：`innodb_file_per_table = on` 默认打开 表示每张表都对应一个表空间文件

- DML增删改操作支持事务 支持ACID模型
- 行级锁，提高并发访问性能
- 支持外键约束

-----------------------

> #### InnoDB逻辑存储结构

![image-20240303160803691](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303160803691.png)

----------------------

### MyISAM、Memory存储引擎

> ### MyISAM
>
> ![image-20240303161556583](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303161556583.png)

-----------------

> ### Memory
>
> ![image-20240303161528276](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303161528276.png)

-------------------

### 存储引擎的特点及区别

![image-20240303161803222](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303161803222.png)

--------------------

### 存储引擎的选择

![image-20240303161920372](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303161920372.png)



## #索引

> ### 索引概述  
>
> *索引	是帮助MySQL高效获取数据的数据结构（有序）*

​	***优缺点：***

![image-20240303172524534](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303172524534.png)

----------------

### 索引结构

> #### MySQL的索引是在存储引擎层实现的，不同的存储引擎有不同的结构

![image-20240303172821863](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303172821863.png)

> #### 不同存储引擎对不同索引结构的支持情况

![image-20240303172933422](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303172933422.png)

---------------------

### BTree 和B+Tree索引

### Hash索引

### R-Tree空间索引

### Full-text全文索引

------------------

### 索引分类

![image-20240303185218580](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303185218580.png)

> #### 聚集索引
>
> 选取主键作为聚集索引的值	叶子节点所挂载的数据为整个 row 的值

![image-20240303185244057](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303185244057.png)

> #### 二级索引
>
> 选取该列属性作为二级索引的值 叶子节点所挂载的数据为该属性所在的主键ID
>
> ***回表查询：通过属性查找row的全部值时 先通过二级索引查找所对应的主键ID 再通过主键ID使用聚集索引查找row的全部值***

![image-20240303185429023](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303185429023.png)

---------------

### 索引语法

```mysql
#创建索引 unique唯一索引 fulltext全文索引	
#联合索引：字段名可以指定多个 同时为多个字段设置同一个索引
#最左前缀法则：联合索引创建之后查询时需要结合索引中字段名的顺序查询数据  
			若跳过某个字段则之后的索引失效
			若使用 > < 范围查询 则范围查询后的索引失效
create [unique/fulltext] index '索引名' on '表名(字段名)';

#查询指定表的索引
show index from '表名';

#删除索引
drop index '索引名' on '表名';
```

-----------------

### SQL性能分析

> #### SQL优化

```mysql
#查询SQL的执行频率进行优化
show [session/global] status like 'Com_______';
```

- SQL优化主要是优化**频率最高**的**查询语句**　即**索引优化**
- **慢查询日志** slow_query_log    记录所有执行时间超过指定时间的SQL语句的日志
  - 默认没有开启	要去MySQL的配置文件中修改

- **`profile`** 操作 添加对执行SQL语句的监控

------------

> #### explain执行计划
>
> 在 **`selsect`** 语句前加 **`explain`** 查看执行计划


----------

### SQL提示

> 若同时符合多个索引条件 比如同时符合单列索引和联合索引(默认使用联合索引) 则可以用SQL提示指定索引
>
> **`use`** 和 **`ignore`** 仅仅是建议 是否采取需要看MySQL优化器
>
> **`force`** 是强制是采取索引

![image-20240303203228607](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303203228607.png)

--------------

### 覆盖索引

> #### 尽量采用覆盖索引 (查询使用了索引并需要返回的列，在该索引中已经可以全部找到)，减少 select*
>
> 即 ***不需要回表查询数据	所有列都在索引中***

----------------

### 前缀索引

> 将很长的字符串字段中的前缀设为索引，减小索引体积

```mysql
create index '索引名' on '表名(字段名(n))'	# n为前缀长度
```

> 前缀索引长度
>
> ![image-20240303205217296](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240303205217296.png)
-----------------

### 索引使用规则

> #### 索引失效

1. **最左前缀法则**：

       *联合索引创建之后 查询时需要结合索引中**字段名的顺序**查询数据*  
            			*若跳过某个字段则之后的索引失效*
            			*若使用 > < 范围查询 则范围查询**后**的索引失效*

2. **索引列使用运算操作，索引列将会失效**      包括普通运算 函数运算等

3. **字符串类型字段使用时，不加单引号' '索引将会失效**     存在隐式类型转换

4. 如果仅仅尾部使用模糊匹配，索引不会失效   **但如果头部使用模糊匹配，索引将会失效**

5. **用or连接时 如果or一侧有索引 or另一侧没有索引 那么所涉及的索引都会失效**

6. **如果MySQL优化器评估使用索引比全表扫描慢，则不使用索引**

--------------------

> #### 索引设计原则

![image-20240304144817245](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304144817245.png)



## ＃SQL优化

> #### 其他SQL语句

### insert优化

> 1. **批量插入**	减少SQL语句执行次数
>
> 2. **手动提交事务**　减少SQL语句提交次数
>
> 3. **主键顺序插入**　主键自动创建主键索引，B＋树顺序插入的效率更高
>
> 4. **大批量插入**　如果需要一次性插入大量数据，使用`insert`语句插入性能较低，此时可以使用MySQL提供的`load`指令插入
>
>    ```mysql
>    #设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
>    set global local_infile=1;
>                                                       
>    #执行load指令将准备好的.sql数据文件加载到表结构中
>    load data local infile '/root/sql1.sql' into table 'tb_user' fields terminated by ',' lines terminated by '/n';
>    ```

-----------

### 主键优化

> 1. **满足业务需求的情况下，尽量降低主键长度**
> 2. **插入数据时，尽量选择顺序插入，选择自增主键**
> 3. **尽量不要使用UUID或者其他自然主键**，如身份证号     乱序插入and长度较长
> 4. **避免对主键进行修改**

---------------

### order by优化

> 1. `order by`排序优化 参考覆盖索引
>
> > 但是`where`中最左前缀法则是无序的 
> >
> > 而`order by`中最左前缀法则是有序的 必须按照顺序查找 因为排序时先按照第一个排序 若相同则按照第二个排序
>
> 2. 索引默认按照升序`asc`排列，若`order by`用到倒叙`desc`排列则性能降低 容易出现 `filesort` 即排序缓冲区不足
>
> 可以手动设置索引的B+树排列顺序	
>
> ``` mysql
> #先按照age升序排列 若age相同 再按照phone降序排列
> create index idx_age_phone on tb_user(age asc, phone desc);
> ```
>
> 3. 如果不可避免出现 `filesort`，大量数据排序时可以增加排序缓冲区大小 `sort_buffer_size` 默认256K

------------

### group by优化

> `group by` 分组优化 参考覆盖索引
>
> **若不符合最左前缀法则 则会使用临时表 效率低**
>
> 若有a,b索引
>
> `where a = '1' group by b ;` 同样符合最左前缀法则

------------

### limit优化

> `limit` 分页优化 参考覆盖索引和子查询

----------

### count优化

> `count` 计数优化 目前没有较好的优化策略，可以在插入时使用key+1自行计数
>
> **效率：**
>
> `count *` = `count(1)` > `count(主键)`  > `count(字段)`

---------------------------

###  update优化

> `update` 更新优化
>
> `where` 根据有索引的字段添加`update` 事务时会添加**行级锁**	锁住该行
>
> `where` 根据无索引的字段添加`update` 事务时会添加**表级锁**	锁住整个表 降低并发性能
>
> ==InnoDB引擎中行锁是针对索引加的锁，不是针对记录加的锁，并且索引不能失效，否则会升级为表锁==



## #MySQL存储对象

###  视图

> 一种虚拟存在的表，类似一个观察db数据的窗口
>
> ![image-20240304183535622](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304183535622.png)

```mysql
#创建视图 创建时or replace可以不加
create [or replace] view '视图名称' as select语句

#查询视图
#查询创建视图的语句
show create view '视图名称'
#查询视图中的数据
select '字段' from '视图名称'

#修改视图
#方法一  or replace必须加
create or replace view '视图名称' as select语句
#方法二
alter view '视图名称' as select语句

#删除视图
drop view '视图名称'
```

> #### 对视图数据进行DML操作   会直接对基表进行DML操作
> #### 对视图数据进行DQL操作   会根据视图进行DQL操作
>
> `with check option` 在创建视图时添加检查选项 若DML不符合视图条件的数据会报错
>
> 默认为 `cascaded` 可以修改为 `local`
>
> `with check option` 同时检查视图所依赖的视图的条件 **不管上级视图有没有定义** 相当于给涉及到的所有视图添加检查选项
>
> `with local check option` 同样同时检查视图所依赖的视图的条件 **但是若上级视图没有定义则不检查**

--------------------

![image-20240304190557817](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304190557817.png)

---------------------

![image-20240304191002057](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304191002057.png)

---------------------

###  存储过程

> SQL语句的集合，对SQL语句进行封装并重用	*SQLserver函数*

> #### 特点
>
> - 封装，复用
> - 可以接收参数，也可以返回数据
> - 减少网络交互，提高效率

```mysql
#创建存储过程
create procedure 存储过程名称(参数列表)
begin
	------SQL语句
end;

#调用存储过程
call 存储过程名称(参数列表);

#查看存储过程语句
show create procedure 存储过程名称;

#删除存储过程
drop procedure 存储过程名称;
```

> #### 变量
>
> ***@@是系统变量	@是用户定义变量***
>
> >  ##### 系统变量
> >
> >  默认是 `session`
> >
> >  ![image-20240304192206168](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304192206168.png)
>
> > ##### 用户定义变量
> >
> > 只有 `session`	会话有效
> >
> > ![image-20240304192400871](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304192400871.png)
>
> > ##### 局部变量
> >
> > 只在该存储过程中生效	声明范围是 begin ... end 块
> >
> > ![image-20240304192710055](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304192710055.png)

--------------

>#### if语法
>
>![image-20240304192825227](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304192825227.png)

-------------------

>#### 参数
>
>类似于方法的形参
>
>![image-20240304193100804](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304193100804.png)

--------------

>#### case语法
>
>![image-20240304193152168](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304193152168.png)

--------------

>#### while语法 满足条件则循环
>
>![image-20240304193304703](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304193304703.png)

--------------

>#### repeat语法 满足条件则退出循环
>
>![image-20240304193329176](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304193329176.png)

--------------

>#### loop语法
>
>`leave` 即 `break`  	`iterate` 即 `countinue`
>
>==**需要给循环起名字 label_name : loop**==
>
>![image-20240304193815941](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304193815941.png)

--------------

>#### 游标语法
>
>![image-20240304193958287](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304193958287.png)
>
>

--------------

>#### 条件处理程序 (try_catch)	设置当满足（）时触发
>
>![image-20240304194450182](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304194450182.png)

--------------

### 存储函数

>![image-20240304194401884](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304194401884.png)

-------------------------------

###  触发器

> **触发器是与表有关的数据库对象，指在DML操作之前或之后，触发并执行触发器中定义的SQL语句集合**
>
> 可以协助应用在数据库端确保数据的完整性，日志记录，数据校验等操作
>
> 触发器**只支持行级触发器** 不支持语句级触发器 即操作一行便会触发一次

---------------------

> #### 别名

![image-20240304195126758](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304195126758.png)

---------------------

>#### 语法

```mysql
#创建触发器
create trigger '触发器名'
before/after insert/update/delete	#设置触发条件 (语句前后触发 什么类型语句触发)
on '表名' for each row	#只支持行级触发器
begin
	
end;

#查看触发器
show triggers;

#删除触发器
drop trigger ['数据库名' ] '触发器名';	#若未指定数据库名字 默认当前数据库
```



## #锁

> #### 概述
>
> 锁是计算机协调多个进程或线程访问某一资源的机制，用于解决数据并发访问的一致性和有效性。

------------------

### 全局锁

> #### 锁定数据库的所有表	加锁后数据库处于只读状态 后续修改操作都会处于阻塞状态
>
> 一般用于全库的数据备份过程 保证数据的一致性

```mysql
#加锁
flush tables with read lock;

#解锁
unlock tables;
```

> #### 特点
>
> ![image-20240304201145077](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304201145077.png)

**也可以通过加参数 `--single-transaction` 来完成不加锁备份保证数据的一致性**

```mysql
#全库的数据备份
mysqldump --single-transaction -u root -p 123123 > itcast.sql
```

-------------------

### 表级锁

> #### 锁住整张表	锁定粒度大，发生锁冲突的概率最高，并发率最低
>
> -------------------

> #### 表锁
>
> 1. **表共享读锁（共享锁）（read lock）**顾名思义 加锁之后所有客户端只能读（对自己同样有效）
>
> 1. **表独占写锁（排他锁）（write lock）**顾名思义 加锁之后只有该客户端可以进行读写操作，其他客户端不能进行读写操作
>    
>    ```mysql
>    #加锁
>    lock tables '表名' read/write
>       
>    #释放锁
>    unlock tables
>    ```
>
> ---------------------

> #### 元数据锁（MDL）
>
> 加锁过程自动控制，**元数据即表结构（字段）** 用于维护表结构
>
> **避免DML语句与DDL语句冲突，保证数据的正确性**
>
> ==***即当一张表有待提交事务时不能修改表结构***==
>
> **==当对表进行增删改查的时候，加MDL读锁（共享）语句执行结束或事务提交后解锁==**
>
> **==当对表结构进行变更的时候，加MDL写锁（排他）语句执行结束或事务提交后解锁==**
>
> ***共享锁和共享锁是兼容的***
>
> ***排他锁和共享锁、排他锁都是互斥的***
>
> ![image-20240304202341991](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304202341991.png)
>
> ------------------------------

> #### 意向锁
>
> 避免DML在执行过程中，**加的行锁与表锁的冲突**，添加了**意向锁**
>
> 使得表锁在检查时不用检查每一行数据是否加锁，减少了表锁的检查
>
> 1. **意向共享锁（IS）**由语句 `select ... lock in share mode` 添加
>
>    与表锁中的共享锁兼容，与排他锁互斥
>
> 2. **意向排他锁（IX）**由语句 `insert`、`update`、`delete`、`select ... for update` 添加
>
>    与表锁中的共享锁 排他锁都互斥
>
>    意向锁之间不会互斥
>
> ---------------------

### 行级锁

> #### 锁住对应的行数据	锁定粒度最小，发生锁冲突的概率最低，并发度最高 应用在InnoDB中
>
> 在InnoDB引擎中，数据是基于索引组织的，**行锁是根据索引项来实现的**，而不是对记录加锁
>
> --------------------------

>#### 行锁（Record Lock）
>
>锁定单行记录的锁，防止其他事务对此行操作，在RC、RR隔离级别下支持
>
>![image-20240304204104935](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304204104935.png)
>
>1. **共享锁（S）**允许一个事务读取一行，与其他事务相同数据集的排他锁互斥
>2. **排他锁（X）**允许获取排他锁的事务更新数据，与其他事务相同数据集的共享锁和排他锁互斥
>
>------------------

>#### 间隙锁（Gap Lock）
>
>锁定索引记录的间隙，确保索引记录的间隙不变，防止其他事务对其insert，产生幻读，在RR隔离级别下支持
>
>![image-20240304204124764](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304204124764.png)
>
>------------------

>#### 临键锁（Next-Key Lock）
>
>行锁和间隙锁的组合，同时锁住数据和数据前面的间隙，在RR隔离级别下支持
>
>![image-20240304204338045](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304204338045.png)
>
>------------------

### ***锁的优化和退化***

![image-20240305162032177](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240305162032177.png)

> **上文中自动加锁指的是加临键锁（MySQL默认的锁）**
>
> **select手动加锁加的是行锁**
>
>  **加锁之后MySQL会根据实际情况进行退化或者优化 这时才真正加锁完成**



> ### 优化及退化规则

> *注意是否有索引以及索引是否唯一*

![image-20240305163243058](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240305163243058.png)



![image-20240304204900132](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304204900132.png)

![image-20240304204918901](https://github.com/ZMZER/Notes/blob/5a489de72b91be3a31427bbbc0a89abd03fd94ea/MySQL/images/image-20240304204918901.png)
