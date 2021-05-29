# ArrayBuffer

## ArrayBuffer的作用是什么
    ArrayBuffer可以开辟出一块内存区域，用来存储操作二进制数据，普通的数组在内存中是不连续的，但ArrayBuffer是连续的，这就意味着操作ArrayBuffer的速度比Array快很多，在底层与其他程序的数据传输中，许多场景需要二进制数据并非Array默认的双精度浮点格式，Array还需要再进行数据格式的转化才可用，ArrayBuffer可以利用视图创建对应格式的二进制数据，直接传输并可用，时间上就会减少许多

<br/>

## ArrayBuffer的基本使用


### ArrayBuffer的创建

    ArrayBuffer可以创建一个固定长度的二进制缓冲区
    使用ArrayBuffer构造函数创建，有一个参数len，控制创建缓冲区的字节长度，0<len<Number.MAX_SAFE_INTEGER(2**53)


```JavaScript
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

<br/>

### DataView对象创建

    构造函数
        DataView

            参数
                buffer
                    已存在的ArrayBuffer对象
                
                offset 可选
                    DataView对象在ArrayBuffer对象的第一个字节的位置，默认是0
                
                byteLength
                    DataView对的字节长度，默认是ArrayBuffer对象的长度
        
<br/>

### DataView对象如何读写数据

    每个DataView对象用以下方法读取数据，U代表无正负符号，数字代表bit数，Int代表整数，Float代表浮点数

        getInt8

        getUint8

        getInt16

        getUint16

        getInt32

        getUint32

        getFloat32

        getFloat64
    
    
    写入与读取一样也有对应的方法

        setInt8

        setUint8

        setInt16

        setUint16

        setInt32

        setUint32

        setFloat32

        setFloat64

<br/>

```JavaScript
    const buff = new ArrayBuffer(20),
    view = new DataView(buff);

    view.setInt8(1,100);

    view.getInt8(1); // 100
```


## 大字节端和小字节端的区别是啥

    大字节端就是高位字节在前面，低位字节在后面

    小字节端就是低位字节在前面，高位字节在后面

    计算机电路优先处理低位字节，效率比较高，因为计算都是从低位开始的，所有在计算机内部处理都是小端字节序，但是人的习惯都是大端字节序，所有除了在计算机内部处理，其他场合几乎都是大端字节序


<br/>

## typedArray对象的创建

    typedArray指的是一组类型化的数组的构造函数，拥有一样的语法，有以下几种

        Int8Array

        Uint8Array

        Uint8ClampedArray

        Int16Array

        Uint16Array

        Int32Array

        Uint32Array

        Float32Array

        Float64Array

    
    不同的参数类型有不同的参数
        length
            当传入参数为整数是，代表数组缓冲区的长度，以调用的构造函数的字节数为单位
        
```JavaScript
    const arrayBuf = new Int32Array(2);

    console.log(arrayBuf.length); // 2
    console.log(arrayBuf.byteLength); // 8

```

        typedArray
            当传入的参数是一个typedArray的对象，会被复制到一个新的typedArray对象中，数组的长度不会变
        

```JavaScript
    const arrayBuf = new Int8Array(2),
    arrayBuf1 = new Int16Array(arrayBuf);

    console.log(arrayBuf1.length); // 2
    console.log(arrayBuf1.byteLength); // 4

```

        object
            当传入的参数是一个对象时，就相当于通过TypedArray.from()方法创建一个类型化数组

```JavaScript
    const arrayBuf = new Int8Array({length:3});
    console.log(arrayBuf.length); // 3
```

        buffer
            当传入的的参数为ArrayBuffer对象，很DataView构造函数的参数是一样的

```JavaScript
    const buff = new ArrayBuffer(4);
    const arrayBuf = new Int8Array(buff,1);

    console.log(arrayBuf.length); // 3
```


<br/>



