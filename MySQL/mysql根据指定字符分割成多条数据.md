

# mysql根据指定字符分割成多条数据

```sql
select distinct  substring_index( substring_index( a.需要拆分的字段名, ',', b.help_topic_id + 1 ), ',',- 1 ) as name
from 表名 a
join mysql.help_topic b ON b.help_topic_id < ( length( a.需要拆分的字段名 ) - length( replace( a.需要拆分的字段名, ',', '' ) ) + 1 ) 
where a.tt_id =1
substring_index() （mysql  拆分函数）
mysql.help_topic （mysql 默认自增序列表）
```