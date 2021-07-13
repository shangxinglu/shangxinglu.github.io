# Element

## 作用
    Element是一个通用性非常强的基类，所有Document对象下的对象都继承自它

## 属性
    
    attribute 只读
        元素所有属性节点的一个是实时集合，集合是一个NameNodeMap对象，
        不是数组，所以不具备数组的方法
    
    classList 只读
        元素的class属性的实时DOMTokenList集合
    
    className 只读
        获取或设置指定元素的class属性

    clientHeight 只读
        元素的内部高度，包含内边距，但不包括水平滚动条、边框和外边距
        对于没有定义CSS或内联布局盒子为0

    clientWidth 只读
        元素的内部宽度，包含内边距，但不包括垂直滚动条、边框和外边距
         对于没有定义CSS或内联布局盒子为0
    
    clientLeft 只读
        表示一个元素的左边框的宽度，如果有垂直滚动条包含滚动条宽度
    
    clientTop 只读
        元素顶部边框的宽度

    id
        元素的id
    
    namespaceURI 只读
        元素的命名空间
    
    nextElementSibling 只读
        元素的下一个兄弟节点
    
    previousElementSibing 只读
        元素的上一个兄弟节点


    innerHTML
        设置或获取元素的后代

    outerHTML
        设置或获取元素及其后代


    prefix 只读
        元素的命名空间前缀

    scrollWidth 只读
        元素的内容宽度度，包括由于溢出导致的视图中不可见的内容

    scrollHeight 只读
        元素的内容高度，包括由于溢出导致的视图中不可见的内容

    scrollLeft
        读取或设置元素滚动条到元素左边的距离
    
    scrollTop
        读取或设置元素滚动条到元素上边的距离
    

## 方法

### 事件监听器

    addEventListener(type,listener[,options])
    addEventListener(type,listener,useCapture)

        作用
            将指定的监听器注册到EventTarget上

        参数
            type
                监听事件类型的字符串
            
            listener
                监听器，必须是一个实现EventListener接口的对象，
                或是一个函数
            
            options 可选
                指定有关监听器的参数对象
                    capture
                        布尔值，表示监听器在捕获阶段触发

                    once
                        布尔值，表示只触发一次
                    
                    passive
                        布尔值，表示监听器永远不会调用preventDefault()
                
            useCapture 可选
                布尔值，监听器在捕获阶段触发
        
    
    dispatchEvent(event)
        作用
            向指定的事件目标派送一个事件，并以合适的顺序同步调用目标元素的事件处理函数
        
        参数
            event
                派送的Event对象
    

    removeEventListener(type,listener[,options])
    removeEventListener(type,listener[,useCapture])
        作用
            通过事件名、监听器和选项参数移除监听器
        
        参数
            type
                事件类型
            
            listener
                需要移除的监听器
            
            options 可选
                监听器的特征
                    
                    capture
                        布尔值，捕获节点的监听器
            
            useCapture
                布尔值，捕获监听器，默认false


### 属性操作


    getAttribute(attributeName)
        作用
            获取元素指定属性的属性值
 
    getAttributeNames()
        作用
            获取包含元素的所有属性名称的数组

    getAttributeNS(namespace,name)
        作用
            获取指定命名空间的指定属性的属性值

    hasAttribute(attrName)
        作用
            判断元素是否含有指定属性

        返回
            布尔值

    hasAttributeNS(namespace,attrName)
        作用
            判断元素在指定命名空间内是否含有指定属性

        返回
            布尔值


    removeAttribute(attrName)
        作用
            移除指定属性
        
    removeAttribute(namespace,attrName)
        作用
            移除指定命名空间内的指定属性

    setAttribute(attrName,value)
        作用
            设置指定属性
        
    setAttribute(namespace,attrName,value)
        作用
            设置指定命名空间内的指定属性
        
    toggleAttribute(name[,force])
        作用
            切换指定布尔值属性的状态
        
        参数
            name
                属性名

            force 可选
                布尔值，是否删或添加属性
            
        返回
            布尔值，属性名最终存在返回true，否则返回false
    
### 位置获取


    getBoundingClientRect()
        作用
            获取元素的大小及其相对于视口的位置
        
        返回
            DOMRect对象

    getClientRects()
        作用
            获取与元素相关的ClientRect对象集合

