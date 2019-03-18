# Node 网络编程
* 利用Node搭建网络服务器
* 4个模块
    * net: TCP
        * 两种写法
        ```node
        var net = require('net')
        var server = net.createServer(function (socket) {
          socket.on('data' ,function (data) {
            socket.write('你好')
          })
          socket.on('end' ,function () {
            console.log('连接断开')
          })
          socket.write('欢迎')
        })

        server.listen(8124, function () {
          console.log('server bound')
        })
        ```
        ```node
        var server = new createServer()
        server.on('connection', function(socket) {
          // 新的连接
        })
        server.listen(8124)
        ```
    * dgram: UDP
    * http: HTTP
      ```node
      var http = require('http')
      http.createServer(function (req, res) {
        res.writeHead(200, {'Content-Type': 'text/plain'})
        res.end('Hello World')
      }).listen(1337, '127.0.0.1')
      console.log('Server running at http://127.0.0.1:1337');
      ```
      维持的并发量和QPS不容小觑？？？
      在HTTP两端是服务器和浏览器，B/S模式
      显示一次网络通信的所有报文信息
      ```iterm
      curl -v http://127.0.0.1:1337
      ```
      * 三次握手
      * 发送请求报文
      * 服务端完成处理，向客户端发送响应内容
    * https: HTTPS
    
