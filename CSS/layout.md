# Layout

## 正常流布局
    正常流布局指在不对页面进行任何布局控制时，浏览器默认的布局方式
    
## Float浮动布局
    把一个元素浮动起来会改变该元素本身和在正常流布局中跟随他的元素的行为

## Flex弹性盒子布局
    弹性盒子布局被专门用来创建横向或纵向的一维页面布局

### flex
    简写属性，用来设置弹性项如何增大或缩小以适应弹性容器中可用的空间
    是以下CSS属性的简写
        flex-grow
        flex-shrink
        flex-basis

    单值语法
        无单位数
            被当做flex:<number> 1 0;
            flex-shrink为1，flex-basic为0
        
        有效宽度值  
            作为flex-basic的值，当做flex: 1 1 <width>
        
        关键字
            none
                当做flex:1 1 auto
            
            initial
                当做flex:0 1 auto
            
            auto
                当做flex:1 1 auto
    
    双值语法
        第一个数必须是无单位数，作为flex-grow的值
        第二个值为以下之一
            一个无单位数:作为flex-shrink的值
            一个有效宽度:作为flex-basic的值
        
    三值语法
        第一个数必须为一个无单位数
            作为flex-grow
        
        第二个数必须为一个无单位数
            作为flex-shrink
        
        第三个数必须是一个有效宽度
            作为flex-basic



    
    flex-grow
        设置了flex项主尺寸的增长系数，指定了flex容器中剩余空间的多少应该分配给项
        剩余空间就是flex容器减去所有flex项后的大小

        <number>
            负值无效，默认为0

    flex-shrink
        指定了flex元素的收缩规则，仅在默认宽度之和大于容器宽度时才会收缩
        收缩的算法有点不一样
        收缩量 = (自己的flex-shrink*自己的flex-basic)/(每一个的flex-shrink*flex-basic的乘机之和)*超出的量

        <number>
            负值无效

    
    flex-basic
        指定了flex元素在主轴方向上的初始化大小，优先级比width高
        如果想完全忽略flex子元素的大小，可以设置flex-basic=0

        <'width'>
            可以是length，也可以是相对于其父弹性容器主轴尺寸的百分数
            负值不被允许，默认是auto
        
        content
            基于元素的内容进行自动调整


### flex-direction
    定义了主轴的方向

        row
            主轴方向被定义为与文本方向相同
        
        row-reverse
            表现与row相同，置换了起点与终点
        
        column
            主轴与快轴方向相同
        
        column-reverse
            表现与column相同，置换了起点与终点

### flex-wrap
    指定了flex元素是单行显示还是多行显示

        nowrap
            flex元素被摆到一行
        
        wrap
            flex元素被打断到多行
        
        wrap-reverse
            cross-start与cross-end互换


### flex-flow
    flex-direction与flex-wrap简写

        语法
            flex-direction flex-wrap


### order
    规定了弹性布局中的可伸缩项目在布局中的顺序

        <integer>
            表示此可伸缩项目的所在的次序

### align-content




## Grid布局
    Grid布局被设计用于同时在两个维度上把元素按行和列排列整齐

## 定位技术
    定位能够让我们把一个元素从原本的正常流布局中移动到另一个位置
    定位技术并不是用来主要页面布局的，而是用来管理和微调页面中一个特殊位置