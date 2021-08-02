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
            


## 条件

    Last-Modified: <day-name>,<day> <month> <year> <hour>:<minute>:<second> GMT
        作用
            响应首部，包含源服务器认为资源上次修改的日期和时间
            用来确定接收或存储的资源是否相同，不如ETag准确
        
        指令
            <day-name>
                表示周几
                Mon,Tue,Wed,Thu,Fri,Sat,Sun之一,区分大小写
            
            <day>
                表示天数，两位数字
            
            <month>
                表示月份
                Jan,Feb,Mar,Apr,May,Jun,Jul,Aug,Sep,Oct,Nov,Dec之一
            
            <year>
                表示年份，4位数字
            
            <hour>
                表示小时数，2位数字
            
            <minute>
                表示分钟数，2位数字
            
            <second>
                表示秒数，2位数字
            
            GMT 
                国际标准时间，HTTP中的时间均用国际标准时间
            

    ETag: W/"<etag_value>"
    ETag: "<etag_value>"
        作用
            响应首部，表示资源的特定版本的标识符，类似于指纹
        
        指令
            W\
                weak,使用弱验证器，只需比较两个ETag的语义是否相等
                不使用就是强验证器，两个ETag需要完全相等
            
            <etag_value>
                ETag的值
            
    If-Match: "<etag_value>"
    If-Match: "<etag_value>","<etag_value>"...
    If-Match: *
        作用
            请求首部，表示这是一个条件请求，表示请求资源满足此首部列出的ETag值时才返回资源
            If-Match中
        
        指令
            etag_value
                资源的实体标签，前面可以加上W\前缀表示使用弱比较算法
            
            *
                可以是任何资源
            
    
    If-Match: "<etag_value>"
    If-None-Match: "<etag_value>","<etag_value>"...
    If-None-Match: *
        作用
            请求首部，仅当服务器上没有任何资源的ETag属性值与它相匹配配才返回，
            采用弱比较，当与If-Modified-Since结合使用的时候，If-None-Match优先

        指令
            etag_value
                资源的实体标签
            
            *
                表示任何资源，仅在上传时有用
            
    
    If-Modified-Since: <day-name>,<day> <month> <year> <hour>:<minute>:<second> GMT
        作用
            请求首部，只有当给定日期之后被修改才返回
            只能和GET和HEAD使用
        
    
    If-Unmodified-Since: <day-name>,<day> <month> <year> <hour>:<minute>:<second> GMT
        作用
            请求首部，只有当未在给定时间后修改，才返回，不然那将返回412状态码
            接收POST和其他不安全的请求


    Vary: *
    Vary: <header-name>,<header-name>...
        作用
            响应首部，用来确定如何匹配未来的请求首部，以决定是否可以使用缓存的响应，
            而不是从源服务器请求新的响应
            服务器使用它指示在内容协商算法中选择资源的表示时，它使用了哪些标头
        
        指令
            *
                每个请求都应该被视为唯一且不可缓存
            
            header-name
                首部名称列表，以逗号隔开
            
    
## 连接管理

    Connection: keep-alive
    Connection: close
        作用
            通用首部，控制当前事务完成后网络连接是否保持打开状态
            注意Connection在HTTP/2.0中禁用

        指令
            close
                表示关闭，HTTP/1.0默认值
            
            keep-alive
                表示希望保持连接打开，HTTP/1.1的默认设置

    
    Keep-Alive: parameters
        作用
            通用首部，允许发送者提示连接的状态，还可以设置超时时长和最大请求数
            需要将Connection设置为keep-alive才有意义

        指令
            parameters
                一系列逗号隔开的参数，每一个参数由一个标识符和一个值构成，并用等号连接

                timeout
                    指定空闲连接需要保持打开状态的最小时长
                
                max
                    连接关闭之前，可以发送请求数的最大值


