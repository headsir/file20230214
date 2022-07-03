# 一、数据表操作

## 1、打开数据库 USE 数据库名称;

## 2、查看当前所在数据库 Select Database();

## 3、创建数据表 CREATE TABLE IF NOT EXISTS (相关设置)

## 4、删除数据表 DROP TABLE 表名;

## 5、数据表插入数据 INSERT INTO  表名 ( 列名 ) VALUES(列对应的值);

## 6、数据表查询	SELECT 列名 FROM 表名 where 条件  LIMIT 返回记录数

## 7、更新数据 	UPDATE 表名 SET 更新列名=更新值

## 8、删除数据	DELETE FROM 表名 where 条件（truncate table 表名;）

## 9、统计计数COUNT**、**平均值AVG**、**求和SUM**、**最大值MAX**、**最小值MIN**

计数  SELECT COUNT(*) FROM 表名 WHERE 条件 

平均值 SELECT AVG(列名) FROM 表名 WHERE 条件

```sql
#计算列为空个数
SELECT count(*) FROM information_bbu WHERE 子网 is NULL;
```



## 10、去重复值 SELECT DISTINCT 列名 FROM 表名 WHERE address IS NOT NULL

## 11、字符拼接  SELECT CONCAT('姓名：',name,'的地址是江西') AS '地址信息' FROM user WHERE address = '江西'

## 12、IFNULL() 函数用于判断第一个表达式是否为 NULL，如果为 NULL 则返回第二个参数的值，如果不为 NULL 则返回第一个参数的值

## 13、分组（透视）	SELECT 分组列 FROM 表名 GROUP BY 分组列;

## 14、表连接

INNER JOIN（内连接,或等值连接）：获取两个表中字段匹配关系的记录。同WHERE表关联

LEFT JOIN（左连接）：获取**左表**所有记录，即使**右表**没有对应匹配的记录。

RIGHT JOIN（右连接）： 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。

select * from 左表 as left_table left join 右表 as right_table on left_table.dept_id = right_table.id

WHERE表关联 select * from person as left_table,dept as right_table where left_table.dept_id = right_table.id;

## 15、多表连接

UPDATE 表名 SET 更新列名=更新值

UPDATE 待更新表  INNER JOIN 库表  ON  待更新表.列名=库表.参照列名 SET 待更新表.列名=库表.需要的列名

案例：

INSERT INTO 子网库(`子网`) SELECT `子网` FROM `5g基站bbu数据` GROUP BY `子网`      插入数据



where 设置单行

### 多表查询

```sql
--单表查询
#整表查询
SELECT * FROM 表名
#单列查询
SELECT 列名 FROM 表名
# 部分查询--使用WHERE限制查询行（列为空is NULL）
--多表查询
SELECT x.GCI,v.子网ID,u.行政区域,w.营业部,s.网元ID,s.基站编号 
FROM `5g工参信息现网x` as x  
INNER JOIN `子网idv` as v ON v.序号 =  x.子网 
INNER JOIN `行政区u` AS u ON u.序号 = x.行政区域
INNER JOIN `营业部w` AS w ON  w.序号 = x.营业部
INNER JOIN `网元信息s` AS s ON s.序号=x.gNBID
WHERE GCI = "4849669-0";

# 条件语句查询,当id 为1时,2倍value,当id为2时,3倍value,当为其它时,4倍value
select id,name,case id when 1 then value * 2
					   when 2 then value * 3
					   else value *4
					   end as "value_if"
from tble;
```

### 更新

```sql
--单表更新
#更新单表单行单列值
UPDATE `小区信息t` as t set t.物理点= 3 WHERE t.序号=3;
#更新单个表单行两列值
UPDATE  `小区信息t` as t set t.物理点= 3 ,t.bbu= 3 WHERE t.序号=2;
#更新单个表两行单列值--需要借助辅助表多表更新
--多表更新
----2张表更新
#更新单列
UPDATE  `小区信息t` AS t
INNER JOIN `基站信息表j` AS j	on t.物理点 = j.基站名称
SET t.物理点=j.序号;
#更新 2列
UPDATE  `小区信息t` AS t
INNER JOIN `基站信息表j` AS j	on t.物理点 = j.基站名称
INNER JOIN `网元信息s` AS s	on t.BBU = s.网元名称
SET t.物理点=j.序号,t.BBU = s.序号;
#3张表更新3列
UPDATE dayworkplan as d 
INNER JOIN information_cell as c on d.站号小区标识=c.GCI
INNER JOIN information_gnb as g on c.`基站名称`=g.`序号`
set d.pci=c.pci,d.`经度`=g.`AAU经度`,d.`纬度`=g.`AAU纬度`
WHERE d.pci is null and d.`经度` is null
```

### 新建表

```sql
CREATE TABLE IF NOT EXISTS 表名(
id INT UNSIGNED AUTO_INCREMENT(自增) COMMENT '主键id',
列名 字符类型(大小) 是否为空（NOT NULL) COMMENT (注释),
 address VARCHAR(40) NOT NULL COMMENT '住址',
create_date DATE COMMENT '创建时间',
 PRIMARY KEY ( id ) 定义列为主键
 )ENGINE(设置存储引擎）=InnoDB DEFAULT CHARSET(设置编码）=utf8mb4;
```

### 添加数据

