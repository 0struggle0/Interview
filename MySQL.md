#### mysql 优化的一般步骤都有哪些？

1. SQL 优化
开启慢查询日志 ，explain分析 SQL 语句,  分析是否需要加上索引，分析索引是否失效
  
2. 数据库设计优化

   字段类型，范式与逆范式，存储引擎

3. 架构优化

   主从复制，读写分离，分库分表，增加缓存

4. 服务器硬件优化



#### 对 mysql 事务的理解？

是多个步骤为一个过程的事务（整体）
1. 事务使用 INNODB 数据库引擎  
如果你不是INNODB ,开启事务,删除那就真的删除了.
2. 要么成批的sql全部执行,要么不执行
3. 事务用来管理   insert  update  delete 的语句

事务条件：

​	原子性   一组事务,要么成功,要么撤回.
​	稳定性    有非法数据(外键约束),事务撤回
​	隔离性  事务独立运行. 一个事务,处理后的结果影响到了其他事务,则事务撤回!
​	可靠性  软件或者硬件崩溃,Innodb 表驱动,会利用日志文件,重构修改.  可靠性  高速度  不可兼得
关键字:
Commit 提交 当一个事务完成后,发出commit 命令使所有的参与表 完成更改.
Rollback 回滚 如果发送故障,发出rollback命令 使事务返回到 所有表以前的状态.



#### mysql 触发器是什么？

 监视某种事件，并触发某种操作 (商品的添加，订单的删除 等等 连贯操作时候使用)

```sql
###触发四要素
    1. 监视地点 table
    2. 触发时间 (after/ before)
    3. 监视事件 (insert/update/delete)
    4. 触发事件 (insert/update/delete)

1、创建一个名为tg1的触发器，当向t1表中插入数据前，就向a表中插入一条数据
delimiter //     mysql中可以转换结束符
mysql>create trigger tg1 before insert on t1 for each row    #固定写法
->begin
-> insert into a values (4);
->end
```



#### 什么是组合索引，及使用情况？

由多个字段组成的索引叫组合索引

1、需要加索引的字段，要在where条件中

2、数据量少的字段不需要加索引

3、如果where条件中是 OR 关系，加索引不起作用

4、符合最左原则

Mysql从左到右的使用索引中的字段，一个查询可以只使用索引中的一部份，但只能是最左侧部分。

例子：当前组合索引是这样一个顺序 ind_status_email(status,email)
单独查询status时，可以用到这个索引，单独查询email时，却用不到
再问:  id name  password  建立组合索引 怎么建立?为什么?
name,password
因为先到先得,  name查询是多!!!  很少会通过password来查



请写出数据类型(int char varchar datetime text)的意思；请问 varchar 和 char有什么区别？

Int 整数 char 定长字符 Varchar 变长字符 Datetime 日期时间型 Text 文本型 Varchar 与char的区别 char是固定长度的字符类型，分配多少空间，就占用多长空间。 Varchar是可变长度的字符类型，内容有多大就占用多大的空间，能有效节省空间。 由于varchar类型是可变的，所以在数据长度改变的时，服务器要进行额外的操作，所以效率比char类型低。



MyISAM 和 InnoDB 的基本区别？索引结构如何实现？

MyISAM类型不支持事务，表锁，易产生碎片，要经常优化，读写速度较快，而InnoDB类型支持事务，行锁，有崩溃恢复能力。读写速度比MyISAM慢。

创建索引：alert table tablename add index (`字段名`)



增加一个字段性别sex，写出修改语句

Alert table user add sex enum(’0′,’1′);



查询出年龄介于20岁到30岁之间的用户

可对where后面的字段 age 建立索引，也可对语句建立存储过程。



数据库索引有几类，分别是什么？什么时候该用索引？

普通索引、主键索引、唯一索引

并非所有的数据库都以相同的方式使用索引，作为通用规则，只有当经常查询列中的数据时才需要在表上创建索引。



对关系型数据库而言，索引是相当重要的概念，请回答有关索引几个问题:

a) 索引的目的是什么?

1、快速访问数据表中的特定信息，提高检索速度

2、创建唯一性索引，保证数据库表中每一行数据的唯一性

3、加速表和表之间的连接

4、使用分组和排序子句进行数据检索时，可以显著减少查询中分组和排序的时间

b) 索引对数据库系统的负面影响是什么?

负面影响：创建索引和维护索引需要耗费时间，这个时间随着数据量的增加而增加；索引需要占用物理空间，不光是表需要占用数据空间，每个索引也需要占用物理空间；当对表进行增、删、改的时候索引也要动态维护，这样就降低了数据的维护速度。

c) 为数据表建立索引的原则有哪些?

1、在最频繁使用的、用以缩小查询范围的字段上建立索引

2、在平频繁使用的、需要排序的字段上建立索引

d) 什么情况下不宜建立索引?

1、对于查询中很少涉及的列或者重复值比较多的列，不宜建立索引

2、对于一些特殊的数据类型，不宜建立索引，比如文本字段(text)，值范围较少的知道等。

7.   指出以下代码片段中的SQL注入漏洞以及解决方法(magic_quotes_gpc = off)

   ```php
   mysql_query(“select id,title from content where catid=’{_GET[catid]}’ and title like ’%_GET[keywords]%’”, $link);
   ```

   注入漏洞主要存在用户提交的数据上，这里的注入漏洞主要是$_GET[catid]和$_GET[keyword]

   解决注入漏洞：

```php
$_GET[catid]=intval($_GET[catid]);
$sql=”select id,title from content where catid=’{$_GET[catid]}’ and title like ’%$_GET[keywords]%”;
$sql=addslashes($sql);
Mysql_query($sql);
```



SQL注入漏洞产生的原因 ? 如何防止?

SQL注入产生的原因：程序开发过程中不注意规范书写sql语句和对特殊字符进行过滤，导致客户端可以通过全局变量POST和GET提交一些sql语句正常执行。

防止SQL注入：

1、开启配置文件中的magic_quotes_gpc和magic_quotes_runtime设置

2、执行sql语句时使用addslashes进行sql语句转换

3、Sql语句书写尽量不要省略小引号和单引号

4、过滤掉sql语句中的一些关键字：update、insert、delete、select、*

5、提高数据库表和字段的命名技巧，对一些重要的字段根据程序的特点命名，取不易被猜到的。

6、Php配置文件中设置register_globals为off，关闭全局变量注册

7、控制错误信息，不要再浏览器上输出错误信息，将错误信息写到日志文件中

 