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
```

### 3.4.1 复制表

```SQL
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

### 3.7.4 判断第一个表达式是否为 NULL-IFNULL函数

==IFNULL()== 函数用于判断第一个表达式是否为 NULL，如果为 NULL 则返回第二个参数的值，如果不为 NULL 则返回第一个参数的值

### 3.7.5 分组（透视）-GROUP BY 

分组（透视）	SELECT 分组列 FROM 表名 GROUP BY 分组列;

