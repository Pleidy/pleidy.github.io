###  ubuntu php环境卸载安装



**卸载php版本**

一、删除php的相关包及配置

```
sudo apt-get autoremove php*
```

二、删除关联

```
sudo find /etc -name "*php*" |xargs  rm -rf
```

三、清除dept列表

```
sudo apt purge `dpkg -l | grep php| awk '{print $2}' |tr "\n" " "`
```

四、检查是否卸载干净（无返回就是卸载完成）

```
 dpkg -l | grep php
```



**安装**

安装php7.4版本，因为之前的PHP版本已经从ubuntu中的包管理移除了，所以我们需要重新添加授权

```
sudo add-apt-repository ppa:ondrej/php && apt-get update
sudo apt-get install php7.4 
# 必要扩展安装
sudo apt-get install php7.4-fpm php7.4-mysql php7.4-curl php7.4-json php7.4-mbstring php7.4-xml php7.4-intl php7.4-dev php-pear
```

通过上面的命令可以安装php7.4版本，如果提示add-apt-repository 不存在可以执行下面的命令

```
apt-get install software-properties-common
```



完成

------

**若出现（111：Connection refused）错误，可尝试代理安装进行解决**

1、安装npm

```
sudo apt-get install npm
```

2、下载安装http代理

```
sudo npm i -g http-proxy-to-socks
```

3、使用代理

```
sudo apt-get -oAcquire::Http::Proxy= update
```

再执行一遍 

```
sudo apt update
```