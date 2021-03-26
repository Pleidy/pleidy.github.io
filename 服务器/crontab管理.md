### crontab

##### 基础命令

```
# 列表
$ crontab -l
# 编辑
$ crontab -e
# 删除
$ crontab -r
```

##### 安装`cron`定时服务，`ubuntu:`

```
$ apt-get install cron
```

##### 使用定时服务

```
# 查看状态
$ service cron status
# 启动
$ service cron start
# 重新载入配置
$ service cron reload
# 重启
$ service cron restart
# 停止
$ service cron stop
```

`注：编辑crontab文件后记得重载定时服务配置`