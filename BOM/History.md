# History

## 作用
    history接口允许操作浏览器曾经在标签或框架里访问的会话历史记录

## 属性

    length 只读
        会话历史中元素的数量，包括当前加载页
    
    scrollRestoration
        滚动恢复属性，可以设置历史导航默认滚动恢复行为
        
            auto
                将恢复用户已滚动到的页面上的位置
            
            manual
                不还原页面上的位置，用户必须手动滚动到该位置
        
    state 只读
        返回在history栈顶的任意值拷贝，不必等待popstate事件发生后再查看


## 方法

    back()
        作用
            在会话历史记录中向后移动一页
        
    
    forward()
        作用
            在会话中向前移动一页
        

    go(delta)
        作用
            可以在历史记录中前后移动
        
        参数
            delta
                |delta|表示要移动的页数，正数前移，负数后移


    pushState(state,title[,url])
        作用
            向当前浏览器会话的历史堆栈中添加一个状态
        
        参数
            state
                状态对象，当用户导航到新状态时，都会触发popstate事件
            
            title
                标题，当前大多数浏览器都忽略此参数，最好传空字符串，防止将来变得可使用
            
            url 可选
                新历史记录条目的URL由此参数指定，浏览器不会在调用后尝试加载，
                但是可能会稍后加载，例如重启浏览器之后

                url必须与当前网址相同origin
    
        注意
            pushState()不会触发hashchange事件

    
    replaceState(state,title[,url])
        作用
            更新当前记录的state对象或者url

        state
            状态对象，当用户导航到新状态时，都会触发popstate事件
        
        title
            标题，当前大多数浏览器都忽略此参数，最好传空字符串，防止将来变得可使用
        
        url 可选
            新历史记录条目的URL由此参数指定，浏览器不会在调用后尝试加载，
            但是可能会稍后加载，例如重启浏览器之后

            url必须与当前网址相同origin