## 内容协商

    Accept: <MIME_type>/<MIME_subtype>
    Accept: <MIME_type>/*
    Accept: */*

        作用
            请求首部，告诉服务器客户端能够理解的内容类型
            服务器通过Content-Type响应首部告知客户端它的选择
        
        指令
            <MIME_type>/<MIME_subtype>
                单一精确的MIME类型
            
            <MIME_type>/*
                一类MIME类型，但是没有指明子类
            
            */*
                任意类型的MIME类型
            
            ;q=权重
                0~1，值越大优先级越高，默认1
                当权重相同，类型越具体优先级越高
            
        示例
            text/html;q=0.2,image/png;q=0.1


    Accept-Encoding: gzip
    Accept-Encoding: compress
    Accept-Encoding: deflate
    Accept-Encoding: br
    Accept-Encoding: identity
    Accept-Encoding: *

        作用
            请求首部，告诉服务器客户端能够理解的内容编码，通常是压缩算法
            服务器通过Content-Encoding响应首部告知客户端它的选择
        
        指令
            identity
                表示未经过压缩和修改
            
            *
                匹配其他任意未在请求首部列出的编码方式，这是默认值
                仅表示算法之间无优先级
            
            ;q=权重
                0~1，值越大优先级越高，默认1
                当权重相同，类型越具体优先级越高

    Accept-Language: <language>
    Accept-Language: *
        作用
            请求首部，告诉服务器客户端能够理解的自然语言
            服务器通过Content-Language响应首部告知客户端它的选择

        指令
            <language>
                含有2~3个字符的字符串表示的语言码或完整的语言标签

            *
                任意语言
            
            ;q=权重
                0~1，值越大优先级越高，默认1
                当权重相同，类型越具体优先级越高

## Cookies

    Cookie: <cookie-list>
        作用
            请求首部，包含存储的或与服务器关联的HTTP cookie
        
        指令
            <cookie-listt>
                <cookie-name>=<cookie-value>形式的名称-值对列表，由分号和空格分隔

    
    Set-Cookie
        作用
            响应首部，用于从服务器向代理用户发送cookie

## 下载

    Content-Disposition
        语法
            作为响应首部
                Content-Disposition: inline
                Content-Disposition: attachment
                Content-Disposition: attachment; filename="file.jpg"
            
            作为多部分主体的首部
                Content-Disposition: form-data; name="fieldName"
                Content-Disposition: form-data; name="fieldName"; filename="file.jpg"
        作用
            在常规的HTTP响应中，Content-Disposition响应首部用来指示内容是否应在浏览器中内嵌显示，
            即作为网页的一部分，还是作为附件，下载并保存到本地
           
           在multipart/form-data主体中，Content-Disposition是通用首部，
           必须在multipart主体的每个子部分上使用，以提供有关它适用的字段信息

        指令
            name
                字段名称
            
            filename
                传输文件原始名称的字符串
                当与attachment结合使用是，它被用作最终给用户的"另存为"对话框的默认文件名
            
            filename*
                与filename的不同之处仅在于filename*使用RFC 5987中的编码
                当同时出现时，filename*优先于filename
    


## 消息体信息

    Content-Length: <length>
        作用    
            实体首部，指名发送给接收方的消息主体的大小

        指令
            <length>
                消息长度，用十进制数字表示字节的数目
    

    Content-Type: text/html; charset=utf-8
    Content-Type: multipart/form-data;boundary=something
        作用
            实体首部，指示资源的MIME类型
        
        指令
            media-type
                资源或数据的MIME类型
            
            charset
                字符编码
            
            boundary
                对于多部分实体，boundary是必需的，用于多部分内容的边界
                边界前面有两个-，最终的边界在末尾还有两个-

    Content-Encoding: gzip
    Content-Encoding: compress
    Content-Encoding: deflate
    Content-Encoding: identity
    Content-Encoding: br

        作用
            实体首部，表示对消息主体进行了哪种方法的编码转换
        
        指令
            identity
                表示未经过压缩和修改
        
    
    Content-Language: <language-tag>
        作用
            实体首部，表示面向用户的语言

        指令
            <language-tag>
                多个语言标签需要用逗号隔开，子语言标签用-隔开
    

    Content-Location: <url>
        作用
            实体首部，指示返回数据的备用位置
        
        指令
            <url>
                相对地址或绝对地址


