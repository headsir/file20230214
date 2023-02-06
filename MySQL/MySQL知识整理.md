# 一、基础知识

[彭珂的课堂](https://haokan.baidu.com/author/1626161619554481)   https://haokan.baidu.com/author/1626161619554481

https://zhuanlan.zhihu.com/p/21677363

## 1.1 **MySQL 数据类型**

**数值类型**

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

 

**日期和时间类型**

TIMESTAMP类型有专有的自动更新特性。

| **类型**  | **大小**** ****(字节)** | **范围**                                                     | **格式**            | **用途**                 |
| --------- | ----------------------- | ------------------------------------------------------------ | ------------------- | ------------------------ |
| DATE      | 3                       | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3                       | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1                       | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8                       | 1000-01-01 00:00:00/9999-12-31 23:59:59                      | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4                       | 1970-01-01 00:00:00/2038结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYYMMDD HHMMSS     | 混合日期和时间值，时间戳 |

 

**字符串类型**

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

### 1.2 BLOB介绍

### 1.2.1 MySQL的四种BLOB类型

  类型 大小(单位：字节)
　　1.TinyBlob 最大 255
　　2.Blob 最大 65K
　　3.MediumBlob 最大 16M
　　4.LongBlob 最大 4G
　　

### 1.2.2 配置修改

在BLOB中存储大型文件，MYSQL提供了很强的灵活性！允许的最大文件大小，可以在配置文件中设置。
1）Windows中在文件my.ini中配置(在系统盘)
[mysqld]
set-variable = max_allowed_packet=10M
2）linux通过etc/my.cnf
[mysqld]
max_allowed_packet = 10M

## 1.2 编码介绍:

1.2.1 GBK、UTF-8、UTF8MB4

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



# 二、数据库

        *CREATE DATABASE 数据库名 【创建数据库】
        *create database <数据库名> character set utf8 【创建数据库时指定数据库的字符集】
        *show databases 【查看存在的数据库】
        *drop database <数据库名> 【删除数据库】
        *USE 库名  【进入数据库】
        *show tables 【显示数据库所有数据表】
    	*Select Database() 【查看当前所在数据库 】


​    

## 2.1 用户权限

![image-20210819220421399](imge/MySQL知识整理.assets/image-20210819220421399.png)









# 三、数据表

```
*DESC <表名> 【查看表结构】 http://c.biancheng.net/view/7199.html
```





## 3.1 表连接

INNER JOIN（内连接,或等值连接）：获取两个表中字段匹配关系的记录。同WHERE表关联

LEFT JOIN（左连接）：获取**左表**所有记录，即使**右表**没有对应匹配的记录。

RIGHT JOIN（右连接）： 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。

select * from 左表 as left_table left join 右表 as right_table on left_table.dept_id = right_table.id

WHERE表关联 select * from person as left_table,dept as right_table where left_table.dept_id = right_table.id;



## 3.2 查询

```sql
*数据表查询	SELECT 列名 FROM 表名 where 条件  LIMIT 返回记录数

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

## 3.3 更新



```SQL
*更新数据 	UPDATE 表名 SET 更新列名=更新值

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

## 3.4 添加数据

```SQL
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
# 插入数据
replace into
```

### 3.4.1 复制表

```SQL
1、复制表结构及数据到新表
CREATE TABLE 新表 SELECT * FROM 旧表
新表中没有了旧表的primary key、Extra（auto_increment）等属性
2、只复制表结构到新表
CREATE TABLE 新表 SELECT * FROM 旧表 WHERE 1=2
或CREATE TABLE 新表 LIKE 旧表
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

## 3.5 删除

```SQL
* 删除数据	DELETE FROM 表名 where 条件

DELETE FROM 表名
truncate  table 表名【清空表】
DROP TABLE 表名  【删除数据表】

```

## 3.6 新建表

```SQL
*创建数据表 CREATE TABLE IF NOT EXISTS (相关设置)


CREATE TABLE IF NOT EXISTS 表名(
id INT UNSIGNED AUTO_INCREMENT(自增) COMMENT '主键id',
列名 字符类型(大小) 是否为空（NOT NULL) COMMENT (注释),
 address VARCHAR(40) NOT NULL COMMENT '住址',
create_date DATE COMMENT '创建时间',
 PRIMARY KEY ( id ) 定义列为主键
 )ENGINE(设置存储引擎）=InnoDB DEFAULT CHARSET(设置编码）=utf8mb4;
```

## 3.7 数据处理

### 3.7.1 数据统计

统计计数COUNT**、**平均值AVG**、**求和SUM**、**最大值MAX**、**最小值MIN

- 计数COUNT

​		计数  SELECT COUNT(*) FROM 表名 WHERE 条件 

```
#计算列为空个数
SELECT count(*) FROM information_bbu WHERE 子网 is NULL;
```

- 平均值AVG

​		平均值 SELECT AVG(列名) FROM 表名 WHERE 条件

### 3.7.2 去重复值 -DISTINCT函数

SELECT ==DISTINCT 列名== FROM 表名 WHERE address IS NOT NULL

### 3.7.3 字符拼接-CONCAT函数

SELECT ==CONCAT('姓名：',name,'的地址是江西')== AS '地址信息' FROM user WHERE address = '江西'

### 3.7.4 判断第一个表达式是否为 NULL-IFNULL

==IFNULL()== 函数用于判断第一个表达式是否为 NULL，如果为 NULL 则返回第二个参数的值，如果不为 NULL 则返回第一个参数的值

### 3.7.5 分组（透视）-GROUP BY 

分组（透视）	SELECT 分组列 FROM 表名 GROUP BY 分组列;

### 3.7.6 字段排序 -ORDER By 

desc 降序

ase 升序，默认

### 3.7.7 关联表删除特定行-DELETE

```
DELETE nr
FROM  `nr宏站单验跟踪表` as nr
INNER JOIN (SELECT bu.cellname FROM `0不符合单验明细` as bu WHERE bu.`基站名称` is null)  as q on q.cellname=nr.cellname
WHERE  nr.cellname = q.cellname
```

### 3.7.8 提取字符-SUBSTRING_INDEX

```
**SUBSTRING_INDEX(s, delimiter, number)**

**s 为需要分列的字段**

**delimiter 为分隔符**

**number则表示在取第几个分隔符旁边的字段**
```

### 3.7.9 根据指定字符分割成多条数据(一行变多行)

```SQL
select distinct  substring_index( substring_index( a.需要拆分的字段名, ',', b.help_topic_id + 1 ), ',',- 1 ) as name
from 表名 a
join mysql.help_topic b ON b.help_topic_id < ( length( a.需要拆分的字段名 ) - length( replace( a.需要拆分的字段名, ',', '' ) ) + 1 ) 
where a.tt_id =1

-- ( length( a.需要拆分的字段名 ) - length( replace( a.需要拆分的字段名, ',', '' ) ) + 1 ),计算需要分割次数
-- length( a.需要拆分的字段名 )，计算表格字节数
-- mysql.help_topic，mysql系统表（mysql 默认自增序列表），这里的作用借助此表id自增特性
-- substring_index() （mysql  拆分函数）
-- substring_index( a.需要拆分的字段名, ',', b.help_topic_id + 1 )，b.help_topic_id 变量
```

### 3.7.10 将多行数据合并成一行数据

一个字段可能对应多条数据，用mysql实现将多行数据合并成一行数据

例如：一个活动id（activeId）对应多个模块名（modelName）,按照一般的sql语句：

```
1 SELECT am.activeId,m.modelName 
2 FROM activemodel am 
3 JOIN  model m 
4 ON am.modelId = m.modelId 
5 ORDER BY am.activeId
```

查询出的列表为图1所示：

![img](imge/日常记录.assets/1105323-20170304155046438-1517964622.png)

　　　　　　图1

修改过后的sql语句，查询后如图2所示：

```
1 SELECT am.activeId,GROUP_CONCAT(m.modelName SEPARATOR ',') modelName
2 FROM activemodel am 
3 JOIN model m 
4 ON am.modelId=m.modelId
5 WHERE m.valid=1
6 GROUP BY am.activeId
```

需注意：

1.GROUP_CONCAT（）中的值为你要合并的数据的字段名;

　SEPARATOR 函数是用来分隔这些要合并的数据的；

　' '中是你要用哪个符号来分隔；

2.必须要用GROUP BY 语句来进行分组管理，不然所有的数据都会被合并成一条记录，如图3

![img](imge/日常记录.assets/1105323-20170304155249923-92571213.png)

　　　　　　　　　　　　图2

 

![img](imge/日常记录.assets/1105323-20170304160157298-1325436616.png)





### 3.7.11 MySQL 对查询的结果集添加自增序号

```
第一种：
select  (@i:=@i+1)  as i, emp.*  from emp,  (select @i:=0) as it 

第二种：
set @n = 0;
select  (@n := @n + 1) as usid ,id,company_id,department_code from sys_user ;
```

### 3.7.12 字符串截取

> MySQL 字符串截取函数：left(), right(), substring()，常用方法。

1. 左边字符串截取：left(str, length)

   ```
   left(字符串, 3)
   ```

2. 右边字符串截取：right(str, length)

   ```
   right(字符串, 3)
   ```

3. 字符串截取：substring(str, pos); substring(str, pos, len)

- 3.1 从字符串的第 4 个字符位置开始取，直到结束。

  ```
  substring(字符串, 4)
  ```

- 3.2 从字符串的第 4 个字符位置开始取，只取 2 个字符。

  ```
  substring(字符串, 4, 2)
  ```

- 3.3 从字符串的第 4 个字符位置（倒数）开始取，直到结束。

  ```
  substring(字符串, -4)
  ```

- 3.4 从字符串的第 4 个字符位置（倒数）开始取，只取 2 个字符。

  ```
  substring(字符串, -4, 2)
  ```

### 3.7.13 13.批量更新数据

CASE...WHEN语句的涵义与一般高级语言中的SWITCH...CASE语句类似，

如下所示，即：在`表名`表中，当字段`字段3`的值为'值X'时，修改`字段1`与`字段2`的值为'结果X'和'结果X'。

```
UPDATE `表名` SET
        `字段1` = CASE `字段3`
        WHEN '值1' THEN '结果1'
        WHEN '值2' THEN '结果2'
        WHEN '值3' THEN '结果3'
    END,
        `字段2` = CASE `字段3`
        WHEN '值1' THEN '结果4'
        WHEN '值2' THEN '结果5'
        WHEN '值3' THEN '结果6'
    END
WHERE `字段3` IN ('值1', '值2', '值3');
```

### 3.7.14 自动获取日期

括号中为当天时间的前一天，如果统计前几天就将括号中的 '1' 改成相应的天数。如果要算月或年，直接将day改为month或year即可

```date_sub(curdate(),interval 1 day)
date_sub(curdate(),interval 1 day)
```

### 3.7.15 避免插入重复数据

最常见的方式就是为字段设置主键或唯一索引，当插入重复数据时，抛出错误，程序终止，但这会给后续处理带来麻烦，因此需要对插入语句做特殊处理，尽量避开或忽略异常

#### insert ignore into

即插入数据时，如果数据存在，则忽略此次插入，前提条件是插入的数据字段设置了主键或唯一索引，测试SQL语句如下，当插入本条数据时，MySQL数据库会首先检索已有数据（也就是idx_username索引），如果存在，则忽略本次插入，如果不存在，则正常插入数据：

![图片](imge/MySQL知识整理.assets/640.jpeg)

#### on duplicate key update

即插入数据时，如果数据存在，则执行更新操作，前提条件同上，也是插入的数据字段设置了主键或唯一索引，测试SQL语句如下，当插入本条记录时，MySQL数据库会首先检索已有数据（idx_username索引），如果存在，则执行update更新操作，如果不存在，则直接插入：

![图片](imge/MySQL知识整理.assets/640-16734236440283.jpeg)

#### replace into

即插入数据时，如果数据存在，则删除再插入，前提条件同上，插入的数据字段需要设置主键或唯一索引，测试SQL语句如下，当插入本条记录时，MySQL数据库会首先检索已有数据（idx_username索引），如果存在，则先删除旧数据，然后再插入，如果不存在，则直接插入：

![图片](imge/MySQL知识整理.assets/640-16734237138396.jpeg)

#### insert if not exists

即 `insert into … select … where not exist ...` ，这种方式适合于插入的数据字段没有设置主键或唯一索引，当插入一条数据时，首先判断MySQL数据库中是否存在这条数据，如果不存在，则正常插入，如果存在，则忽略：

![图片](imge/MySQL知识整理.assets/640-16734237420849.png)

### 3.7.16 数据的导入

#### **第一种情况：导入部分不包含中文字体**

```
load data infile "文件绝对路径" 	导入文件
into table "表名" 			   目标表
fields terminated by ',' 		每个具体字段内容之间是以逗号隔开的
optionally enclosed by '"' 		描述的是字段的括起字符，就是说字段中如果有引号，就当做是字段的一部分
escaped by '"'					描述的是转义字符。默认的是反斜杠
lines terminated by '\r\n'		对每行进行分割，这里要注意一个问题，如果csv文件是在windows下生成，那分割用 ‘rn’，linux下								  用 ‘n’
IGNORE 1 LINES					是忽略第一行，因为第一行往往是字段名
(Id,@dummy,DayOfWeek,PdDistrict,Address,X,Y); 括号中有个字段很特别 @dummy，它是说如果csv文件中有个字段我不想插进去，那													就把对应字段名变成@dummy。
想顺便插入导入时间，就在最后加上set update_time=current_timestamp；
```

#### **第二种情况：导入数据包含中文字体**

```
load data infile "文件绝对路径" into table "表名" 
character set gb2312 -- 设置编码格式 UTF8
fields terminated by ',' 
optionally enclosed by '"' 
escaped by '"'
lines terminated by '\r\n';
```



## 3.8 触发器

### 3.8.1 触发器语法

        触发器经常用于加强数据的完整性约束和业务规则等。 触发器创建语法四要素：
        1.监视地点(table)
        2.监视事件(insert/update/delete) 
        3.触发时间(after/before) 
        4.触发事件(insert/update/delete)
触发器的特性：

　　1、有begin end体，begin end;之间的语句可以写的简单或者复杂

　　2、什么条件会触发：I、D、U

　　3、什么时候触发：在增删改前或者后

　　4、触发频率：针对每一行执行

　　5、触发器定义在表上，附着在表上。

也就是由事件来触发某个操作，事件包括INSERT语句，UPDATE语句和DELETE语句；可以协助应用在数据库端确保数据的完整性。

```
delimiter 自定义结束符号
create trigger 触发器名字 触发时间 触发事件 on 表 for each row
begin
    -- 触发器内容主体，每行用分号结尾
end
自定义的结束符合

delimiter ;
```

### 3.8.2 Navicat中触发器实现

- 删除：

  OLD表示将要删掉的原数据,触发：after,before 都可用

  ```
  begin
      INSERT INTO student_copy1(`stu_id`, `stu_name`, `stu_gender`, `stu_age`) 
      VALUES (OLD.`stu_id`, OLD.`stu_name`, OLD.`stu_gender`, OLD.`stu_age`);
  end
  ```

- 插入

  new表示将要插入的数据,触发：after,before 都可用

  ```
  begin
      INSERT INTO student_copy1(`stu_id`, `stu_name`, `stu_gender`, `stu_age`) VALUES (new.`stu_id`, new.`stu_name`, new.`stu_gender`, new.`stu_age`);
  end
  ```

  



# 四、SQL中的流程控制

**流程控制的定义**

一般是指用来控制程序执行和流程分至点额命令，一般指的是逻辑计算部分的控制。

**流程控制种类**

常见的流程控制有以下8种

> BEGIN ... END：将多个T-SQL语句合为一个逻辑块
> WAITFOR：挂起语句的执行，直到指定的时间点或者指定的时间间隔
> GOTO：改变程序执行的流程，使程序跳转到标识符指定的程序行再继续往下执行
> WHILE：循环控制，当满足WHILE后面的条件后，就可以循环执行WHILE下面的语句
> IF ... ELSE：表示条件判断
> BREAK：程序完全跳出循环语句
> RETURN：使程序从一个查询、存储过程或批量处理中无条件返回，其后面的语句不再执行
> CONTINUE：命令继续返回执行

## 4.1 **BEGIN...END**

**语法**

> BEGIN 
>
> sql_statement... 
>
> END

**示例**

我们在数据库中打印出我们公众号的名称"SQL数据库开发"

```
DECLARE @A VARCHAR(20)
SET @A='SQL数据库开发'
BEGIN
SELECT @A
END
```

## 4.2 **IF [...ELSE]**

### 4.2.1 **IF语法**

> IF <条件表达式>
>
>  {命令行 | 程序块}

**IF示例**

如果某字符串的长度大于5，就打印该字符串

```
DECLARE @A VARCHAR(20)
SET @A='SQL数据库开发'
IF LEN(@A)>5
SELECT @A
```

### 4.2.2 **IF...ELSE语法**

> IF <条件表达式> 
>
> {命令行 | 程序块} 
>
> ELSE {命令行 | 程序块}

**IF...ELSE示例**

如果字符串的长度大于10，就打印该字符串，否则打印"字符串长度太短"

```
DECLARE @A VARCHAR(20)
SET @A='SQL数据库开发'
IF LEN(@A)>10
SELECT @A
ELSE
SELECT '字符串长度太短'
```

## 4.3 **WHILE**

**语法**

> WHILE <条件表达式>
>
>  {命令行 | 程序块} 
>
> CONTINUE 
>
> {命令行 | 程序块} 
>
> BREAK 
>
> {命令行 | 程序块}

**示例**

有1到10这样一组数字，从1按顺序开始，遇到偶数就跳过，遇到奇数就打印出来，当遇到9就结束打印。

```
DECLARE @i int;
SET @i = 0;
WHILE(@i < 10)
BEGIN
    SET @i = @i + 1;
    IF(@i % 2 = 0)
    BEGIN
        PRINT ('跳过偶数数' + CAST(@i AS varchar));
        CONTINUE;
    END
    ELSE IF (@i = 9)
    BEGIN
        PRINT ('到' + CAST(@i AS varchar) + '就结束打印');
        BREAK;
    END
    PRINT @i;
END
```

## 4.3 RETURN

**语法**

> RETURN [整数表达式]

**示例**

```
BEGIN
    PRINT(1);
    PRINT(2);
    RETURN ;
    PRINT(3); --在RETURN之后的代码不会被执行，因为会跳过当前批处理
    PRINT(4);
END
GO
BEGIN
    PRINT(5);
END
```

## 4.4 GOTO

注意：

- 语句标识符可以是数字或者字母的组合，但必须以":"结束。而在GOTO语句后的标识符不必带":"。
- GOTO语句和跳转标签可以在存储过程、批处理或语句块中的任何地方使用，但不能超出批处理的范围。

**语法**

> GOTO 标识符

**示例**

```
DECLARE @i INT;
SET @i = 1;
PRINT @i;
SET @i = 2;
PRINT @i;
GOTO ME;
SET @i = 3; --这行被跳过了
PRINT @i;

ME:PRINT('跳到我了?');
PRINT @i
```

## 4.5 **WAITFOR**

注意：

WAITFOR常用语某个特定的时间点或时间间隔自动执行某些任务。在WAITFOR语句中不能包含打开游标，定义视图这样的操作。在包含事务的语句中不要使用WAITFOR语句，因为WAITFOR语句在时间点或时间间隔执行期间将一直拥有对象的锁，当事务中包含WAITFOR语句，事务的其他语句又需要访问被锁住的数据对象事就容易发生死锁现象。

**指定时间点的语法**

> WAITFOR  TIME <具体时间>

**示例**

在'08:10:00'执行打印字符串"SQL数据库开发"

```
WAITFOR TIME '08:10:00'
PRINT 'SQL数据库开发'
```

如果你执行这句话，那如果在今天这个点之前，那么等到这个时候它就会打印字符串，如果在今天这个点之后，那你需要等到第二天的这个时间点才会打印。在未执行之前查询窗口是一直"正在执行查询..."状态

![图片](imge/MySQL知识整理.assets/640.png)



**指定等待时间间隔的语法**

> WAITFOR DELAY 'INTERVAR'

INTERVAR为时间间隔，指定执行WAITFOR 语句之前需要等待的时间，最多为24小时。

**示例**

```
WAITFOR DELAY '00:00:03'
PRINT 'SQL数据库开发'
```

在等到3秒钟后，会打印出字符串

# 五、**SQL 的书写规范**

## 5.1 表名

表名要有意义，且标准 SQL 中规定表名的第一个字符应该是字母。

## 5.2注释

有单行注释和多行注释，如下

```
-- 单行注释
-- 从SomeTable中查询col_1 
SELECT col_1
  FROM SomeTable;

/*
多行注释
从 SomeTable 中查询 col_1 
*/
SELECT col_1
  FROM SomeTable;
```

多行注释很多人不知道，这种写法不仅可以用来添加真正的注释，也可以用来注释代码，非常方便

## 5.3缩进

就像写 Java，Python 等编程语言一样 ，SQL 也应该有缩进，良好的缩进对提升代码的可读性帮助很大，以下分别是好的缩进与坏的缩进示例

```
-- 好的缩进
SELECT col_1, 
    col_2, 
    col_3,
    COUNT(*) 
  FROM tbl_A
 WHERE col_1 = 'a'
   AND col_2 = ( SELECT MAX(col_2)
                   FROM tbl_B
                  WHERE col_3 = 100 )
 GROUP BY col_1,
          col_2,
          col_3


-- 坏的示例
SELECT col1_1, col_2, col_3, COUNT(*)
FROM   tbl_A
WHERE  col1_1 = 'a'
AND    col1_2 = (
SELECT MAX(col_2)
FROM   tbl_B
WHERE  col_3 = 100
) GROUP BY col_1, col_2, col_3
```

## 5.4 空格

代码中应该适当留有一些空格，如果一点不留，代码都凑到一起， 逻辑单元不明确，阅读的人也会产生额外的压力，以下分别是是好的与坏的示例

```
-- 好的示例
SELECT col_1
  FROM tbl_A A, tbl_B B
 WHERE ( A.col_1 >= 100 OR A.col_2 IN ( 'a', 'b' ) )
   AND A.col_3 = B.col_3;

-- 坏的示例
SELECT col_1
  FROM tbl_A A,tbl_B B
 WHERE (A.col_1>=100 OR A.col_2 IN ('a','b'))
   AND A.col_3=B.col_3;
```

# 六、数据库开发规范

```
1、所有的数据库对象名称必须使用小写字母并用下划线分割（MySQL大小写敏感，名称要见名知意，最好不超过32字符）
2、禁止在数据中存储图片，文件二进制数据（使用文件服务器）
3、禁止在线上做数据库压力测试
4、禁止从开发环境，测试环境直接连生产环境数据库
5、限制每张表上的索引数量，建议单表索引不超过5个（索引会增加查询效率，但是会降低插入和更新的速度）
6、避免使用ENUM数据类型（修改ENUM值需要使用ALTER语句，ENUM类型的ORDER BY操作效率低，需要额外操作，禁止使用书值作为ENUM的枚举值
7、尽量把所有的字段定义为NOT NULL（索引NULL需要额外的空间来保存，所以需要暂用更多的内存，进行比较和计算要对NULL值做特别的处理）
8、使用timestamp或datetime类型来存储时间
9、同财务相关的金额数据，采用decimal类型（不丢失精度，禁止使用 float 和 double）
10、所有的数据库对象名称禁止使用MySQL保留关键字
11、临时库表必须以tmp为前缀并以日期为后缀（tmp_）
12、备份库和库必须以bak为前缀并以日期为后缀(bak_)
13、所有存储相同数据的列名和列类型必须一致。
14、数据库和表的字符集尽量统一使用utf8（字符集必须统一，避免由于字符集转换产生的乱码，汉字utf8下占3个字节）
15、所有表和字段都要添加注释COMMENT，从一开始就进行数据字典的维护
16、建议使用物理分表的方式管理大数据
17、尽量做到冷热数据分离，减小表的宽度（mysql限制最多存储4096列，行数没有限制，但是每一行的字节总数不能超过65535。列限制好处：减少磁盘io，保证热数据的内存缓存命中率，避免读入无用的冷数据）
18、禁止在表中建立预留字段（无法确认存储的数据类型，对预留字段类型进行修改，会对表进行锁定）
避免使用双%号和like，搜索严禁左模糊或者全模糊（如果需要请用搜索引擎来解决。索引文件具有 B-Tree 的最左前缀匹配特性，如果左边的值未确定，那么无法使用此索）
19、建议使用预编译语句进行数据库操作
20、禁止跨库查询（为数据迁移和分库分表留出余地，降低耦合度，降低风险）
21、禁止select * 查询（消耗更多的cpu和io及网络带宽资源，无法使用覆盖索引）
22、in 操作能避免则避免，若实在避免不了，需要仔细评估 in 后边的集合元素数量，控制在 1000 个之内
23、禁止使用order by rand（）进行随机排序
24、避免建立冗余索引和重复索引（冗余：index（a,b,c) index(a,b) index(a)）
25、禁止给表中的每一列都建立单独的索引
26、区分度最高的列放在联合索引的最左侧
27、尽量把字段长度小的列放在联合索引的最左侧
28、尽量避免使用外键（禁止使用物理外键，建议使用逻辑外键）
29、尽量使用 union all 代替 union
30、拆分复杂的大SQL为多个小SQL（ MySQL一个SQL只能使用一个CPU进行计算）
31、对于程序连接数据库账号，遵循权限最小原则
32超过三个表禁止 join。（需要 join 的字段，数据类型必须绝对一致；多表关联查询时，保证被关联的字段需要有索引。即使双表 join 也要注意表索引、SQL 性能。）
33、在varchar字段上建立索引时，必须指定索引长度，没必要对全字段建立索引，根据实际文本区分度决定索引长度即可。
34、SQL 性能优化的目标：至少要达到 range 级别，要求是 ref 级别，如果可以是 consts最好
35、使用 ISNULL()来判断是否为 NULL 值。
36、尽量不要使用物理删除（即直接删除，如果要删除的话提前做好备份），而是使用逻辑删除，使用字段delete_flag做逻辑删除，类型为tinyint，0表示未删除，1表示已删除
37、如果有 order by 的场景，请注意利用索引的有序性。order by 最后的字段是组合,索引的一部分，并且放在索引组合顺序的最后，避免出现 file_sort 的情况，影响查询性能。
38、索引命名要规范，主键索引名为 pk_ 字段名；唯一索引名为 uk _字段名 ；普通索引名则为 idx _字段名。
39、写完SQL先explain查看执行计划（SQL性能优化）
40、变更SQL操作先在测试环境执行，写明详细的操作步骤以及回滚方案，并在上生产前review。（SQL后悔药）
41、设计数据库表的时候，加上三个字段：主键，create_time,update_time。（SQL规范优雅）
42、写完SQL语句，检查where,order by,group by后面的列，多表关联的列是否已加索引，优先考虑组合索引。（SQL性能优化）
43、SQL命令行修改数据，养成begin + commit 事务的习惯(SQL后悔药)
```

## 6.1 建表规范

表规范

```
临时库表必须以tmp为前缀并以日期为后缀（tmp_）
备份库和库必须以bak为前缀并以日期为后缀(bak_)
```

字段规范

```
主键索引名为 pk_ 字段名；
唯一索引名为 uk _字段名 ；
普通索引名则为 idx _字段名；
```

必有字段

```
pk_id 				int	主键索引,自增
created_time		timestamp（CURRENT_TIMESTAMP）	创建日期，自动添加
updated_time		timestamp（CURRENT_TIMESTAMP）	更新日期，自动更新
delete_flag			tinyint	逻辑删除标识，0表示未删除，1表示已删除
```

操作习惯

```
避免使用ENUM数据类型
字段定义为NOT NULL
采用decimal类型（不丢失精度，禁止使用 float 和 double）

单表索引不超过5个
begin + commit 事务的习惯
尽量使用 union all 代替 union
写完SQL先explain查看执行计划（SQL性能优化）
使用 ISNULL()来判断是否为 NULL 值
```

# 七、日常笔记

## 1. id重新开始自增           

方法1：
truncate table 你的表名
//这样不但将数据全部删除，而且重新定位自增的字段

方法2：
delete from 你的表名
dbcc checkident(你的表名,reseed,0) 
//重新定位自增的字段，让它从1开始

## 2. UNION报错

原因：2个表排序规则不一致导致的

排序规则：

```
utf8mb4_general_ci 

utf8mb4_croatian_ci 
```

## 3. navicat mysql 默认添加更新行时当前日期

### 3.1 创建日期

不需要勾选 根据当前时间戳更新

### 3.2 更新日期

CURRENT_TIMESTAMP

![image-20220318173421056](imge/日常记录.assets/image-20220318173421056.png)



## 4.跳过MySQL密码认证

```
跳过密码认证
1、停止MySQL服务  net stop MySQL
2、启动MySQL服务 mysqlld --skip-grant-tables(跳过密码认证表）
恢复密码认证
1、查找mysql进程  tasklist |findstr mysql
2、杀死MySQL进程  taskkill /F /PID 8920(进程ID)
3、正常启动MySQL服务 net start MySQL
```

## 5.修改group_concat的限制

==多行数据合并==会报这种错误

使用group_concat时，如果行数太多，可能会报错：row 20000 was cut by group_concat()

解决方法是修改mysql中的**group_concat_max_len，**此值大于你要分组的数量即可

**1.查看当前mysql group_concat_max_len**

```
mysql> show variables like '%group_concat%';
+----------------------+---------+
| Variable_name        | Value   |
+----------------------+---------+
| group_concat_max_len | 200 |
+----------------------+---------+
1 row in set (0.00 sec)
```

**2.修改group_concat_max_len**

a)如果不方便重启mysql，可以在mysql状态通过命令设置，如：

```
SET GLOBAL group_concat_max_len=2000000;
SET SESSION group_concat_max_len=2000000;
```

注：此种方式在mysql重启后会读取配置文件重新设置，会导致设置失效，所以建议依旧要修改配置文件

b)修改配置文件，**mysql 5.7版本的配置文件为：/etc/mysql/mysql.conf.d/mysqld.cnf**

在[mysqld]下新增配置：group_concat_max_len = 2000000

重启，通过方式1查看即可。

## 6.导出中文乱码

编码选择如下：

![image-20220528223134607](imge/日常记录.assets/image-20220528223134607.png)



## 7.查询命令

### 7.1 查看mysql的运行时长

```ruby
show global status like 'uptime';
```

### 7.2 查看时间

```ruby
show global variables like '%timeout';
```

### 7.3 查看执行SQL插入、更新文件大小是否超过 max_allowed_packet 

该值设置过小将导致单个记录超过限制后写入数据库失败，且后续记录写入也将失败

```
show global variables like 'max_allowed_packet';
set global max_allowed_packet=1024*1024*28;

"D:\Program Files\mysql-8.0.25-winx64\mysql-8.0.25-winx64\bin\mysqld" -- deaults-file="D:\Program Files\mysql-8.0.25-winx64\my.ini" MySQL
# 指定文件
mysqld --defaults-file="D:\Program Files\mysql-8.0.25-winx64\my.ini"
```

### 7.4 查看当前数据库的默认编码：

```
show variables where Variable_name like 'collation%';
```

查看各表编码：

```
show create table ‘table_name’;
```



## 8. 本地文件导入

```
# 导入文件
local-infile=1

参考：3.7.16
```

