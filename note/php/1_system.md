### **PHP 获取对应操作系统属性**

```
$system = php_uname('s');

// 与windows相似度 $percent_windows为一个双精度的值
similar_text("windows",$system,$percent_windows);

//与linux相似度
similar_text("linux",$system,$percent_linux);
```

