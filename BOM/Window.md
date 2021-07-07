# BOM

## Window

    window对象表示一个包含DOM文档的窗口

    每个标签页都有自己的window

### 属性

    closed 只读
        表示当前窗口是否关闭
    

    console 只读
        获取console对象的引用
    
    customElements 只读
        获取一个CustomElementRegistry对象的引用
        
    crypto 只读
        获取Crypto对象

    devicePixelRatio 只读
        获取当前显示设备的物理像素与CSS像素分辨率之比

    frameElement 只读
        获取当前窗口被<iframe>或<object>嵌入时的<iframe>或<object>元素,
        如果当前窗口为顶层或不同源返回null

    frames 只读
        获取当前窗口所有的直接子窗口的类数组对象
    
    history 只读
        获取对history对象的引用

    innerHeight 只读
        浏览器窗口的内部高度，包含水平滚动条高度

    innerWidth 只读
        浏览器窗口的内部宽度，包含垂直滚动条宽度
    
    length 只读
        窗口中包含的框架数量(frame和iframe)

    location
        获取location对象
    
    locationbar 只读
        获取一个可以用来检查地址栏的visible属性的对象

    localStorage 只读
        获取当前访问源一个只读的Storage对象

    menubar 只读
        获取可以用来检查菜单栏的visible属性的对象

    name
        获取或设置窗口名称
    
    navigator 只读
        获取navigator对象的引用

    opener
        获取打开当前串口的那个窗口引用

    outerHeight 只读
        整个浏览器的高度

    outerWidth 只读
        整个浏览器的的宽度

    pageXOffset 只读
        scrollX别名

    pageYOffset 只读
        scrollY别名

    scrollX 只读
        页面水平方向滚动的像素值

    scrollY 只读
        页面垂直方向滚动的像素值
    
    parent 只读
        获取当前窗口的父窗口对象，如果没有父窗口返回自身
    

### 方法
    open(strUrl,strWindName[,strWindowFeatures])
        作用
            打开一个指定名称和资源的窗口
        
        参数
            strUrl
                新窗口需要载入的url地址
            
            strWindowName 
                新窗口的名字，字符串中不能含有空白字符
            
            strWindowFeatures 可选
                一个字符串值，配置将要打开的窗口的一些特性

        返回
            打开新窗口的引用
    

    postMessage(message,targetOrigin[,transfer])
        作用
            可以安全的实现跨源通信

        参数
            message
                将要发送到其他window的数据，将会被安全的序列化
            
            targetOrigin
                指定要接收的窗口的origin属性，只有协议、主机地址和端口都匹配才会发送
            
            transfer 可选
                是遗传和message同时传递的Transferable对象
    

    print()
        作用
            打开打印对话框打印当前文档


    prompt(text,value)
        作用
            显示一个对话框，对话框包含一条文字信息，用来提示用户输入文字

        参数
            text
                提示文字
            value
                默认值

        返回
            用户输入的文字或null


    resizeBy(xDelta,yDelta)
        作用
            调整窗口大小
        
        参数
            xDelta
                水平方向变化的像素值
            
            yDelta
                垂直方向变化的像素值
        
        注意
            只能修改window.open()打开的窗口同时窗口只能有一个tab
        

    resizeTo(aWidth,aHeight)
        作用
            动态调整窗口的大小
        
        参数
            aWidth
                新的outerWidth宽度

            aHeight
                新的outerHeight盖度
            
        注意
            只能修改window.open()打开的窗口同时窗口只能有一个tab

    
    scroll(xCoord,yCoord)
    scroll(options)
        作用
            滚动窗口至文档中的特定位置
        
        参数
            xCoord
                想要置于左上角的像素点的x坐标
            
            yCoord
                想要置于左上角像素点的y坐标
            
            options
                一个ScrollToOptions字典
                
                top
                    指定window或元素的Y轴方向滚动的像素
                
                left
                    指定window或元素的X轴方向滚动的像素
                
                behavior
                    指定滚动是否应该平滑进行还是立即跳到指定位置
                        auto 跳跃

                        smooth  滚动
                        

    scrollBy(xCoord,yCoord)
    scrollBy(options)
        作用
            在窗口中指定偏移量滚动文档
        
        参数
            xCoord
                水平偏移量
            
            yCoord
                垂直偏移量
            
            options
                一个ScrollToOptions字典
    

    scrollTo(xCoord,yCoord)
    scrollTo(options)
        作用
            滚动到文档中的某个坐标
        
        参数
            xCoord
                文档中的x坐标
            
            yCoord
               文档中的y坐标
            
            options
                一个ScrollToOptions字典
    
            
    stop()
        作用
            相当于点击了浏览器的停止按钮，
            但是不能阻止已经包含在加载中的文档
 