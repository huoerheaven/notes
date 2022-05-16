#### CORS跨域请求限制
> 跨域资源共享或者说 CORS 是一种使用额外的 HTTP 头部来允许浏览器可以在一个不同域的网站内获取另一个域下的服务器资源的机制。    
[参考]（https://zhuanlan.zhihu.com/p/57080609）
1. 什么是预请求
> 在 CORS 中，可以使用 OPTIONS 方法发起一个预检请求，以检测实际请求是否可以被服务器所接受。
预检请求报文中的 Access-Control-Request-Method 首部字段告知服务器实际请求所使用的 HTTP 方法；
Access-Control-Request-Headers 首部字段告知服务器实际请求所携带的自定义首部字段。
服务器基于从预检请求获得的信息来判断，是否接受接下来的实际请求。
2. CORS预请求
    - 跨域时 浏览器允许的方法
        - GET 
        - POST
        - HEAD
        > 使用上述三种方法发起请求时，浏览器不会发起预请求    
        > 使用除了这三种方法之外的方法比如：PUT,DELETE,浏览器会发起一个预请求，如果服务器没有设置允许这两种方法访问，则会造成跨域。
        > 服务器可以设置` "Access-Control-Allow-Methods": "PUT,DELETE"`来解决浏览器跨域问题
    - 浏览器允许Content-Type
        - text/plain
        - multipart/form-data
        - application/x-www-form-urlencodeded
    - 请求头限制
        - Accept
        - Accept-Language
        - Content-Language
        - Content-Type
        - Last-Event-ID
        > 如果用户自定义请求头，浏览器会发起一个预请求，如果服务器没有设置允许自定义请求头访问，则会造成跨域。
        > 服务器可以设置` "Access-Control-Allow-Headers": "此处为自定义请求头的名字"`来解决浏览器跨域问题
    
    
 

        