## 代理

    Forwarded: by=<identifier>; for=<identifier>;host=<host>;prop=<http|https>
        作用
            请求首部，包含代理服务器的信息，有几个已经成为标准的首部可以用来替代它，
            X-Forwared-For、X-Forwarded-Host、X-Forwarded-Proto
        
        指令
            <identifier>
                显示使用代理过程中被修改或丢失的信息
            
            by=<identifier>
                请求进入代理服务器的接口
            
            for=<identifier>
                发起请求的客户端和代理链中的后续代理
            
            host=<host>
                代理收到的Host首部信息
            
            proto=<http|https>
                发出请求的协议


    X-Forwarded-For: <client>, <proxy1>, <proxy2>
        作用
            请求首部，用于识别通过HTTP代理或负载均衡连接到Web服务器的客户端的原始IP地址
        
        指令
            <client>
                客户端IP地址
            
            <proxy1>,<proxy2>
                如果经过多个代理，会依次列出每个代理的IP

        
    X-Forwarded-Host: <host>
        作用
            请求首部，用于在表示客户端请求的原始主机
        
        指令
            <host>
                服务器主机
    

    X-Forwarded-Proto: <protocol>
        作用
            请求首部，包含客户端连接到代理时的协议
        
        指令
            <protocol>
                协议
    

    Via: [<protocol-name> "/"] <protocol-version> <host> [ ":" <port> ]
    Via: [<protocol-name> "/"] <protocol-version> <pseudonym>
        作用
            通用首部，由代理服务器添加，用来追踪消息转发情况，防止循环请求，
            以及识别传递链中对协议的支持能力

        指令
            <protocol-name>
                协议名称
            
            <protocol-version>
                协议版本号
            
            <host> and <port>
                公共代理的URL和端口号
            
            <pseudonym>
                内部代理的名称或别名


## 重定向

    Location: <url>
        作用
            响应首部，指定页面重新定向的地址，一般在3XX响应中才有用
        
        指令
            <url>
                相对地址或绝对地址


## 请求上下文

    From: <email>
        作用
            请求首部，包含请求代理用户的电子邮件地址
        
        指令
            <email>
                邮件地址
    
    Host: <host>:<port>
        作用
            请求首部，请求发送的目标服务器的主机和端口号
            Host必须在HTTP/1.1请求中发送

        指令
            <host>
                主机
            
            <port>
                端口号

    
    Referer: <url>
        作用
            请求首部，包含发出请求页面的绝对路径或相对路径
            有两种情况，Referer不会被发送
                1. 来源页面使用file或data的协议
                2. 请求的是非安全协议，而来源页面是安全协议

        指令
            <url>
                发出请求页面的绝对路径或相对路径


    Referrer-Policy: no-referrer
    Referrer-Policy: no-referrer-when-downgrade
    Referrer-Policy: origin
    Referrer-Policy: origin-when-cross-origin
    Referrer-Policy: same-origin
    Referrer-Policy: strict-origin
    Referrer-Policy: strict-origin-when-cross-origin
    Referrer-Policy: unsafe-url

        作用
            响应首部，控制请求首部Referer的内容，防止包含一些敏感信息的URL被发送给第三方站点
        
        指令
            no-referrer
                Referer会被完全省略
            
            no-referrer-when-downgrade
                当协议安全级别从较高发到较低的目的地(HTTPS->HTTP)，Referer会被忽略

            origin
                只发送origin作为引用地址

            origin-when-cross-origin
                对于以同源请求发送完整的URL，但是对于对于非同源请求仅发送origin
            
            same-origin
                只对同源请求发送引用地址
            
            strict-origin
                同等级安全级别发送应用地址，降级情况不发送
            
            strict-origin-when-cross-origin
                对于同源请求发送完整URL，同等级安全级别发送origin，降级情况不发送
            
            unsafe-url
                一直发送完整URL
        
        在HTML内置
            <meta name="referrer" content="origin">
        
            <a>,<area>,<img>,<iframe>,<script>,<link>元素上通过referrerpolicy属性设置
    
    User-Agent: <product> / <product-version> <comment>
        
        作用
            请求首部，描述用户代理的应用程序、操作系统、供应商、版本的字符串
                
        指令
            <product>
                产品标识符
            
            <product>
                产品版本号
            
            <comment>
                零个或多个说明


## 响应上下文

    Allow: <http-methods>
        作用
            响应首部，列出资源支持的方法
        
        指令
            <http-methods>
                请求方法，以逗号分隔
    

    Server: <product>

        作用
            响应首部，描述源服务器使用的软件
        
        指令
            <product>
                产品名称


