# Flex

## flex
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


## flex-direction
    定义了主轴的方向

        row
            主轴方向被定义为与文本方向相同
        
        row-reverse
            表现与row相同，置换了起点与终点
        
        column
            主轴与快轴方向相同
        
        column-reverse
            表现与column相同，置换了起点与终点

## flex-wrap
    指定了flex元素是单行显示还是多行显示

        nowrap
            flex元素被摆到一行
        
        wrap
            flex元素被打断到多行
        
        wrap-reverse
            cross-start与cross-end互换


## flex-flow
    flex-direction与flex-wrap简写

        语法
            flex-direction flex-wrap


## order
    规定了弹性布局中的可伸缩项目在布局中的顺序

        <integer>
            表示此可伸缩项目的所在的次序

## justify-content    
    定义了浏览器如何分配顺着主轴方向上元素之间及其周围的空间

        start
            从行首开始排列，每行第一个元素与行首对齐，同时后续元素与前一个对齐，不受-reverse的影响

        flex-start
            从行首开始排列，每行第一个弹性元素与行首对齐，同时后续弹性元素与前一个对齐，受-reverse影响
        
        flex-end
            从行尾开始排列，每行最后一个弹性元素与行尾对齐，其他元素与后一个对齐
        
        center
            伸缩元素向每行中点排列
        
        space-between
            在每行上均匀分配弹性元素，相邻元素距离相同，第一个元素与行首对齐，最后一个元素与行尾对齐

        space-around
             在每行上均匀分配弹性元素，相邻元素距离相同，第一个元素和最后一个元素到行首和行尾的距离是元素间距离的一半
        
        space-evenly
            每行元素之间的距离都完全一样，包括第一个和最后一个
        

    

## align-content
    设置浏览器如何沿着弹性布局纵轴在内容项之间和周围分配空间
    对单行模型无效

        start
            所有行从容器的起始边缘开始填充
        
        end
            所有行从容器的结束边缘开始填充
        
        flex-start
            所有行从交叉轴的起点开始填充
        
        flex-end
            所有行从交叉轴的末尾开始填充
        
        center
            所有行向交叉轴中心填充

        space-between
            相邻两行间距相等，第一行与交叉轴的起点对齐，最后一行与交叉轴的终点对齐
        
        space-around
            相邻两行间距相等，第一行和最后一行到交叉轴起点和终点的距离是行间距的一半
        
        space-evenly
            所有行在交叉轴均匀分布

        stretch
            拉伸所有行来填满剩余空间，剩余空间均匀的分配给每一行


## align-items
    将所有直接子节点上的align-self值设为一个组

## align-self
    对齐当前flex行中的元素，并覆盖已有的align-items的值
    align-self不适合于块类型的盒模型
    任何flexbox元素在测轴方向的margin值为auto，align-self会被忽略

        auto
            设置为父元素的align-items的值
        
        normal
            取决于当前的布局模式
                在绝对定位的替代元素上表现为start，在其他所有绝对定位元素上表现为stretch
            
        self-start
            将项目对齐到与交叉轴中项目起始侧对应的对齐容器的边缘齐平
        
        self-end
            将项目对齐到与交叉轴中项目尾侧对应的对齐容器的边缘齐平

        flex-start
            对齐到交叉轴的首端
        
        flex-end
            对齐到交叉轴的末端
        
        center
            对齐到交叉轴中间

## place-content
    是align-content和justify-content的简写

## row-gap
    用来设置元素行之间的间隙

        <length>

        <percentage>
            栅格容器的百分比

## column-gap
    设置元素列之间的间隙
        <length>

        <percentage>
            栅格容器的百分比
        
## gap
    是row-gap和column-gap的简写

