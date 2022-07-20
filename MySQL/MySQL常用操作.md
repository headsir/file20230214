

## 18、创建临时表

#创建临时表tmp_table1
CREATE TEMPORARY TABLE tmp_table1 (
	id INT UNSIGNED AUTO_INCREMENT,
type VARCHAR ( 255 ),
type2 VARCHAR ( 255 )) ENGINE = INNODB DEFAULT CHARSET = utf8mb4;

# 			 [     MySQL数据库（主键、索引、外键、触发器...）         ](https://www.cnblogs.com/DannyShi/p/4617469.html) 		

**主键：**

　　能够唯一标识表中某一行的属性或属性组。一个表只能有一个主键，但可以有多个候选索引。主键常常与外键构成参照完整性约束，防止出现数据不一致。主键可以保证记录的唯一和主键域非空,数据库管理系统对于主键自动生成唯一索引，所以主键也是一个特殊的索引。

**索引：**

　　是用来快速地寻找那些具有特定值的记录。主要是为了检索的方便，是为了加快访问速度， 按一定的规则创建的，一般起到排序作用。

　　唯一性索引：这种索引和前面的“普通索引”基本相同，但有一个区别：索引列的所有值都只能出现一次，即必须唯一。

　　注:当你的应用程序进行SQL查询速度很慢时，应该想想是否可以建索引。

 

　　在数据库表中，对字段建立索引可以大大提高查询速度。假如我们创建了一个 mytable表：

　　CREATE TABLE mytable(  ID INT NOT NULL,  username VARCHAR(16) NOT NULL );  我们随机向里面插入了10000条记录，其中有一条：5555, admin。

　　在查找username="admin"的记录 SELECT * FROM mytable WHERE  username='admin';时，如果在username上已经建立了索引，MySQL无须任何扫描，即准确可找到该记录。相反，MySQL会扫描所有记录，即要查询10000条记录。

　　索引分单列索引和组合索引。单列索引，即一个索引只包含单个列，一个表可以有多个单列索引，但这不是组合索引。组合索引，即一个索包含多个列。

- 普通索引：这是最基本的索引，它没有任何限制
- 唯一索引：它与前面的普通索引类似，不同的就是：索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。
- 主键索引：它是一种特殊的唯一索引，不允许有空值。一般是在建表的时候同时创建主键索引。
- 组合索引：CREATE TABLE mytable(  ID INT NOT NULL,  username  VARCHAR(16) NOT NULL,  city VARCHAR(50) NOT NULL,  age INT NOT NULL  ); 为了进一步榨取MySQL的效率，就要考虑建立组合索引。就是将 name, city, age建到一个索引里。

选择和合适的数据类型：

- 通常情况下，越小的数据类型通常更好，越小的数据类型通常在磁盘、内存和CPU缓存中都需要更少的空间，处理起来更快。
- 简单的数据类型更好，整型数据比起字符，处理开销更小，因为字符串的比较更复杂。在MySQL中，应该用内置的日期和时间数据类型，而不是用字符串来存储时间；以及用整型数据类型存储IP地址。
- 尽量避免NULL：应该指定列为NOT NULL，除非你想存储NULL。在MySQL中，含有空值的列很难进行查询优化，因为它们使得索引、索引的统计信息以及比较运算更加复杂。你应该用0、一个特殊的值或者一个空串代替空值。

建立索引的时机：

　　一般来说，在WHERE和JOIN中出现的列需要建立索引，但也不完全如此，因为MySQL只对<，<=，=，>，>=，BETWEEN，IN，以及某些时候的LIKE才会使用索引。

**外键：**

　　是用于建立和加强两个表数据之间的链接的一列或多列。外键约束主要用来维护两个表之间数据的一致性。简言之，表的外键就是另一表的主键，外键将两表联系起来。一般情况下，要删除一张表中的主键必须首先要确保其它表中的没有相同外键（即该表中的主键没有一个外键和它相关联）。

MySQL中创建外键：

删除时、更新时：

- RESTRICT：限制，指的是如果字表引用父表的某个字段的值，那么不允许直接删除父表的该值。
- NO ACTION：无参照完整性关系，有了也不生效。
- CASCADE：级联，删除父表的某条记录，字表中引用该值的记录会被删除。
- SET NULL：在父表上update/delete记录时，将子表上匹配记录的列设为null 。

**触发器：**

　　触发器是一种与表操作有关的数据库对象，当触发器所在表上出现指定事件时，将调用该对象，即表的操作事件触发表上的触发器的执行。

以Navicat for MySQL为例说明触发器的使用：

![img]()

