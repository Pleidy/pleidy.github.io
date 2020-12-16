## windows创建cmd脚本文件

1、文件格式：cmd/bat（cmd文件可使用命令较多，系统需windows2000以上）

2、命令

- echo（打开`on`、关闭`off`请求回显功能或消息显示，默认为回显）

  ```cmake
  echo '11'
  ## 输出11
  ```

- @（不显示@后面的命令）

  ```cmake
  @echo　off
  ## 此命令之后，只展示后续命令的结果，不展示命令语句
  ```

  

3、例子:

```cmake
@echo off
cd /www/aa
## 执行后，将直接进入aa项目目录下，过程中不会展示命令语句
```

