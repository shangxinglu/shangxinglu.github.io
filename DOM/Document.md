# Document

## 描述
    Document接口表示任何在浏览器中载入的网页，并作为网页内容的入口



## 属性

    body
        获取文档中的<body>元素或者<frameset>元素

    
    characterSet 只读
        获取当前文档的字符编码

    compatMode 只读
        文档的呈现模式是quirks怪异模式或strict严格模式

        值
            BackCompat
                怪异模式
            
            CSS1Compat
                标准或准标准模式
            
    contentType 只读
        文档的Content-Type类型

    doctype 只读
        文档关联的文档类型定义
    
    documentElement 只读
        文档对象的根元素，HTML文档是html元素


    documentURI 只读
        文档的位置(location)

    fonts
        获取文档的FontFaceSet接口，
        FontFaceSet接口对加载新字体、检查已加载字体的加载状态非常有用

    forms 只读
        文档中from元素的集合
    
    head 只读
        文档中的head元素，如果有多个，返回第一个

    hidden 只读
        表示页面是否隐藏

    images 只读
        当前文档中所有image元素的集合

    links 只读
        所有超链接的列表

    scripts 只读
        所有script元素的集合

    scrollingElement 只读
        滚动文档的Element对象引用，与documentElement属性相同

    visibilityState 只读
        文档的可见性，有以下值
            visible
                页面至少部分可见
            
            hidden
                页面对用户不可见
            
            prerender
                页面正在渲染中



    cookie
        获取和设置文档相关联的cookie

    defaultView 只读
        与当前文档关联的window对象

    designMode
        获取或设置让用户编辑这个文档的能力
            off
                关闭
            
            on
                开启

    dir 
        获取或设置文档的文字方向
            ltr
                从左向右
            
            rtl
                从右向左

    domain
        获取或设置文档的域名，一般用于同源策略
        只能设置为当前域名的一部分，比如将二级设置成一级

    lastModified 只读
        文档的最后修改日期和时间
    
    location 只读
        获取location对象

    readyState 只读
        文档的加载状态，状态的改变会触发readystatechange事件

    referrer 只读
        一个URI，表示当前页面是从该URI打开或跳转

    title
        获取或设置文档标题

    URL 只读
        文档的URL地址

    activeElement
        当前聚焦状态的Element
    
    fullScreenElement
        以全屏模式显示的Element节点