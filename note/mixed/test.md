## php项目github自动pull到服务器

> 项目名：web

##### 一、自动触发

**1、在服务器添加脚本文件**：`gitpull.sh`

```cmake
#!/bin/sh
cd /www/web
git reset --hard origin/master 
git clean -f 
git pull 2>&1 
git checkout master

# 注：若脚本文件路径不在www用户组的路径下，需要在www对应用户组路径下添加脚本文件
# 查看用户组路径(找到对应用户组即可)：cat /etc/passwd
```



**2、创建脚本（`gitpull.sh`）回调接口**：www.web.com/git-hook

```php
<?php
    exec('sh ~/gitpull.sh 2>&1', $a, $b);
	//print_r($a); # 若不能正常拉取，可开启答应查看错误信息
    //echo PHP_EOL;
    //print_r($b);
# 注：接口名称自定义，exec执行脚本路径根据实际路径而定
```



**3、在github项目中添加回调路径**

> web > Settings > Webhooks > add webhook

- Payload URL：（填写回调地址：www.test.com/git-hook）
- Content type：（若不需要记录git操作信息可不管）
- Which events would you like to trigger this webhook?（建议选择：Just the `push` event.）
- Active：（默认选择）



**其它：**

- 需要www用户组对脚本文件的操作权限

- 需要www用户组对web有读写权限

- .git/FETCH_HEAD也需要给与相应的权限

- .git/config文件中url需要带上用户名及密码，否则拉取会报错

  ```cmake
  [remote "origin"]
  	url = https://{username}:{password}@github.com/{username}/web.git
  ```

- 确认脚本文件gitpull.sh文件格式

  ```cmake
  vim gitpull.sh
  :set ff
  :set ff=unix
  :wq
  # 注：若执行命令:set ff左下角显示fileformat=unix，则说明格式正常，不需要后续操作，直接退出即可
  ```

------



##### 二、主动触发

**1、创建脚本文件（同上）**

**2、在服务器中添加定时任务，每分钟执行**