## 范围请求

    Accept-Ranges: <range-unit>
    Accept-Ranges: none

        作用
            响应首部，标识自身支持范围的请求，字段的具体指用于定义范围的单位
            存在Accept-Range标头的情况下，浏览器可能会尝试恢复中断的下载，而不是从头开始
        
        指令
            <range-unit>
                定义服务器支持的范围单位
            
            none
                不支持范围单位，标志着首部失效，很少使用
            

    Range: <unit>=<range-start>-
    Range: <unit>=<range-start>-<range-end> [,<range-start>-<range-end>*]
        作用
            请求首部，指示服务器应返回文件的哪一部分，可以一次请求多个部分，
            服务器已multipart文件的形式返回，如果服务器返回的是范围响应，
            需要使用206状态码，如果请求范围不合法，返回416状态码

        指令
            <unit>
                范围所采用的单位，通常是字节
            
            <range-start>
                一个整数，特定单位下，范围的起始值
            
            <range-end>
                一个整数，特定单位下，范围的结束值，如果不存在，范围一直延伸到文档结束
        

    If-Range:<day-name>,<day> <month> <year> <hour>:<minute>:<second> GMT
    If-Range: <etag>
        作用
            请求首部，是Range在指定条件下起作用，可以用Last-Modified事件值用作验证，
            也可以使用ETag，但是不能同时使用，一般用来确保断点续传的下载过程中，
            确保下载的资源没有发生改变

    
    Content-Range: <unit> <range-start>-<range-end>/<size>
    Content-Range: <unit> <range-start>-<range-end>/*
    Content-Range: <unit> */<size>
        作用
            响应首部，显示数据片段在整个文件中的位置
        
        指令
            <unit>
                数据区间采用的单位，通常是字节
            
            <range-start>
                一个整数，表示给定单位下，区间的起始值
            
            <range-end>
                一个整数，表示给定单位下，区间的技术支持值
            
            <size>
                整个文件的大小，如果大小未知，使用`*`


## 获取元数据请求首部

    Sec-Fetch-Site: cross-site
    Sec-Fetch-Site: same-origin
    Sec-Fetch-Site: same-site
    Sec-Fetch-Site: none

        作用
            请求首部，指示了请求发起者的来源和请求资源的来源之间的关系
        
        指令
            cross-site
                发起者和服务器是不同站点
            
            same-origin
                发起者和服务器是相同的origin
            
            same-site
                发起者和服务器具有相同的协议，域名/子域名，
                但是不一定具有相同端口号
            
            none
                请求是用户操作发起的

    
    Sec-Fetch-Mode: cors
    Sec-Fetch-Mode: navigate
    Sec-Fetch-Mode: no-cors
    Sec-Fetch-Mode: same-origin
    Sec-Fetch-Mode: websocket

        作用
            请求首部，指示请求的模式
        
        指令
            cors
                这是一个跨域请求
            
            navigate
                请求由HTML文档之间发起
            
            same-origin
                请求与所请求的资源来自同一origin
            
            websocket
                正在请求建立WebSocket连接

    
    Sec-Fetch-User: ?1
        作用
            请求首部，表示用户发起的请求，值始终是?1,
            请求如果不是由用户激活触发，规范要求完全省略标头

    
    Sec-Fetch-Dest: audio
    Sec-Fetch-Dest: audioworklet
    Sec-Fetch-Dest: document
    Sec-Fetch-Dest: embed
    Sec-Fetch-Dest: empty
    Sec-Fetch-Dest: font
    Sec-Fetch-Dest: frame
    Sec-Fetch-Dest: iframe
    Sec-Fetch-Dest: image
    Sec-Fetch-Dest: manifest
    Sec-Fetch-Dest: object
    Sec-Fetch-Dest: paintworklet
    Sec-Fetch-Dest: report
    Sec-Fetch-Dest: script
    Sec-Fetch-Dest: serviceworker
    Sec-Fetch-Dest: sharedworker
    Sec-Fetch-Dest: style
    Sec-Fetch-Dest: track
    Sec-Fetch-Dest: video
    Sec-Fetch-Dest: worker
    Sec-Fetch-Dest: xslt
        作用
            请求首部，指示请求的数据，将如何使用
        
        指令
            audio
                音频数据，可能源自HTML的<audio>标签
            
            audioworklet
                音频worklet使用的数据，可能源于调用audioWorklet.addModult()
            
            document
                文档数据，用户启动顶级导航的结果
            
            embed
                嵌入的内容，这可能源自<embed>标签
            
            empty
                空字符串，用于没有自己价值的目的地
                例如fetch()、XMLHttpRequest
            
            font
                字体，可能源自CSS @font-face
            
            frame
                一个frame，可能源于HTML<frame> 标签
            
            iframe
                一个iframe，可能源于HTML<iframe>标签

            image
                图像，可能源自HTML<image>，CSS的background-image、
                CSS cursor等
            
            manifest
                清单，可能源自HTML的<link rel=mainifest>

            object
                对象，源自HTML<object>标签

            paintworklet
                绘制Worklet，可能源自对CSS.PaintWorklet.addModule()的调用

            report
                报告，例如内容安全策略报告

            script
                一个脚本，可能源自HTML<script>标签，
                或者WorkerGlobalScope.importScripts()的调用
            
            serviceworker
                服务工作者，可能源自对navigator.serviceWorker.register()的调用
            
            sharedworker
                共享工作者，可能源自SharedWorker

            style
                样式，可能源自HTML<link rel=stylesheet>或CSS @import
            
            track
                HTML文本轨道，可能源自HTML<track>标签
            
            video
                视频数据，可能源自HTML<video>标签
            
            worker
                工作者
            
            xslt
                SLST的变化


