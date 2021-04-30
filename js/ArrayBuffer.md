# ArrayBuffer

## ArrayBuffer的作用是什么
    ArrayBuffer可以开辟出一块内存区域，用来存储操作二进制数据，普通的数组在内存中是不连续的，但ArrayBuffer是连续的，这就意味着操作ArrayBuffer的速度比Array快很多，在底层与其他程序的数据传输中，许多场景需要二进制数据并非Array默认的双精度浮点格式，Array还需要再进行数据格式的转化才可用，ArrayBuffer可以利用视图创建对应格式的二进制数据，直接传输并可用，时间上就会减少许多

<br/>

## ArrayBuffer的基本使用


### ArrayBuffer的创建

    ArrayBuffer可以创建一个固定长度的二进制缓冲区
    使用ArrayBuffer构造函数创建，有一个参数len，控制创建缓冲区的字节长度，0<len<Number.MAX_SAFE_INTEGER(2**53)


```javascript
    const buff = new ArrayBuffer(10);

    console.log(buff.byteLength); // 10

```

<br/>

### ArrayBuffer对象的属性和方法有哪些

    属性
        byteLength  只读
            数组的字节大小
    
    方法
        Array的方法ArrayBuffer大部分都可用，但是以下几个是不可用的
        
        concat
        pop
        push
        shift
        unshift
        splice
        
<br/>

## ArrayBuffer对象如何操作读取

    arrayBuffer是不可操作的，需要使用DataView或TypedArray对象进行操作和读取

<br/>

## DataView如何操作ArrayBuffer对象

    DataView对象拥有从ArrayBuffer对象读写多种数值的底层接口

### DataView对象创建

    构造函数
        DataView

            参数
                buffer
                    已存在的ArrayBuffer对象
                
                offset 可选
                    DataView对象在ArrayBuffer对象的第一个字节的位置，默认是0
                
                byteLength
                    DataView对象控制从第一个字节开始的长度，默认是ArrayBuffer对象的长度