### 元素选择

    getElementsByClassName(names)
        作用
            获取一个实时的HTMLCollection对象，包含所有拥有指定class的子元素，
            当在document对象上搜索，会检索整个文档，包括根元素
        
        参数
            names
                要匹配的类名列表，类名之间用空白符分隔

    getElementsByTagNames(tagName)
        作用
            获取一个实时的包含所有指定标签名的HTMLCollection集合
            搜索不包括元素自身
        
        参数
            tagName
                标签名

    getElementsByTagNamesNS(namespace,tagName)
        作用
            获取指定命名空间内的一个实时的包含所有指定标签名的HTMLCollection集合
            搜索不包括元素自身
        
    querySelector(selectors)
        作用
            获取与指定选择器组匹配的元素集合中的第一个元素
        
        参数
            selectors
                一个CSS选择器字符串
        
    
    querySelectorAll(selectors)
        作用
            获取与指定选择器组匹配的元素集合，该集合是非实时的
        
        参数
            selectors
                一个CSS选择器字符串
    
    matches(selectorString)
        作用
            判断元素是否被指定选择器字符串选中
        
        参数
            selectorString
                选择器
            
        返回
            布尔值



    closest(selector)
        作用
            获取与选择器匹配且离当前元素最近的祖先元素
        
        参数
            selector
                选择器


### 元素操作

    insertAdjacentElement(position,element)
        作用
            将一个给定元素插入到调用元素的给定位置
        
        参数
            position
                插入位置的字符串
                    beforebegin
                        在该元素前面
                    
                    afterbegin
                        在该元素第一个子节点前面
                
                    beforeend
                        该元素最后一个子节点的后面
                    
                    afterend
                        该元素的后面
                    
            element
                要插入的元素

    insertAdjacentHTML(position,text)
        作用
            将一个给定文本解析为Element元素，并插入到调用元素的给定位置
        
        参数

            text
                要被解析为HTML或XML的元素


    insertAdjacentText(position,text)
        作用
            将一个给定文本节点入到调用元素的给定位置
        
        参数
 
            text
               文本

    before(...nodes)
        作用
            在当前元素的前面插入一组Node或DOMString对象，
            DOMString对象等效Text节点
        
        参数
            nodes
                要插入的一组Node或DOMString对象

    after(...nodes)
        作用
            在当前元素的后面插入一组Node或DOMString对象，
            DOMString对象等效Text节点
        
        参数
            nodes
                要插入的一组Node或DOMString对象

    remove()
        作用
            移除元素自身和内部所有元素

    append(...nodes)
        作用
            在当前元素的最后一个子元素后面插入一组Node或DOMString对象

    replaceWith(...nodes)
        作用
            将当前元素替换为一组Node或DOMString对象


    replaceChildren([...nodes])
        作用
            用一组新的子节点替换旧的子节点
            如果未传入节点，会清空所有子节点

    

### 元素滚动

    scroll(xCoord,yCoord)
    scroll(options)
        作用
            将元素滚动到特定坐标，如果元素不含有滚动条就不会有效果
        
    scrollBy(xCoord,yCoord)
    scrollBy(options)
        作用
            将元素滚动一段距离，如果元素不含有滚动条就不会有效果

    scrollTo(xCoord,yCoord)
    scrollTo(options)
        作用
            将元素滚动到特定坐标，如果元素不含有滚动条就不会有效果
        


### 其他
    requestFullscreen([option])
        作用
            发出异步请求，使元素全屏模式显示，不能保证进入全屏
        
        参数
            option 可选
                一个FullscreenOptions对象，提供控制向全屏过渡的行为的选项，
                目前唯一选项是navigationUI，默认值为auto

        返回
            promise


## 一些对象类型


    DOMRect

        属性
            x
                矩形左上角的x坐标

            y
                矩形左上角的y坐标

            width
                矩形宽度
            
            height
                矩形高度
            
            top
                顶部坐标值
            
            right
                右坐标值
            
            bottom
                底部坐标值
            
            left
                左坐标值



    ScrollToOptions
        属性
            top 
                控制Y轴方向滚动像素数
            
            left
                控制x轴方向滚动像素值

            behavior
                指定滚动是否应该平滑进行，还是立即跳到指定位置

                    smooth
                        滚动
                    
                    auto
                        跳跃