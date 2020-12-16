## MYSQL

##### ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using passwor:yes) 解决办法

```
windows:
1、mysql.ini文件，
在[mysqld]后面任意一行添加“skip-grant-tables”用来跳过密码验证的过程，重启
2、
mysql> use mysql;
mysql> update user set password=password("你的新密码") where user="root";
mysql> flush privileges;
mysql> quit
3、注释配置文件中的：skip-grant-tables,重启
```

##### **创建视图表**

```
create view view_table as select * from table;
```