　　![img](https://images0.cnblogs.com/blog2015/236253/201507/031029541613315.png)

- 名：用户自定义一个触发器的名字
- 触发：这里有两个选项（Before&After）说明是在执行语句之前还是在之后操作
- 插入、更新、删除

可以建立6种触发器，即：BEFORE INSERT、BEFORE UPDATE、BEFORE DELETE、AFTER INSERT、AFTER UPDATE、AFTER DELETE。

另外有一个限制是不能同时在一个表上建立2个相同类型的触发器，因此在一个表上最多建立6个触发器。

INSERT 型触发器：插入某一行时激活触发器，可能通过 INSERT、LOAD DATA、REPLACE 语句触发；
UPDATE  型触发器：更改某一行时激活触发器，可能通过 UPDATE 语句触发；
DELETE 型触发器：删除某一行时激活触发器，可能通过  DELETE、REPLACE 语句触发。

**BEGIN … END 详解**
在MySQL中，BEGIN … END 语句的语法为：

BEGIN
[statement_list]
END
其中，statement_list  代表一个或多个语句的列表，列表内的每条语句都必须用分号（;）来结尾。
而在MySQL中，分号是语句结束的标识符，遇到分号表示该段语句已经结束，MySQL可以开始执行了。因此，解释器遇到statement_list  中的分号后就开始执行，然后会报出错误，因为没有找到和 BEGIN 匹配的 END。

这时就会用到 DELIMITER 命令（DELIMITER 是定界符，分隔符的意思），它是一条命令，不需要语句结束标识，语法为：
DELIMITER  new_delemiter
new_delemiter  可以设为1个或多个长度的符号，默认的是分号（;），我们可以把它修改为其他符号，如$：
DELIMITER  $
在这之后的语句，以分号结束，解释器不会有什么反应，只有遇到了$，才认为是语句结束。注意，使用完之后，我们还应该记得把它给修改回来。

**下面举例说明一个完整的数据库触发器例子：**

假设系统中有两个表：
班级表 class(班级号 classID, 班内学生数 stuCount)
学生表 student(学号 stuID,  所属班级号 classID)
要创建触发器来使班级表中的班内学生数随着学生的添加自动更新，代码如下：

DELIMITER $
create trigger tri_stuInsert after insert
on student for each  row
begin
declare c int;
set c = (select stuCount from class where  classID=new.classID);
update class set stuCount = c + 1 where classID =  new.classID;
end$
DELIMITER ;

**变量详解**
MySQL 中使用 DECLARE 来定义一局部变量，该变量只能在 BEGIN … END  复合语句中使用，并且应该定义在复合语句的开头，

即其它语句之前，语法如下：

DECLARE var_name[,...] type [DEFAULT value]
其中：
var_name 为变量名称，同 SQL  语句一样，变量名不区分大小写；type 为 MySQL 支持的任何数据类型；可以同时定义多个同类型的变量，用逗号隔开；变量初始值为 NULL，如果需要，可以使用  DEFAULT 子句提供默认值，值可以被指定为一个表达式。

对变量赋值采用 SET 语句，语法为：

SET var_name = expr [,var_name = expr] ...

**NEW 与 OLD 详解**

上述示例中使用了NEW关键字，和 MS SQL Server 中的 INSERTED 和 DELETED 类似，MySQL 中定义了 NEW 和  OLD，用来表示

触发器的所在表中，触发了触发器的那一行数据。
具体地：
在 INSERT 型触发器中，NEW  用来表示将要（BEFORE）或已经（AFTER）插入的新数据；
在 UPDATE 型触发器中，OLD 用来表示将要或已经被修改的原数据，NEW  用来表示将要或已经修改为的新数据；
在 DELETE 型触发器中，OLD 用来表示将要或已经被删除的原数据；
使用方法：  NEW.columnName （columnName 为相应数据表某一列名）
另外，OLD 是只读的，而 NEW 则可以在触发器中使用 SET  赋值，这样不会再次触发触发器，造成循环调用（如每插入一个学生前，都在其学号前加“2013”）。

**查看触发器**

和查看数据库（show databases;）查看表格（show tables;）一样，查看触发器的语法如下：

SHOW TRIGGERS [FROM schema_name];
其中，schema_name 即 Schema 的名称，在 MySQL 中  Schema 和 Database 是一样的，也就是说，可以指定数据库名，这样就

不必先“USE database_name;”了。

**删除触发器**

和删除数据库、删除表格一样，删除触发器的语法如下：

DROP TRIGGER [IF EXISTS] [schema_name.]trigger_name

**触发器的执行顺序
**

我们建立的数据库一般都是 InnoDB 数据库，其上建立的表是事务性表，也就是事务安全的。这时，若SQL语句或触发器执行失败，MySQL  会回滚事务，有：

①如果 BEFORE 触发器执行失败，SQL 无法正确执行。
②SQL 执行失败时，AFTER 型触发器不会触发。
③AFTER  类型的触发器执行失败，SQL 会回滚。
