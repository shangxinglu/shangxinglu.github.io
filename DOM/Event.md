# Event

## 描述
    Event接口表示在DOM中出现的事件，一些事件由用户触发，而其他事件由API生成
    Event本身包含适用于所有事件属性和方法


## Event

### 事件对象创建

    new Event(type[,eventInit])
        参数
            type
                创建事件的名称
            
            eventInit
                EventInit类型的字典，接受以下字段

                    bubbles 可选
                        布尔值，默认false，表示事件是否冒泡
                    
                    cancelable 可选
                        布尔值，默认false，表示事件能否被取消
                    
                    composed 可选
                        布尔值，默认false，指示事件是否会在影子DOM根节点之外触发侦听器
                    
```HTML
    <div class="d1">
        <div class="d2"></div>
    </div>
```

```JavaScript
    const d1El = document.querySelector('.d1');
    const d2El = document.querySelector('.d2');

    d1El.addEventListener('fly',()=>{
        console.log('fly d1');
    })

    d2El.addEventListener('fly',()=>{
        console.log('fly d2');
    })

    const fly = new Event('fly',{bubbles:true});

    d2El.dispatchEvent(fly); // fly d2
                             // fly d1 
```

### 属性

    bubbles 只读
        布尔值，是否在DOM中冒泡
    
    cancelBubble
        布尔值，是否可阻止继续冒泡
    
    cancelable 只读
        布尔值，事件是否可取消，可以通过这个属性来进行条件判断

    composed 只读
        布尔值，事件是否可以穿过Shadow DOM和常规DOM之间的隔阂进行冒泡
    
    currentTarget 只读
        对当前注册目标的引用，可能在重定向中被改变
    
    defaultPrevented 只读
        布尔值，当前事件是否调用了event.preventDefault()

    eventPhase 只读
        代表当前执行阶段的整数值

    returnValue
        表示该事件的默认操作是否已被阻止，默认true,
        设置为false即可阻止默认操作

    target 只读
        触发事件的对象的引用

    timeStamp 只读
        事件发生时时间戳，注意在Gecko中不是正确的时间戳

    type 只读
        事件类型，不区分大小写
    
    isTrusted 只读
        布尔值，事件由用户行为生成时为true，
        事件由脚本创建、修改、通过EventTarget.dispatchEvent()派发时为false


### 方法

    composedPath()
        作用
            获取一个对象数组，表示将在其上调用事件侦听器的对象

    preventDefault()
        作用
            如果事件可取消就取消事件

    stopImmediatePropagation()
        作用
            阻止同一元素的同一事件类型的监听器继续调用

    stopPropagation()
        作用
            阻止捕获和冒泡阶段中当前事件的进一步传播



## MouseEvent

### 创建

    new MouseEvent(type[,mouseEventInit])
        参数
            type
                事件名

            mouseEventInit
                初始化MouseEvent的字典，有以下属性

                    screenX 可选
                        long型，默认0，设置鼠标事件发生时相对于用户屏幕的水平坐标位置
                    
                    screenY 可选
                        long型，默认0，设置鼠标事件发生时相对于用户屏幕的垂直坐标位置
                    
                    clientX 可选
                        long型，默认0，设置鼠标事件发生时相对于客户端窗口的水平坐标位置
                    
                    clientY 可选
                        long型，默认0，设置鼠标事件发生时相对于客户端窗口的垂直坐标位置
                    
                    ctrlKey 可选
                        布尔型，默认false，表明是否同时按下ctrl键
                    
                    shiftKey 可选
                        布尔型，默认false，表明是否同时按下shift键
                    
                    altKey 可选
                        布尔型，默认false，表明是否同时按下alt键
                    
                    metaKey 可选
                        布尔型，默认false，表明是否同时按下meta键
                    
                    button 可选
                        short型，默认0，表明哪个键被按下或释放
                            0
                                主键，通常是左键

                            1
                                辅助键，通常是中键
                            
                            2
                                次键，通常是右键
                    
                    relatedTarget 可选
                        EventTarget型，默认null
                    
                    region 可选
                        默认null，表明点击事件影响的区域DOM的id

                    

## KeyboardEvent

