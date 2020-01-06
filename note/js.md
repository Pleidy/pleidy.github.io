## JS

##### 判断数组key是否存在

```
array.hasOwnProperty(key)
```

##### 获取地址栏参数

```
function GetQueryString(data) {
    var reg = new RegExp("(^|&)" + data + "=([^&]*)(&|$)", "i");
    var r = window.location.search.substr(1).match(reg);
    if (r != null) return unescape(r[2]); return null;
}
```

##### 去除字符串空格

```
function  trm(e){
    return e.replace( /^\s+/, "" ).replace( /\s+$/, "" );
}
```

