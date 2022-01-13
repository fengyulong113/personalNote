# HTTP

## HTTP的请求方法
- **GET**：通常用来获取资源。
- **HEAD**：获取资源的元信息。
- **POST**：提交数据，即上传数据。
- **PUT**：修改数据。
- **DELETE**：删除资源。
- **CONNECT**：建立连接隧道，用于代理服务器。
- **OPTIONS**：列出可对资源实行的请求方法，用来跨域请求。
- **TRACE**：追踪请求-响应的传输路径。
  
### GET和POST的区别
- GET请求会被浏览器主动缓存下来，留下历史记录，而POST不会。
- GET只能对URL编码，只能接收ASCII字符，而POST没有限制。
- GET一般放在URL中，因此不安全，POST放在请求体中，更适合传输敏感信息。
- GET请求会把请求报文一次性发出去，而POST会分为两个TCP包，首先发送header部分，如果服务器响应，然后在发送body部分。
  
## HTPP状态码
- **1xx**：表示目前是协议处理的中间状态，还需要后续操作。
- **2xx**：表示成功状态。
- **3xx**：重定向状态，资源位置发生变动，需要重新请求。
- **4xx**：请求报文有误。
- **5xx**：服务器端发生错误。

#### 1xx
**101 Switching Protocols**。在`HTTP`升级为`WebSocket`的时候，如果服务器同意变更，就会发送状态码101.
#### 2xx
**200 OK**成功状态码。通常在响应体中放有数据。
**204 NO Content**含义与200一致，但响应头后没有body数据。
**206 Partial Content**表示部分内容，它的使用场景为HTTP分块下载和断点续传，当然也会带上相应的响应头字段`Content-Range`。
#### 3xx
**301 Moved Permanently**即永久重定向，对应着**302 Found**，即临时重定向。
**304 Not Modified**当协商缓存命中时会返回这个状态码。
#### 4xx
**400 Bad Request**：笼统地提示了一下错误，并不知道哪里出错了。
**403 Forbidden**：服务器禁止访问，原因有很多，比如法律禁止，信息敏感。
**404 Not Found**：资源未找到，表示在服务器上没有找到相应的资源。
**405 Method Not Allowed**:请求方法不被服务器端允许。
**406 Not Acceptable**：资源无法满足客户端的条件。
**408 Request Timeout**：服务器等待了太长时间。
**409 Conflict**：多个请求发生了冲突。
**413 Request Entity Too Large**：请求体的数据过大。
**414 Request-URI Too Long**：请求行里的 URI 太大。
**429 Too Many Request**：客户端发送的请求过多。
**431 Request Header Fields Too Large**：请求头的字段内容太大。
#### 5xx
**500 Internal Server Error**：仅仅告诉你服务器出错了，出了啥错咱也不知道。
**501 Not Implemented**:表示客户端请求的功能还不支持。
**502 Bad Gateway**：服务器自身是正常的，但访问的时候出错了，啥错误咱也不知道。
**503 Service Unavailable**： 表示服务器当前很忙，暂时无法响应服务。


