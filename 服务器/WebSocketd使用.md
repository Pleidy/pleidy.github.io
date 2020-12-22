### websocket简单使用

测试系统：ubuntu、linux

> 下载链接：https://github.com/joewalnes/websocketd/releases

1. 将下载的websocketd包解压，包类型根据不同系统而定

   ```
   # 解压
   unzip websocketd-0.3.0-linux_amd64
   ```

2. 创建监听脚本`socket.sh`：

   ```
   #!/bin/bash
   echo "1";
   ```

3. 服务器端开启socket监听

   ```
   # ./websocketd 为解压后的路径，开启的port不能被占用
   ./websocketd --port=9501 bash socket.sh
   ```

4. 客户端获取/监听

   `test.html`：路径`ws://localhost:9501/`根据实际情况而定

   浏览器中执行html文件，打开控制台，输出 `1` 则表示连接成功

   ```
   <!DOCTYPE html>
   <html>
   <head>
       <title></title>
       <script type="text/javascript">
           var ws = new WebSocket('ws://localhost:9501/');
   
           ws.onmessage = function(event) {
               console.log(event.data);
           };
       </script>
   </head>
   <body>
   </body>
   </html>
   ```

   **websocketd基本操作**

   ```
   var ws = new WebSocket("wss://echo.websocket.org");
   
   ws.onopen = function(evt) { 
     console.log("Connection open ..."); 
     ws.send("Hello WebSockets!");
   };
   
   ws.onmessage = function(evt) {
     console.log( "Received Message: " + evt.data);
     ws.close();
   };
   
   ws.onclose = function(evt) {
     console.log("Connection closed.");
   }; 
   ```

   **简易聊天系统脚本**

   > 来源地址：https://github.com/joewalnes/websocketd/blob/master/examples/bash/chat.sh

   ```
   #!/bin/bash
   
   # Copyright 2013 Jeroen Janssens
   # All rights reserved.
   # Use of this source code is governed by a BSD-style
   # license that can be found in the LICENSE file.
   
   # Run a simple chat server: websocketd --devconsole --port 8080 ./chat.sh
   #
   # Please note that this example requires GNU tail, which is not the default
   # tail on OS X. Even though this script properly escapes the variables,
   # please keep in mind that it is in general a bad idea to read
   # untrusted data into variables and pass this onto the command line.
   
   echo "Please enter your name:"; read USER
   echo "[$(date)] ${USER} joined the chat" >> chat.log
   echo "[$(date)] Welcome to the chat ${USER}!"
   tail -n 0 -f chat.log --pid=$$ | grep --line-buffered -v "] ${USER}>" &
   while read MSG; do echo "[$(date)] ${USER}> ${MSG}" >> chat.log; done
   ```

   