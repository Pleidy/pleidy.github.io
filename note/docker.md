## docker



```
//端口设置
docker run -d -it --name lamp -p 80:80 -p 3306:3306 -p 6379:6379 -p9501:9501 -v E:/htdocs:/www lamp:v8 bash -c "bash start;bash"
```

