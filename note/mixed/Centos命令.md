# CentOS7 常用命令

```
pwd	显示工作路径

cat file1	从第一个字节开始正向查看文件的内容
head -2 file1	查看一个文件的前两行
more file1	查看一个长文件的内容
tac file1	从最后一行开始反向查看一个文件的内容
tail -3 file1	查看一个文件的最后三行
vi file	打开并浏览文件

文本内容处理
grep str /tmp/test	在文件 ‘/tmp/test’ 中查找 “str”
grep ^str /tmp/test	在文件 ‘/tmp/test’ 中查找以 “str” 开始的行
grep [0-9] /tmp/test	查找 ‘/tmp/test’ 文件中所有包含数字的行
grep str -r /tmp/*	在目录 ‘/tmp’ 及其子目录中查找 “str”
diff file1 file2	找出两个文件的不同处
sdiff file1 file2	以对比的方式显示两个文件的不同

查询操作
find / -name file1	从 ‘/’ 开始进入根文件系统查找文件和目录
find / -user user1	查找属于用户 ‘user1’ 的文件和目录
find /home/user1 -name *.bin	在目录 ‘/ home/user1’ 中查找以 ‘.bin’ 结尾的文件

网络相关
ifconfig eth0	显示一个以太网卡的配置
ifconfig eth0 192.168.1.1 netmask 255.255.255.0	配置网卡的IP地址
ifdown eth0	禁用 ‘eth0’ 网络设备
ifup eth0	启用 ‘eth0’ 网络设备
iwconfig eth1	显示一个无线网卡的配置
iwlist scan	显示无线网络
ip addr show	显示网卡的IP地址

系统相关
top	罗列使用CPU资源最多的linux任务 （输入q退出）
pstree	以树状图显示程序
passwd	修改密码
df -h	显示磁盘的使用情况

```