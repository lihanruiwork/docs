* 客户端与服务器之间多次通信开销巨大，所以缓存并重复利用之前获取的资源是性能优化的一种
* 最佳无需与服务器通信
* 浏览器自带http缓存
* 先检查本地缓存，如果已过期，将验证令牌放在If-None-Match，发送至服务器
* 如果资源未发生变化，则返回304（Not Modified），此时直接用原资源，这些浏览器都会自动做
* Etag：验证令牌
* Cache-Control：
  * no-cache 必须和服务器确认响应是否有变化
  * no-store 禁止缓存，必须向服务器请求，下载完整资源
  * max-age 允许缓存被重用的最长时间
  * private 不可以用中间缓存
* 在资源内容发生变化时更改其网址，强制用户下载新响应 