```sql
#插入一行数据
 INSERT INTO  表名 ( 列名 ) VALUES(列对应的值)
#插入一列数据
INSERT INTO 临时表(id) SELECT i.gNBID FROM information_bbu as i WHERE i.`子网` is null
#建好的表中添加一列：
alter table TABLE_NAME add column NEW_COLUMN_NAME varchar(20) not null;
这条语句会向已有的表中加入新的一列，这一列在表的最后一列位置。如果我们希望添加在指定的一列，可以用：
alter table TABLE_NAME add column NEW_COLUMN_NAME varchar(20) not null after COLUMN_NAME;
注意，上面这个命令的意思是说添加新列到某一列后面。如果想添加到第一列的话，可以用：
alter table TABLE_NAME add column NEW_COLUMN_NAME varchar(20) not null first;
#多表插入数据
-- INSERT INTO  表名 ( 列名 ) VALUES(列对应的值)
INSERT INTo dayworkplan(基站类型,站号小区标识,站名,cellname) SELECT  CONCAT(b.制式,b.`站型`),b.`站号小区号`,b.`站名`,b.`小区名`   FROM basworkpro as b WHERE b.单验日期='未开始安排';
```

### 删除

```
#清空表
DELETE FROM 临时表 
truncate  table 临时表
```

### 复制表

```sql
1、复制表结构及数据到新表
CREATE TABLE 新表SELECT * FROM 旧表
新表中没有了旧表的primary key、Extra（auto_increment）等属性
2、只复制表结构到新表
CREATE TABLE 新表SELECT * FROM 旧表WHERE 1=2
或CREATE TABLE 新表LIKE 旧表
3、复制旧表的数据到新表(假设两个表结构一样)
INSERT INTO 新表SELECT * FROM 旧表
4、复制旧表的数据到新表(假设两个表结构不一样)
INSERT INTO 新表(字段1,字段2,.......) SELECT 字段1,字段2,...... FROM 旧表
5、可以将表1结构复制到表2
SELECT * INTO 表2 FROM 表1 WHERE 1=2
6、可以将表1内容全部复制到表2
SELECT * INTO 表2 FROM 表1
7、 show create table 旧表;
这样会将旧表的创建命令列出。我们只需要将该命令拷贝出来，更改table的名字，就可以建立一个完全一样的表
8、mysqldump
用mysqldump将表dump出来，改名字后再导回去或者直接在命令行中运
9、复制旧数据库到新数据库（复制全部表结构并且复制全部表数据）
#mysql -u root -ppassword
>CREATE DATABASE new_db;
#mysqldump old_db -u root -ppassword--skip-extended-insert --add-drop-table | mysql new_db -u root -ppassword
10、表不在同一数据库中（如，db1 table1, db2 table2)
sql: insert into db1.table1 select * from db2.table2 (完全复制)
insert into db1.table1 select distinct * from db2.table2(不复制重复纪录）
insert into tdb1.able1 select top 5 * from db2.table2 (前五条纪录)
```

### 一行变多行（根据特定字符分割字符串）

```sql
SELECT a.`网元ID`,a.LDN,substring_index(substring_index( a.`PRRU发射接收组`,';',b.help_topic_id + 1 ), ';' ,- 1) AS PRRU发射接收组
FROM (SELECT sec.`网元ID`,sec.LDN,sec.`PRRU发射接收组` FROM sectorfunction as sec  WHERE sec.`PRRU发射接收组` <>"")  as a
JOIN mysql.help_topic b ON b.help_topic_id < (length(a.`PRRU发射接收组`) - length( replace(a.`PRRU发射接收组`, ';', '')  ) + 1)
-- (length(a.`PRRU发射接收组`) - length( replace(a.`PRRU发射接收组`, ';', '')  ) + 1),计算需要分割次数
-- length(a.`PRRU发射接收组`)，计算表格字节数，
-- help_topic，mysql系统表，这里的作用借助此表id自增特性
-- substring_index( a.`PRRU发射接收组`,';',b.help_topic_id + 1 )，b.help_topic_id 变量
```



```
************************************************************************
        *create database <数据库名> character set utf8 【创建数据库时指定数据库的字符集】
        *show databases 【查看存在的数据库】
        *drop database <数据库名> 【删除数据库】
        *USE 库名  【进入数据库】
        *show tables 【显示数据库所有数据表】
        * DESC <表名> 【查看表结构】
        *SELECT 列名 FROM 表名 where 条件  LIMIT 返回记录数  【数据表查询】
        *UPDATE 表名 SET 更新列名=更新值 【更新数据】
        *====================================================================
        *&&&&&&&&&&&&&&&&&&&&&&&&&& 创 建 表 &&&&&&&&&&&&&&&&&&&&&&&&&&
        *====================================================================
        *CREATE TABLE IF NOT EXISTS 表名(
        *    id INT UNSIGNED AUTO_INCREMENT(自增) COMMENT '主键id',
        *    列名 字符类型(大小) 是否为空（NOT NULL) COMMENT (注释),
        *   address VARCHAR(40) NOT NULL COMMENT '住址',
        *    create_date DATE COMMENT '创建时间',
        *    PRIMARY KEY ( id ) 定义列为主键
        *   )ENGINE(设置存储引擎）=InnoDB DEFAULT CHARSET(设置编码）=utf8mb4;
        ======================================================================
        *DROP TABLE 表名  【删除数据表】
        *INSERT INTO  表名 ( 列名 ) VALUES(列对应的值)  【数据表插入数据】
        **********************************************************************
```



## 16、查看表结构

http://c.biancheng.net/view/7199.html

DESC <表名>

## 17、用户权限

![image-20210819220421399](imge/image-20210819220421399.png)

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
