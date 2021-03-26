## docker

##### 镜像

```
// 导入
docker load -i <镜像文件>
// 导出
docker save -o <导出文件名> <镜像名>
// 重命名
dokcer tag 镜像ID <镜像名>
```

##### 容器

```
// 容器导出
docker export <容器名> -o <保存名>

//删除
docker rm -f 容器ID

//镜像->容器   端口设置
docker run -d -it --name lamp -p 80:80 -p 3306:3306 -p 6379:6379 -p9501:9501 -v E:/WEB:/www lamp:v8 bash -c "bash start;bash"
```

##### 项目路径配置(apache)

```
// 配置项目对应地址
cd /etc/apache2/sites-available
```

##### **centos**

```
docker安装：curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
docker启动：systemctl start docker
```

##### docker export和docker save的区别

```
1、docker save保存的是镜像(image)，docker export保存的是容器(container);
2、docker load用来载入镜像包，docker import用来载入容器包，但两者都会恢复为镜像;
```

