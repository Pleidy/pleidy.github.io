## SQL

> 表名：tablename
>
> 字段名：fieldname

##### table结构打印

```
DESC TABLE tablename;
```

##### table显示创建语句

```
SHOW CREATE TABLE tablename;
```

##### table临时表

```
CREATE TEMPORARY TABLE ...
```

##### table开启慢查询记录

```
(永久)
mysqld.cnf文件配置
slow_query_log = 1;// 开启慢查询记录
long_query_time = 1;// 查询时长
log_output = TABLE,FILE;// 记录格式:表格记录，日志文件

（临时）
show VARIABLES；// 查看对应配置信息
配置以上信息，例：set global slow_query_log = 1;
...

其它配置信息：
log_queries_not_using_indexes; // 记录未添加索引值
```

##### 函数

```
rtrim() //前空格
ltrim() //后空格
replace() //所有空格
LENGTH() //计算字符串长度

例：UPDATE tablename SET fieldname=replace(fieldname,' ','');
SELECT replace(fieldname,' ','') as f FROM `tablename`;
```



