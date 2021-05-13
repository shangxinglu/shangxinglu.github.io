# Closure

## Closure是什么

    Closure也就是常说的闭包，闭包就是记录函数实例在运行时的可访问标识符的结构

<br/>

## Closure如何产生
    函数每执行一次就会产生一个闭包

<br/>

## Closure的产生过程
    当函数开始执行，JavaScript首先会先创建一个执行环境，并将其可用的标识符列表指向函数作用域，这也是初始状态，此时闭包只是对函数作用域的一个引用，之后函数会进行this的绑定，将this添加到闭包中，紧接着就是执行函数内的可执行代码

    
