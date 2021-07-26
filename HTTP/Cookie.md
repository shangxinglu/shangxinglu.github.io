# Cookie

## 作用
    Cookie可以存储客户端的数据，一般用于存储状态数据和个性化数据，
    现代浏览器有storage和indexedDB，可以用来存储数据，
    Cookie每次请求都会被携带，会带来额外的性能开销，
    所以Cookie开始被逐渐淘汰

## 创建
    服务器可以在响应头里设置Set-Cookie响应头，浏览器接收后一般会保存下Cookie，
    之后对服务器的每次请求都会Cookie请求头将Cookie发送给服务器

    Cookie可以指定过期时间、域、路径、有效期、适用站点

## 生命周期
    通过过期时间Expires或有效时间Max-age决定，如果同时设置，忽视Expires

## 限制访问
    有两个属性用来限制访问Cookie，确保Cookie的安全，Secure和HttpOnly

        Secure
            设置了Secure，那么Cookie只有通过HTTPS协议加密过的请求发送给服务端
        
        HttpOnly
            设置HttpOnly的Cookie，不会被document.cookie访问到


## 作用域
    Domain和Path标识定义了Cookie的作用域
    就是允许Cookie发送给哪些URL

    Domain
        指定了哪些主机可以接受Cookie，默认origin，不包含子域名，
        如果指定了Domain，则一般包含子域名
    
    Path
        指定了主机下哪些路径可以接受Cookie，以`/`作为路径分隔符，子路径会被匹配

## SameSite
    SameSite属性可以让服务器控制在跨站请求时浏览器不发送Cookie
    有三个值

        None
            浏览器在同站请求、跨站请求下继续发送cookie，不区分大小写
        
        Strict
            浏览器只在访问相同站点时发送cookie，同时要满足Cookie的作用域
        
        Lax
            与Strict类似，但是用户从外部站点导航至URL时除外，这是现代浏览器的默认选项

