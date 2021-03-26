### ubuntu 本地代理安装

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

成功