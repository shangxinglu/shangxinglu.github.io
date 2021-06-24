# Canvas

## 标签width、height和css的width、height区别

    标签中的width和height决定了画布的可绘制像素点，

    css的width和height决定了画布最后呈现的大小，
    所以如果css中的值远大于标签中的值，就会显得模糊

    概括一下就是标签中的width和height决定我们绘制时的大小，
    css中的width和height决定了绘制的画布会被拉伸成多大
    
    建议不使用css去设置width和height
    
