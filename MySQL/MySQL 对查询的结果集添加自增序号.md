# MySQL 对查询的结果集添加自增序号，两种写法

第一种：

select <font size= 4 color='red'> (@i:=@i+1)  as i </font>, emp.*  from emp, <font size= 4 color='red'> (select @i:=0) as it </font>

第二种：

<font size= 4 color='red'>set @n = 0</font>;
select  <font size= 4 color='red'>(@n := @n + 1) as usid</font> ,id,company_id,department_code from sys_user ;