### 创建

    new KeyboardEvent(type[,keyboardEventInit])
        参数
            type
                事件类型
            
            keyboardEventInit 可选
                有以下属性，都是可选值，别用来设置对应的属性
                    key,code,loaction,ctrlKey,shiftKey,altKey,metaKey,
                    repeat,isComposing
            
### 属性

    altKey 只读
        布尔值，表明alt键是否被按下

    ctrlKey 只读
        布尔值，表明ctrl键是否按下
    
    shiftKey 只读
        布尔值，表明shift键是否按下
    
    metaKey 只读
        布尔值，表明meta键是否按下

    
    key 只读
        用户按下的物理按键的值，受修饰键影响

    code 只读
        表示键盘上的物理键，不受修饰键影响

    isComposing 只读
        布尔值，表示事件是否在compositionstart之后和compositionend之前触发
    
    location 只读
        unsigned long类型，表示按键在键盘或其他设备上的位置
    
    repeat 只读
        布尔值，如果一直被按住，则为true


## DragEvent

### 描述
    DragEvent是一个表示拖、放交互的一个接口

### 可拖拽元素
    在HTML中，除了图像、链接和选择的文本默认的可拖拽行为之外，其他元素默认都是不可拖拽的

    如果要是其他HTML元素可拖拽，需要以下操作
        设置元素的draggable属性为true
        为元素添加dragstart事件监听器

### 可放置元素
    想要让一个元素变成可释放区域，必须要给元素设置dragover和drop事件处理程序，
    在阻止dragover的默认操作

### 拖拽流程
    假设有拖拽元素source和放置元素target

    在source的dragstart事件中设置allowEffect，控制source的支持的操作

    在target的dragenter事件中设置dropEffect，控制target支持的操作

    在source的dragstart事件设置拖拉的数据

    在target的drop事件中获取拖拉的数据

    

### 属性

    dataTransfer 只读
        DataTransfer对象类型，保存拖放期间传输的数据

### DataTransfer对象
    
    属性

        dropEffect
            当前选定的拖放操作类型，必须是以下值之一
                none 不允许操作
                copy 复制
                link 链接
                move 移动
        
        effectAllowed
            提供所有可操作性的类型，必须是以下值之一
                none 不允许操作
                copy 只复制
                link 链接
                move 只移动

                copyLink 复制或链接
                copyMove 复制或移动
                linkMove 链接或移动
                all 复制、移动、链接
                
                uninitialize 未设置 相当于all
        
        files
            数据传输中可用的所有本地文件的列表
        
        items 只读
            提供一个包含所有拖动数据列表的DataTransferItemList对象,
            DataTransferItem就是拖拽项的数据
        
        types 只读
            在dragstart事件中设置的拖动数据格式的数组


    方法
        clearData()
            作用
                删除给定类型关联的数据

        getData()
            作用
                检索给定类型的数据

        setData()
            作用
                设置给定类型的数据
        
        setDragImage()
            作用
                自定义拖动图像

### 事件

    drag
        用户拖动元素或文本时触发

    dragstart
        当用户开始拖动元素或选择文本时触发

    dragend
        拖动操作结束触发

    dragenter
        拖动元素或选择文本输入有效的放置目标时触发

    dragleave
        当拖动的元素或文本离开有效的放置目标时触发
    
    dragover
        当元素或文本被拖动到有效的放置目标时触发

    drop
        在有效放置目标上放置元素或选择文本时触发



## 剪切板事件

### Clipboard API
    Clipboard API提供了响应剪切板命令(剪切、复制、粘贴)与异步读写剪切板的能力
    Clipboard对象通过navigator.clipboard接口访问到

### Clipboard对象

    方法

        read()
            作用
                请求剪贴板内容的副本，可以获取任意数据
                需要提前获取到`clipboard-read`的权限
            
            返回
                promise
            
        
        readText()
            作用
                获取剪切板的文本内容
            
            返回
                promise
            
        
        write(dataTransfer)
            作用
                向剪切板写入图片等任意数据，
                需要提前获取到`clipboard-write`的权限
            
            参数
                dataTransfer
                    dataTransfer对象
            
            返回
                promise

            
        writeText(text)
            作用
                向剪切板写入字符串

            参数
                text
                    写入剪切板的文本内容

            返回
                promise
    

### 事件

    copy
        复制

    cut
        剪切

    paste
        粘贴
    
    


        
    

                        