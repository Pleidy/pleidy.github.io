### Laravel job队列

------

**以database为驱动**

##### 创建所需表格，默认jobs

```
php artisan queue:table
php artisan migrate
```

##### 配置文件`.env`添加或修改

```
QUEUE_CONNECTION=database
// 以上配置修改对应config/queue.php中的配置
```

##### 运行队列进程（需要保持连接）

```
php artisan queue:work
# 每次发布新版本后需重新启动队列进程
# php artisan queue:restart
```

##### 创建队列文件：ProcessPodcast

```
php artisan make:job ProcessPodcast
// job文件会在：app/http/jobs/ProcessPodcast.php
// 执行后在handle方法中处理任务逻辑，可以以日志打印形式来进行测试
```

##### 分发任务（调用）

```
ProcessPodcast::dispatch()
// 可以携带参数，
```

------

##### supervisor进程守护配置

`因为队列进程需要保持连接，可以使用supervisor来进行管理`

```
# ubuntu安装
apt-get install supervisor	
# yum安装
yum install supervisor	
# 其它
...
```

安装后会生成配置文件：supervisor配置文件：`/etc/supervisord.conf`

```
# 打开/etc/supervisord.conf配置文件，找到files对应的路径，在路径下创建管理文件
[include]
files = /etc/supervisord.d/*.ini	# 此路径为自动生成
```

添加进程管理文件（后缀名根据配置文件中的路径来确定）：`jobs.ini`

```
[program:jobs]
# 在相应目录下执行命令
directory=/
# 需要执行的命令
command=php artisan queue:work
# 进程名称
process_name=%(program_name)s_%(process_num)02d
# 在 supervisord 启动的时候也自动启动
autostart=true
# 程序异常退出后自动重启
autorestart=true
# 执行用户
user=root
# 开启的进程数，进程建议小于CPU*2
numprocs=2
# 把 stderr 重定向到 stdout，默认 false
redirect_stderr=true
```

supervisor相关命令

```
# 启动 / 可指定进程启动
supervisorctl start
# 停止
supervisorctl stop
# 重启
supervisorctl restart
# 读取配置文件差异
supervisorctl reread
# 重新加载
supervisorctl reload
```

##### 注意事项

- 使用supervisor进程管理命令之前先启动supervisord，否则程序报错。
  使用命令`supervisord -c /etc/supervisord.conf`启动

- 使用`ps -fe | grep supervisord`查看所有启动过的supervisord服务

- 注意查看supervisor所需的系统函数是否被禁用，若禁用则程序会报错

  