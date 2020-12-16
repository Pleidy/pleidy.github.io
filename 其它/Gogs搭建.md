## Gogs搭建

> 以下方式使用docker镜像搭建

**docker 安装**

```
docker安装：curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
docker启动：systemctl start docker
```

**拉取gogs**

```
搜索镜像：docker search gogs
拉取镜像：docker pull gogs/gogs
```

**映射目录：**`/var/gogs`

```
创建需要映射文件的目录：mkdir -p gogs
```

**镜像运行，生成容器**

```
运行：docker run --name=gogs -d  -p 22:22 -p 3000:3000 -v /var/gogs:/data gogs/gogs

注：需检查端口是否被占用
```

**运行容器**

```
docker start gogs
```

**运行gogs**

```
首次：http://localhost:3000/install
访问：http://106.12.122.168:3000/

注：首次进入需要安装，如不想配置数据库：可选择sqlLite
```

