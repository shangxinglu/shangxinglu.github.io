# Header

## 验证

    WWW-Authenticate: <type>  realm=<realm> charset=<charset>
        作用
            响应首部，定义了使用何种验证方式去获取对资源的链接
            通常会一起返回401状态

        指令
            type
                认证类型，默认一个通用类型Basic

            
            realm
                一个保护区域的描述，未指定通常显示一个格式化的主机名替代
            
            charset
                告知客户端服务器首选的编码方案

    Authorization:<type> <credentials>
        作用
            请求首部，表示服务器用于验证用户代理身份的凭证

        指令
            type
                验证类型，常用的是基本验证Basic
            
            credentials
                身份凭证
                如果是Basic，用冒号`:`将用户名和密码进行拼接,然后用base64进行编码

    
    Proxy-Authenticate:<type>  realm=<realm> charset=<charset>
        作用
            响应首部，指定了获取代理服务器上资源访问权限而采用的身份验证方式，
            一般和407状态一起响应

        指令
            type
                认证类型，默认一个通用类型Basic

            realm
                一个保护区域的描述，未指定通常显示一个格式化的主机名替代


    Proxy-Authorization: <type> <credentials>
        作用
            请求首部，包含代理服务器用于身份验证的凭证

        指令
            type
                身份验证类型
            
            credentials
                凭证
        

## 缓存

    Age: <delta-seconds>
        作用
            响应首部，表示对象在缓存代理中存储的时长，单位秒
        
        指令
            delta-seconds
                非负整数，秒数


    Cache-Control: max-age=<seconds>
    Cache-Control: max-stale[=<seconds>]
    Cache-Control: min-fresh=<seconds>
    Cache-Control: no-cache
    Cache-Control: no-store
    Cache-Control: no-transform
    Cache-Control: only-if-cached
        作用
            通用首部，用来实现缓存机制，缓存指令是单向的，不一定包含在响应中
            这是最重要的规则，用来控制请求/响应链中的缓存机制
        
        缓存请求指令

            Cache-Control: max-age=<seconds>
            Cache-Control: max-stale[=<seconds>]
            Cache-Control: min-fresh=<seconds>
            Cache-Control: no-cache
            Cache-Control: no-store
            Cache-Control: no-transform
            Cache-Control: only-if-cached

        缓存响应指令

            Cache-Control: must-revalidate
            Cache-Control: no-cache
            Cache-Control: no-store
            Cache-Control: no-transform
            Cache-Control: public
            Cache-Control: private
            Cache-Control: proxy-revalidate
            Cache-Control: max-age=<sconds>
            Cache-Control: s-maxage=<sconds>

        可缓存性指令

            作用
                定时是否可以缓存响应/请求、可以缓存的位置
                以及缓存之前是否需要与源务器进行验证的指令
    
            指令
                public
                    响应可以存储在任何缓存中，即使响应通常是不可缓存的
                
                private
                    响应只能由浏览器的缓存存储，即使响应通常是不可缓存的
                
                no-cache
                    响应可以存储在任何缓存中，即使响应通常是不可缓存的，
                    但是存储缓存之前必须要通过原始服务器的验证
                
                no-store
                    响应不会存储在任何缓存中

        有效期相关指令
   
            max-age=<seconds>
                资源被视为新鲜的最长时间
                缓存必须由服务器开启，请求中可以使用max-age=0避免走缓存
            
            s-maxage=<seconds>
                覆盖max-age或Expires首部字段，仅适用于共享缓存，被私有缓存忽略
            
            max-stale[=<seconds>]
                表示客户端愿意接受一个已经过期的资源
                可以设置一个可选的秒数，表示响应过期时间的上限值
            
            min-fresh=<seconds>
                表示客户端希望获取一个在指定秒数内仍然是新鲜的响应

        重新验证与重新加载指令

            must-revalidate
                一旦资源过期，在成功向原始服务器验证前，不能使用缓存的响应
            
            proxy-revalidate
                与must-revalidate相同作用，但是只适用于共享缓存，被私有缓存忽略

        其他指令

            no-transform
                不得对资源进行转换，
                就是Content-Type、Content-Encoding等首部信息不能被代理修改

            only-if-cached
                表示客户端只接受已缓存的响应，并且不会向原始服务器检查是否有更新


    
    Clear-Site-Data: "cache"
    Clear-Site-Data: "cache","cookies"
    Clear-Site-Data: "*"

        作用
            响应首部，表示清除当前请求网站有关的浏览器数据(cookie,存储,缓存)
        
        指令
            "cache"
                表示希望删除响应URL来源的本地缓存数据
            
            "cookies"
                表示希望删除响应URL来源的所有cookies
            
            "storage"
                表示希望删除响应URL来源的所有DOM存储
            
            "executionContexts"
                表示希望响应URL来源的上下文重新加载
            
            "*"
                表示希望删除响应URL来源的所有类型数据

    Expires: <http-date>
        作用
            响应首部，表示过期时间，
            无效日期，代表资源已经过期
        
        指令
            http-date
                一个HTTP-date时间戳
    
    Pragma: no-cache
        作用
            通用首部，用来兼容只支持HTTP/1.0的服务器
        
        指令
            no-cache
                与Cache-Control:no-cache效果一致
            


