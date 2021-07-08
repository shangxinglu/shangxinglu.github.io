# Location

## 作用
    location接口表示其链接到的对象的位置

## 属性

    href
        完整的URL，允许href的更新
    
    protocol
        URL的协议包含':'
    
    host
        URL的主机名，如果有端口号，会包含端口号

    hostname
        URL的主机名，不包含端口号

    port
        URL端口号

    pathname
        首字母为'/'的URL的路径

    search
        URL标识符中'?'以及跟随其后的一串URL查询参数

    hash
        包含URL标识符中的'#'和后面片段标识符

    origin 只读
        页面来源的域名标准形式
    

## 方法

    assign(url)
        作用
            触发窗口加载并显示指定URL内容
        
        参数
            url 
                需要跳转的URL


    reload([forceReload])
        作用
            刷新当前页面

        参数
            forceReload
                布尔值，强制重新加载，不从缓存读取


    replace(url)
        作用
            以给定的URL替换当前的资源

        参数
            url 
                需要导航到的URL

    toString()
        作用
            获取整个URL的字符串