## 传输编码

    Transfer-Encoding: chunked
    Transfer-Encoding: compress
    Transfer-Encoding: deflate
    Transfer-Encoding: gzip
    Transfer-Encoding: identity
        作用
            实体首部，指名了将实体安全传递给用户所采用的编码格式，
            Transfer-Encoding是一个逐跳传输消息首部
        
        指令
            chunked
                数据以一系列分块的形式发送
            
            compress
                采用`LZW`压缩算法
            
            deflate
                采用`zlib`结构和`deflate`压缩算法
            
            gzip
                表示采用`LZ77`算法
            
            identity
                表示未经过压缩和修改
            
    
    TE: compress
    TE: deflate
    TE: gzip
    TE: trailers
    TE: gzip,deflate;q=0.5

        作用
            请求首部，用来指定用户代理希望使用的传输编码类型
            有个非正式名字Accept-Transfer-Encoding
        
        指令
            trailers
                允许挂载字段


    Trailer: heander-names
        作用
            实体首部，允许发送方在分块发送的消息后面添加额外的元信息，
            例如数据签名等
            需要设置TE为trailers

        指令
            消息首部，以下首部字段不允许出现
                用于信息分帧的首部(例如Transfer-Encoding,Content-Length)
                用于路由用途的首部(例如Host)
                请求修饰首部(例如控制类和条件类的)
                身份验证首部(例如Authorization,Set-Cookie)
                Content-Encoding,Content-Type,Content-Range,Trailer

## 其他

    Date: <day-name>,<day> <month> <year> <hour>:<minute>:<second> GMT
        作用
            通用首部，消息产生的日期和时间

    Link: <uri-reference>; param1=value1; param2=value2
        作用
            实体首部，提供了序列化HTTP头部链接的方法
        
        指令
            <uri-reference>
                必须包含在`<``>`中间
            
            param
                等价于<link>元素的attribute和value


    Retry-After: <date>
    Retry-After: <delay-seconds>
        作用
            响应首部，表示用户代理需要等待多长时间之后才能继续发送请求

        指令
            <data>
                日期

            <delay-seconds>
                秒数


    SourceMap: <url>
    X-SourceMap: <url> <deprecated>
        作用
            响应首部，将生成的代码链接到一个source map文件，
            使浏览器可以重建原始的资源，然后显示在调试器里
        
        指令
            <url>
                一个指向source map文件的一个相对或绝对URL
            

    Connection: upgrade    
    Upgrade: protocol_name[/version]
        作用
            通用首部，仅限HTTP/1.1使用，用于将已建立的连接升级到不同协议
        
        指令
            protocol_name
                协议名，多个以逗号隔开，优先级降序排列

    
    X-DNS-Prefetch-Control: on
    X-DNS-Prefetch-Control: off
        作用
            响应首部，控制浏览器DNS预读取功能，
            DNS预读取是一项使浏览器主动去执行域名解析的功能
        
        指令
            on
                启用DNS预解析
            
            off
                关闭DNS预解析
            
        HTML
            可以使用HTML的meta标签设置
            <meta http-equiv="x-dns-prefetch-control" content="on">
    