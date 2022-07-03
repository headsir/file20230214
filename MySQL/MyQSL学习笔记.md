# 1、学习网址

[彭珂的课堂](https://haokan.baidu.com/author/1626161619554481)   https://haokan.baidu.com/author/1626161619554481

https://zhuanlan.zhihu.com/p/21677363

# 2、创建数据库和设置编码(命令)

```
创建数据库和设置编码
CREATE DATABASE 数据库名;
创建数据库时指定数据库的字符集
create database <数据库名> character set utf8;
查看数据库编码格式
show variables like 'character_set_database';
show create database world;
修改数据库的编码格式
alter database <数据库名> character set utf8;
查看存在的数据库
show databases;
MySQL 选择数据库
在你连接到 MySQL 数据库后，可能有多个可以操作的数据库，所以你需要选择你要操作的数据库。
MySQL 删除数据库
在删除数据库过程中，务必要十分谨慎，因为在执行删除命令后，所有数据将会消失。
drop 命令删除数据库
drop 命令格式：
drop database <数据库名>;

```

# 3、编码介绍:GBK、UTF-8、UTF8MB4

```
GBK和UTF-8
GBK:
GBK是在国家标准GB2312基础上扩容后兼容GB2312的标准（好像还不是国家标准）。GBK编码专门用来解决中文编码的，是双字节的。不论中英文都是双字节的。
UTF－8
UTF－8 编码是用以解决国际上字符的一种多字节编码，它对英文使用8位（即一个字节），中文使用24位（三个字节）来编码。对于英文字符较多的论坛则用UTF－8 节省空间。另外，如果是外国人访问你的GBK网页，需要下载中文语言包支持。访问UTF-8编码的网页则不出现这问题。可以直接访问。
UTF8MB4
Emoji 表情（Emoji 是一种特殊的 Unicode 编码，常见于 ios 和 android 手机上），和很多不常用的汉字，以及任何新增的 Unicode 字符等等。
区别：
GBK包含全部中文字符；
UTF-8则包含全世界所有国家需要用到的字符；
UTF8MB4支持4字节，常见就是Emoji表情的存储；
```

# 4、MySQL 创建数据表

## **创建MySQL数据表的SQL语法：**

CREATE TABLE table_name (column_name column_type);

例如，我们在 PENGKE 数据库中创建数据表user，首先，连接MySQL，输入命令：use pengke

选择我们要操作的数据库：

![img](https://www.vipkes.cn/images/upload/datum/42/1.jpg)



## **创建user表，语法如下****：**

CREATE TABLE IF NOT EXISTS `user`(

  `id` INT UNSIGNED AUTO_INCREMENT COMMENT '主键id',

  `name` VARCHAR(8) NOT NULL COMMENT '姓名',

  `address` VARCHAR(40) NOT NULL COMMENT '住址',

  `create_date` DATE COMMENT '创建时间',

  PRIMARY KEY ( `id` )

ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;


![img](https://www.vipkes.cn/images/upload/datum/42/2.jpg)

## **实例解析：**

IF NOT EXISTS即该表不存在。

如果你不想字段为 NULL(空) 可以设置字段的属性为 NOT NULL(不为空)， 在操作数据库时如果输入该字段的数据为NULL ，就会报错。

AUTO_INCREMENT定义当前列为自增的属性，一般用于主键，每一次新增数据，该字段的数值会自动加1。

PRIMARY KEY关键字用于定义列为主键。 可以使用多列来定义主键(复合主键)，列间以逗号分隔。

ENGINE 设置存储引擎，CHARSET 设置编码。

注意：MySQL命令终止符为分号 ; 。

## **执行成功后，就可以****输入****命令****：****desc user****查看****user****表结构：**

![img](https://www.vipkes.cn/images/upload/datum/42/3.jpg)

## **MySQL 删除数据表**

MySQL中删除数据表是非常容易的,所以网上有一个梗(删库跑路)。

例如：删除user表语法：

DROP TABLE user ;

# 5、**MySQL 数据类型**

## **数值类型**

| **类型**     | **大小**                                 | **范围（有符号）**                                           | **范围（无符号）**                                           | **用途**        |
| ------------ | ---------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | --------------- |
| TINYINT      | 1 字节                                   | (-128，127)                                                  | (0，255)                                                     | 小整数值        |
| SMALLINT     | 2 字节                                   | (-32 768，32 767)                                            | (0，65 535)                                                  | 大整数值        |
| MEDIUMINT    | 3 字节                                   | (-8 388 608，8 388 607)                                      | (0，16 777 215)                                              | 大整数值        |
| INT或INTEGER | 4 字节                                   | (-2 147 483 648，2 147 483 647)                              | (0，4 294 967 295)                                           | 大整数值        |
| BIGINT       | 8 字节                                   | (-9,223,372,036,854,775,808，9 223 372 036 854 775 807)      | (0，18 446 744 073 709 551 615)                              | 极大整数值      |
| FLOAT        | 4 字节                                   | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  | 单精度 浮点数值 |
| DOUBLE       | 8 字节                                   | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度 浮点数值 |
| DECIMAL      | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 | 依赖于M和D的值                                               | 依赖于M和D的值                                               | 小数值          |

 

## **日期和时间类型**

TIMESTAMP类型有专有的自动更新特性。

| **类型**  | **大小**** ****(字节)** | **范围**                                                     | **格式**            | **用途**                 |
| --------- | ----------------------- | ------------------------------------------------------------ | ------------------- | ------------------------ |
| DATE      | 3                       | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3                       | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1                       | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8                       | 1000-01-01 00:00:00/9999-12-31 23:59:59                      | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4                       | 1970-01-01 00:00:00/2038结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYYMMDD HHMMSS     | 混合日期和时间值，时间戳 |

 

## **字符串类型**

| **类型**   | **大小**            | **用途**                        |
| ---------- | ------------------- | ------------------------------- |
| CHAR       | 0-255字节           | 定长字符串                      |
| VARCHAR    | 0-65535 字节        | 变长字符串                      |
| TINYBLOB   | 0-255字节           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255字节           | 短文本字符串                    |
| BLOB       | 0-65 535字节        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535字节        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215字节    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215字节    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295字节 | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295字节 | 极大文本数据                    |

------



# 6、**MySQL 插入数据**

**语法**

INSERT INTO插入数据SQL：

INSERT INTO table_name ( field1, field2,...fieldN )

VALUES( value1, value2,...valueN );

如果数据是字符型，必须使用单引号或者双引号，如："value"。

 

**例子：向pengke数据库中的user表，插入一条数据**

![img](https://www.vipkes.cn/images/upload/datum/43/1.jpg)

**SQL:**

INSERT INTO user(name,address,create_date)

VALUES(’彭珂’,’江西’,’2019-05-18’);

![img](https://www.vipkes.cn/images/upload/datum/43/2.jpg)

# 7、**MySQL 查询数据**

**语法**

SELECT查询数据SQL：

SELECT column_name,column_name

FROM table_name

[WHERE Clause]

[LIMIT N][ OFFSET M]

查询语句中，如果需要查询多个表的数据，可以在FROM关键字后面，加上多个表名，表名之间使用逗号(,)分割，并使用WHERE语句来设定查询条件或表之间关联。

SELECT 关键字后面，可以加上需要查询的字段名，可以多个字段、字段名用逗号（,）分割。

SELECT 关键字后面，使用星号（*）来代替所有字段，星号是通配符，相当于全部字段。

使用 LIMIT 属性来设定返回的记录数。

通过OFFSET指定SELECT语句开始查询的数据偏移量。默认情况下偏移量为0。

**例子：查询user表中的全部字段和全部数据**

**SQL语句：****SELECT \* FROM user;**

![img](https://www.vipkes.cn/images/upload/datum/43/3.jpg)

**解析：SELECT是查询关键字，后面的星号(\*)是通配符，也就是全部字段的意思。**

**为什么说这是查询全部数据，因为没有加WHERE查询条件，请看学习资料中的：“WHERE子句”。**

 

**例子：查询user表中的id、name、address这三个字段和全部数据**

**SQL语句:****SELECT id,name,address FROM user;**

![img](https://www.vipkes.cn/images/upload/datum/43/4.jpg)

**解析：可以看到，这样就只查询到指定字段（id、name、address）的内容了。**

# 8、**MySQL 更新****数据**

**语法**

UPDATE 修改数据SQL：

UPDATE table_name SET field1=new-value1, field2=new-value2

[WHERE Clause]

同时更新多个字段，可在SET关键字后加上多个字段和对应的值，使用逗号（,）隔开。

**例子：****更新****user表中的name、address这****两****个字段****（会把全部数据的这两个字段都更新哦，讲WHERE子句的时候，就可以添加条件更新啦，请看学习资料中的：“WHERE子句”）**

**SQL语句:****UPDATE user** **SET** **name** **=** **‘****大神珂****’****,address** **= ‘北京’****;**

![img](https://www.vipkes.cn/images/upload/datum/43/5.jpg)

提示更新完成了，我们查询下数据（你记住查询语句了没？），看看

![img](https://www.vipkes.cn/images/upload/datum/43/6.jpg)

**解析：可以看到，这样就****把user表中，****指定字段（name、address）的内容****更新****了。**

# 9、**MySQL** **删除数据**

**语法**

DELETE 删除数据SQL：

DELETE FROM table_name [WHERE Clause]

如果没有指定 WHERE 子句，对应的表中所有记录将被删除。

**例子：****删除****user表中的全部数据**

**SQL语句:****DELETE FROM user****;**

![img](https://www.vipkes.cn/images/upload/datum/43/7.jpg)

查询下全部数据看看，有没有被删除

![img](https://www.vipkes.cn/images/upload/datum/43/8.jpg)

**解析：可以看到，内容****都被删除****了。**

# 10、MySQL查询中的Where子句

从数据表中查询数据是用SELECT语句。

如果需要有条件地从表中选取数据，可将 WHERE 子句添加到 SELECT 语句中。

**语法**

以下是 SQL SELECT 语句使用 WHERE 子句从数据表读取数据语法：

SELECT field1, field2,...fieldN FROM table_name1, table_name2...

[WHERE condition1 [AND [OR]] condition2.....

1.查询语句中你可以使用一个或者多个表，表之间使用逗号, 分割，并使用WHERE语句来设定查询条件。

2.你可以使用 AND 或者 OR 指定一个或多个条件。

4.WHERE 子句也可以运用于 SQL 的 DELETE 或者 UPDATE 命令。

5.WHERE 子句类似于程序语言中的 if 条件，根据 MySQL 表中的字段值来读取指定的数据。

以下为操作符列表，可用于 WHERE 子句中。

下表中实例假定 A 为 10, B 为 20

| **操作符** | **描述**                                                     | **实例**             |
| ---------- | ------------------------------------------------------------ | -------------------- |
| =          | 等号，检测两个值是否相等，如果相等返回true                   | (A = B) 返回false。  |
| <>, !=     | 不等于，检测两个值是否相等，如果不相等返回true               | (A != B) 返回 true。 |
| >          | 大于号，检测左边的值是否大于右边的值, 如果左边的值大于右边的值返回true | (A > B) 返回false。  |
| <          | 小于号，检测左边的值是否小于右边的值, 如果左边的值小于右边的值返回true | (A < B) 返回 true。  |
| >=         | 大于等于号，检测左边的值是否大于或等于右边的值, 如果左边的值大于或等于右边的值返回true | (A >= B) 返回false。 |
| <=         | 小于等于号，检测左边的值是否小于于或等于右边的值, 如果左边的值小于或等于右边的值返回true | (A <= B) 返回 true。 |

 

**例子（为了初学者更好审题，写的比较清晰，实际不会有这么清晰的题哈）：**

例1：查询user表中，name值为“彭珂”的用户全部信息。

SELECT * FROM USER WHERE name = '彭珂';

例2：查询user表中，name值为“彭珂”的用户的id和地址信息。

SELECT id,address FROM USER WHERE name = '彭珂';

例3：查询user表中，address值不为“江西”的用户全部信息。

SELECT * FROM USER WHERE address != '江西';

例4：查询user表中，create_date值大于2019-05-23的用户全部信息。

SELECT * FROM USER WHERE create_date > '2019-05-23';

例5：查询user表中，address值为“江西”并且create_date值大于 2015-05-23的用户全部信息。（此题为课后作业，下节课讲解[MySQL AND和OR]。）

例6：查询user表中，name值为“彭珂”或者是“大神珂”的用户全部信息。（此题为课后作业，下节课讲解[MySQL AND和OR]。）

# 11、MySQL更新操作中的Where子句

从数据表中更新数据是用UPDATE... SET...语句。

如果需要有条件地从表中更新数据，可将 WHERE 子句添加到 UPDATE 语句中。

**语法**

以下是 SQL UPDATE 语句使用 WHERE 子句从数据表更新数据语法：

UPDATE table_name SET field1=new-value1,field2=new-value2 [WHERE condition1 [AND [OR]] condition2.....]

**例子（为了初学者更好审题，写的比较清晰，实际不会有这么清晰的题哈）：**

例1：更新user表中，name值为“彭珂”的用户的address值为“湖南”。

UPDATE USER SET address = '湖南' WHERE name = '彭珂';

例2：更新user表中，name值为“彭珂”的用户的address值为“江西”和创建时间值为“2019-05-25”。

UPDATE USER SET address = '江西',create_date = '2019-05-25' WHERE name = '彭珂';

例3：更新user表中，address值为“江西”并且create_date值大于 2015-05-23的用户的姓名值为“小甜”。（此题为课后作业，下节课讲解[MySQL AND和OR]。）

例4：更新user表中，name值为“彭珂”或者是“大神珂”的用户的address值为“上海”。（此题为课后作业，下节课讲解[MySQL AND和OR]。）

# 12、我们在使用where子句时，经常遇到不仅仅满足一个条件，而是多个条件，这时候我们需要用到：and 和 or、Like、in、is null

and就是'与'得意思,并列,两个条件要都成立。

or就是'或'得意思,只要其中一个条件成立就可以了

SQL LIKE 子句中使用百分号 %字符来表示任意字符，类似于UNIX或正则表达式中的星号 *。

如果没有使用百分号 %, LIKE 子句与等号 = 的效果是一样的。

IN 和 NOTIN，顾名思义，是 在....  和 不在...。

not in 把null也排除，用 in 也不会包含null。所以如有存在null空值，则那条记录不会被查询出。

**MySQL NULL 值处理**

MySQL使用 SQL SELECT 命令及 WHERE 子句来读取数据表中的数据,但是当提供的查询条件字段为 NULL 时，该命令可能就无法正常工作。

为了处理这种情况，MySQL提供了三大运算符:

**IS NULL: 当列的值是NULL,此运算符返回true。**

**IS NOT NULL: 当列的值不为NULL, 运算符返回true。**

**<=>: 比较操作符（不同于=运算符），当比较的的两个值为NULL时返回true。**

关于 NULL 的条件比较运算是比较特殊的。**不能使用 = NULL 或 != NULL 在列中查找 NULL 值 。**

在MySQL中，NULL值与任何其它值的比较（即使是NULL）永远返回false，即 NULL = NULL 返回false 。

**MySQL中处理NULL使用IS NULL和IS NOT NULL运算符。** 

直接举个两个例子就很清晰了：

**例1：****查询user表中，地址不是NULL的用户全部信息。**

SELECT * FROM user WHERE address IS NOT NULL;

**例2：查询user表中，创建时间是NULL的用户全部信息。**

SELECT * FROM user WHERE create_date IS NULL;

# 13、**MySQL统计计数COUNT**、**平均值AVG**、**求和SUM**、**最大值MAX**、**最小值MIN**

count函数是用来统计表中或数组中记录的一个函数。

count(*) 它返回检索行的数目，不论其是否包含 NULL值。

count(列名)，会排除NULL值。

count(主键)效率高于count（加了索引的列名），count（列名）效率再低一点，如果那列离的越远的话，其实效率是会更低的。

**拓展：**

count(1)与count(*)比较：

1.如果你的数据表没有主键，那么count(1)比count(*)快

2.如果有主键的话，那主键（联合主键）作为count的条件也比count(*)要快

3.如果你的表只有一个字段的话那count(*)就是最快的啦

4.count(*) count(1) 两者比较。主要还是要count(1)所相对应的数据字段。如果count(1)是聚索引,id,那肯定是count(1)快。但是差的很小的。

因为count(*),自动会优化指定到那一个字段。所以没必要去count(?)，用count(*),sql会帮你完成优化的。

如果在开发中确实需要用到count()聚合，那么优先考虑count(*)，因为mysql数据库本身对于count(*)做了特别的优化处理。有主键或联合主键的情况下，count(*)略比count(1)快一些。没有主键的情况下count(1)比count(*)快一些。如果表只有一个字段，则count(*)是最快的。使用count()聚合函数后，最好不要跟where age =  1；这样的条件，会导致不走索引，降低查询效率。除非该字段已经建立了索引。使用count()聚合函数后，若有where条件，且where条件的字段未建立索引，则查询不会走索引，直接扫描了全表。count(字段),非主键字段，这样的使用方式最好不要出现。因为它不会走索引。

**直接举两个例子就很清晰了：**

**例1：****统计user表中，共有数据条数。**

SELECT COUNT(*) FROM user; 

**例2：统计user表中，创建时间大于2019-03-05的数据条数。**

SELECT COUNT(*) FROM user WHERE create_date > ‘2019-03-05’;

# 14、MySQL过滤重复数据DISTINCT

**MySQ DISTINCT去重**

在数据表中，可能有的字段会包含重复值。这并不成问题，不过，有时您也许希望仅仅列出不同（distinct）的值。

关键词 DISTINCT 用于返回唯一不同的值。

 **直接举个例子就很清晰了：**

 **例1：****统计user表中，查询所有的地址，地址不能重复。**

 SELECT DISTINCT address FROM user WHERE address IS NOT NULL;

# 15、MySQL_字符串拼接CONCAT

**MySQL_字符串拼接CONCAT**

使用MySQL中的CONCAT（a,b）函数，可以将字符串a和b进行拼接。

**直接举个例子就很清晰了：**

**例1：查询出地址为江西用户的姓名，并且结果格式为：“xxx的地址是江西”。**

SELECT CONCAT(name,'的地址是江西') AS '地址信息' FROM user WHERE address = '江西';

**例****2****：****查询出地址为江西用户的姓名，并且结果格式为：“姓名：xxx的地址是江西”。**

SELECT CONCAT('姓名：',name,'的地址是江西') AS '地址信息' FROM user WHERE address = '江西';

# 16、MySQL_获取字符串位置LOCATE

**MySQL_获取字符串位置LOCATE**

使用MySQL中的LOCATE（substring,str）函数，可以在字符串str中，搜索substring字符或字符串，**搜索到的第一次出现位置**，作为返回值返回。

**直接举个例子就很清晰了：**

**例1：****搜索字符串“pengke”中，“e”所在的位置。**

SELECT LOCATE('e','pengke') FROM DUAL;

# 17、使用MySQL中的UCASE（str）函数，可以将字符串转换成大写、**字母变小写LCASE**

**直接举个例子就很清晰了：**

**例1：查询全部用户信息，并且将他们的英文名称用****大****写展示。**

SELECT UCASE(en_name) FROM user;

**例2：更新全部用户的英文名称为****大****写。**

UPDATE user SET en_name = UCASE(en_name);

# 18、IFNULL() 函数用于判断第一个表达式是否为 NULL，如果为 NULL 则返回第二个参数的值，如果不为 NULL 则返回第一个参数的值。IFNULL() 函数语法格式为：IFNULL(expression, alt_value)

**直接举个例子就很清晰了：**

**例1：****查询全部用户信息，如果用户地址信息为NULL时，则用“未知”代替。**

SELECT id,name,IFNULL(address,'未知') AS address,create_date,age,en_name FROM user;

# 19、MySQL_排序ORDER BY

**MySQL_ORDER BY**

如果要对读取的数据进行排序，可以使用 MySQL 的 ORDER BY 子句来设定字段进行排序，再返回搜索结果。

1.可以使用任何字段来作为排序的条件，返回排序后的查询结果。

2.可以设定多个字段来排序。

3.可以使用 ASC 或 DESC 关键字来设置查询结果是按**升序（从小到大，越小的在前面）**或**降序排列（从大到小，越大的在前面）**。 **默认升序排列**。

4.还可以添加 WHERE...LIKE 子句来设置条件。

**直接举个例子就很清晰了：**

**例1：****查询全部用户信息，结果以创建时间降序排序（时间离现在越近得在最前面）。**

SELECT * FROM user ORDER BY create_date DESC;

**例2：查询全部用户信息，结果以年龄升序排序（年龄最小的在最前面）。**

SELECT * FROM user ORDER BY age ASC;

# **20、MySQL_分组GroupBy（透视）**

**MySQL_GroupBy**

1.GROUP BY 语句根据一个或多个列对结果集进行分组。

2.在分组的列上可以使用 COUNT, SUM, AVG,等函数。

**直接举个例子就很清晰了：**

**例1：查询全部用户信息，结果按 [Excel宏密码破解.md](Excel宏密码破解.md) 地址分组展示。**

SELECT  address  FROM user GROUP BY address;

**例2：查询全部用户信息，结果按年龄分组，并且求出每组年龄的和。**

SELECT age ,SUM(age) FROM user GROUP BY age;

# 21、MySQL_分页LIMIT

**MySQL_LIMIT**

使用MySQL LIMIT子句来限制SELECT语句返回记录的行数。

在SELECT语句中使用LIMIT子句来约束结果集中的行数。LIMIT子句接受一个或两个参数。两个参数的值**必须为零或正整数**。

LIMIT子句语法：

SELECT 

  column1,column2,...

FROM

  table

LIMIT offset , count;

offset参数指定要返回的第一行的偏移量。第一行的偏移量为0，而不是1。count指定要返回的最大行数。

 **直接举个例子就很清晰了：**

 **例1：****按年龄升序，查询最前面的5条全部用户信息。**

SELECT * FROM user ORDER BY age LIMIT 0,5;

**例2：需要做一个分页功能，每页显示3条数据，数据按年龄升序排序。请查询出第二页的用户信息。**

SELECT * FROM user ORDER BY age LIMIT 3,3;

**分页中的offset：（每页显示的数量 \* 页码） - 每页显示的数量。****![img](https://www.vipkes.cn/images/upload/datum/66/1.png)

第一页：**![img](https://www.vipkes.cn/images/upload/datum/66/2.png)**

第二页：**![img](https://www.vipkes.cn/images/upload/datum/66/3.png)**

第三页：**![img](https://www.vipkes.cn/images/upload/datum/66/4.png)**

# 22、MySQL_主键约束

**MySQL_主键约束**

主键约束：primary key。

对表中的数据进行限定，保证数据的**正确性、有效性和完整性**。 

 \1. 含义：**非空且唯一**。

 \2. 一张表只能有一个字段为主键。

 \3. 主键就是表中记录的唯一标识。

**主键：**

1.在创建表时，添加主键约束

​    create table stu(

​      id int primary key,-- 给id添加主键约束

​      name varchar(20)

​    );

 

2.删除主键

​    -- 错误 alter table stu modify id int ;

​    ALTER TABLE stu DROP PRIMARY KEY;

 3.创建完表后，添加主键

​    ALTER TABLE stu MODIFY id INT PRIMARY KEY;

 **自动增长：**

\1. 概念：如果某一列是**数值类型**的，使用 auto_increment 可以来完成值得自动增长

注：数据类型参考：[MySql数据类型](https://www.vipkes.cn/datumPreviewData.html?id=41)

​    \2. 在创建表时，添加主键约束，并且完成主键自增长

​    create table stu(

​      id int primary key auto_increment,-- 给id添加主键约束

​      name varchar(20)

​    );

​    \3. 删除自动增长

​    ALTER TABLE stu MODIFY id INT;

​    \4. 添加自动增长

​    ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;

#  23、MySQL_非空约束

**MySQL非空约束**

非空约束 NOT NULL：

强制列不能为 NULL 值，约束强制字段始终包含值。意味着，如果不向字段添加值，就无法插入新记录或者更新记录。

 \1. 创建表时，设置某些字段非空约束。

CREATE TABLE `new_table` (

`id`  int(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主键id' ,

`name`  varchar(8) NOT NULL COMMENT '姓名' ,

`address`  varchar(40) NULL DEFAULT NULL COMMENT '住址' ,

`create_date`  date NULL DEFAULT NULL COMMENT '创建时间' ,

`age`  int(3) NULL DEFAULT 0 COMMENT '年龄' ,

`en_name`  varchar(100) NULL DEFAULT NULL COMMENT '英文名' ,

PRIMARY KEY (`id`)

\2. 表创建之后（通过 ALTER TABLE 语句）来删除not null约束：

ALTER TABLE new_table MODIFY name varchar(8);

查看表结构: DESC new_table;

 \3. 在表创建之后（通过 ALTER TABLE 语句）来增加not null约束：

ALTER TABLE new_table MODIFY name varchar(8) not null;

# 24、MySQL_表连接

**MySQL_表连接**

在一张表中对数据的读取，是非常简单的，但是在真正的应用中经常需要从多个数据表中读取数据。

对多个数据表进行关联，使用 MySQL 的 JOIN 在两个或多个表中查询数据。

也可以在 SELECT, UPDATE 和 DELETE 语句中使用 Mysql 的 JOIN 来联合多表查询。

JOIN 按照功能大致分为如下三类：

INNER JOIN（内连接,或等值连接）：获取两个表中字段匹配关系的记录。

LEFT JOIN（左连接）：获取**左表**所有记录，即使**右表**没有对应匹配的记录。

RIGHT JOIN（右连接）： 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。

![img](https://www.vipkes.cn/images/upload/datum/69/1.png)

**1.员工表（person），表结构：**

**![img](https://www.vipkes.cn/images/upload/datum/69/2.png)**



**附加SQL（直接复制以下代码，运行后即可创建person表）：**

CREATE TABLE `person` (

`id`  int(11) NOT NULL AUTO_INCREMENT COMMENT '员工编号' ,

`name`  varchar(10) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '姓名' ,

`job`  varchar(9) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '岗位' ,

`mgr`  int(5) NULL DEFAULT NULL COMMENT '直系领导编号' ,

`hire_date`  date NULL DEFAULT NULL COMMENT '入职日期' ,

`dept_id`  int(5) NULL DEFAULT NULL COMMENT '部门编号' ,

PRIMARY KEY (`id`)

)

ENGINE=InnoDB

DEFAULT CHARACTER SET=utf8 COLLATE=utf8_general_ci

AUTO_INCREMENT=7

ROW_FORMAT=DYNAMIC

;

**2.** **部门表（dept），表结构：**

**![img](https://www.vipkes.cn/images/upload/datum/69/3.png)**

附加SQL（直接复制以下代码，运行后即可创建****dept****表）：**

CREATE TABLE `dept` (

`id`  int(11) NOT NULL AUTO_INCREMENT COMMENT '部门编号' ,

`name`  varchar(14) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '部门名称' ,

`loc`  varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '地址' ,

PRIMARY KEY (`id`)

)

ENGINE=InnoDB

DEFAULT CHARACTER SET=utf8 COLLATE=utf8_general_ci

AUTO_INCREMENT=4

ROW_FORMAT=DYNAMIC

;

LEFT JOIN（左连接）：**

![img](https://www.vipkes.cn/images/upload/datum/69/4.png)

**直接举个例子就很清晰了：**

**例1：****查询所有员工和对应的部门信息。**

select * from person as left_table left join dept as right_table on left_table.dept_id = right_table.id;

**RIGHT JOIN（右连接）：**

![img](https://www.vipkes.cn/images/upload/datum/69/5.png)

**例1：查询所有****部门****和对应的****员工****信息。**

select * from person as left_table right join dept as right_table on left_table.dept_id = right_table.id;

**INNER JOIN（内连接、等值连接）：**

**![img](https://www.vipkes.cn/images/upload/datum/69/6.png)**



**例1：查询所有****有部门的****员工信息。**

select * from person as left_table inner join dept as right_table on left_table.dept_id = right_table.id;

**WHERE表关联**

**例1：查询所有有部门的员工信息。**

select * from person as left_table,dept as right_table where left_table.dept_id = right_table.id;