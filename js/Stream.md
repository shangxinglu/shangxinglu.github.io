# Stream

## Stream是什么

Stream是将网络资源拆分成小块，可以让应用在数据一到达就可用，而无需全部加载完毕

为了方便理解，可以把数据想象成管道运输中的液体


<br/>

## Stream如何使用

Stream有三种类型

    1.可读流
    2.可写流
    3.转换流

<br/>

### 可读流

    可读流是一个可以通过公共接口读取数据块的流

<br/>

#### 可读流的创建

  ReadableStream()构造器可以创建并返回包含处理函数的可读流实例，它有两参数:
    
    underlyingSource:
        这是一个对象，包含几个属性和方法

        start(controller)
            当对象被构造时会立即调用
            controller是一个ReadableStreamDefaultController或ReadableByteStreamController类型的对象

        pull(controller) 可选
            当流内部队列不满时，会重复调用这个方法，直到补满
             controller是一个ReadableStreamDefaultController或ReadableByteStreamController类型的对象

        cancel(reson) 可选
            当应用程序表示将取消流时，会调用此方法

        type 可选
            控制正在处理的可读类型的流，如果值为bytes，则controller将是ReadableByteStreamController，不然为ReadableStreamDefaultController

        autoAllocateChunkSize 可选
            对于字节流，可以用来打开流的自动分配功能

    queueingStrategy 可选:

        定义流的排队策略的对象，有两个参数

            highWaterMark
                非负整数，定义了在应用背压之前可以包含在内部队列中的块的总数

            size(chunk)
                包含参数chunk的方法，表示每个块使用的大小，单位字节

```javascript

    const content = '我是一个文件!!!!!!!!!!!';

    let i = 0;

    const readableStream = new ReadableStream({
        start(controller){
            controller.enqueue(content);
        },

        pull(controller){
            controller.enqueue(i++);
        }
    });

    const reader = readableStream.getReader();

    async function read(){
        const result = await reader.read().then(res=>res);
        console.log(result.value);
        return;
    }

    read(); // 我是一个文件!!!!!!!!!!!
    read(); // 0
    read(); // 1

```

<br/>

#### ReadableStreamDefaultController类型对象怎么创建

    ReadableStreamDefaultController实例没有办法手动创建，只会在ReadableStream构建过程中自动创建

<br/>

#### ReadableStreamDefaultController类型对象的作用是什么

    表示一个控制器，用来控制ReadableStream的状态和内部队列，默认控制器用于非字节流

#### ReadableStreamDefaultController有哪些属性和方法

    desiredSize 只读：
        返回填充流的内部队列所需的大小

    close()：
        关闭关联的流

    enqueue():
        将给定的块放入到关联的流中

    error()：
        会导致之后与关联流的任何交互都会出错

<br/>

#### ReadableByteStreamController类型对象怎么创建

    ReadableStreamDefaultController实例同样是没有办法手动创建，只会在ReadableStream构建过程中自动创建

<br/>

#### ReadableByteStreamController类型对象的作用是什么

    表示一个控制器，用来控制ReadableStream的状态和内部队列，控制器用于字节流

#### ReadableByteStreamController有哪些属性和方法

    byobRequest (byob是bring your own buffer 带上自己的缓冲区的意思)：
        当前BYOB拉取请求

    desiredSize 只读：
        返回填充流的内部队列所需的大小

    close()：
        关闭关联的流

    enqueue():
        将给定的块放入到关联的流中

    error()：
        会导致之后与关联流的任何交互都会出错

<br/>

#### ReadableStreamDefaultReader

    ReadableStreamDefaultReader是读取器类型，用来读取流数据

    创建
        一般会用raderStream.getReader()获取实例，比较方便，也可以用构造函数ReadableStreamDefaultReader创建，有一个参数，是ReadableStream的实例

    属性

        closed
            如果readableStream被锁定或者被释放返回fulfilled状态的promisep
            如果报错返回rejected的promisep
    
    方法

        read()
            返回一个promise，用来访问内部队列的下一个块
    
        cancel()
            取消这个readableStream

        releaseLock()
            释放对这个readableStream的锁定

```javascript

    const content = '我是一个文件!!!!!!!!!!!';

    const readableStream = new ReadableStream({
        start(controller){
            controller.enqueue(content);
        }
    });

    const reader = new ReadableStreamDefaultReader(readableStream);

    reader.read().then(console.log); // {value:'我是一个文件!!!!!!!!!!!',done:false}

```

#### ReadableStreamBYOBReader

    ReadableStreamBYOBReader是读取器类型，可以将流直接读入提供的缓冲区，减少后期的复制开销

    创建
        一般会用raderStream.getReader()获取实例，比较方便，也可以用构造函数ReadableStreamBYOBReader创建，有一个参数，是ReadableStream的实例

    属性

        closed 只读
            如果readableStream被锁定或者被释放返回fulfilled状态的promise
            如果报错返回rejected的promise
    
    方法

        read()
            返回一个promise，用来访问内部队列的下一个块
    
        cancel()
            取消这个readableStream

        releaseLock()
            释放对这个readableStream的锁定

#### 可读流实例的使用

<br/>

  locked

    只读
    用来判断当前可读流是否被读取器锁定

```javascript

const content = '我是一个文件!!!!!!!!!!!';

const read = new ReadableStream({
    start(controller){
        controller.enqueue(content);
    }
});

console.log(read.locked); // false

read.getReader();

console.log(read.locked); // true

```

<br/>

  cancel()

    用来取消读取流，返回一个Promise对象，注意需要在可读流在未锁定状态调用

```javascript
const content = '我是一个文件!!!!!!!!!!!';

const readableStream = new ReadableStream({
    start(controller){
        controller.enqueue(content);
    },

    cancel(reason){
        console.log(reason);
    }
    
});


readableStream.cancel('reason'); // reason

```


<br/>

  getReader()
    
    创建一个读取器并将流锁定于其上，有一个参数，是含有mode属性的对象，
        mode：
            byob
            返回ReadableStreamBYOBReader
        
            其他
            返回ReaderStreamDefaultReader

    

```javascript

    const content = '我是一个文件!!!!!!!!!!!';

    const readableStream = new ReadableStream({
        start(controller){
            controller.enqueue(content);
        }
    });

    const reader = readableStream.getReader();

    reader.read().then(console.log); // {value:'我是一个文件!!!!!!!!!!!',done:false}

```

<br/>

  pipeThrough()

    用来将当前管道流输入到转换流或可读可写流

```javascript


```

  pipeTo()

    将当前可读流输出到给定的可写流，并且返回一个Promise对象

```javascript
    const content = '1234567890';

    const readableStream = new ReadableStream({
        start(controller){
            controller.enqueue(content);
        }
    });

    const writableStream = new WritableStream({
        start(){
            console.log('start');
        },

        write(chunk,controller){
            console.log('write',chunk);
        },
    });

    readableStream.pipeTo(writableStream); // wirte 1234567890

```

  tee()

    会将当前可读流锁定，并返回两个当前可读流的分支，可独立操作
    

### 可写流

    可读流用于流数据写入到目的地，被称为一个水槽的标准对象，该对象有内置的反压和排队功能

#### 可写流的创建

    WritableStream()构造器可以创建并返回包含处理函数的可写流实例，它有两参数:
    
    underlyingSource:
        这是一个对象，包含几个属性和方法

        start(controller)
            当对象被构造时会立即调用
            controller是一个WritableStreamDefaultController类型对象

        write(chunk,controller) 可选
            chunk是要写入的数据块，会被写入基础接收器
            controller是一个WritableleStreamDefaultController类型对象

        close(controller) 可选
            作用
                当块写入完成将会调用此方法，释放对接收器的访问权限所需的一切

            参数
                controller
                    一个WritableleStreamDefaultController类型对象
            
            返回值
                如果是异步的返回一个promise

        abort(reson)
            作用
                当应用发出关闭流并将其至于错误的信号，将调用此方法，可以清除任何保留资源

            参数
                reson
                    错误原因

             返回值
                如果是异步的返回一个promise


    queueingStrategy 可选:

        定义流的排队策略的对象，有两个参数

            highWaterMark
                非负整数，定义了在事假反压之前内部队列中的块的总数

            size(chunk)
                包含参数chunk的方法，表示每个块使用的大小，单位字节    


```javascript
    const writableStream = new WritableStream({
        start(controller){
            console.log('start');
        },

        write(chunk,controller){
            console.log('write',chunk);
        },

        close(controller){
            console.log('close');
        },

        abort(reson){
            console.log(reson);
        }

    });

    const writer = writableStream.getWriter(); // write

    writer.write(111); // write 111
```

<br/>

#### WritableStreamDefaultController的作用

    用来控制WritableStream的状态

#### WritableStreamDefaultController实例怎么创建

    WritableStreamDefaultController实例没有办法手动创建，只会在WritableStream构建过程中自动创建

<br/>

#### WritableByteStreamController实例的方法

    error()：
        会导致之后与关联流的任何交互都会出错

<br/>

#### WritableStream实例的使用

    属性

        closed 只读
            布尔值，表示当前写入流是否锁定写入器
    
    方法

        abort()
            终止写入流，并将其移至错误状态，并丢弃所有剩余的队列数据

        close()
            关闭流
        
        getWriter()
            会返回一个WritableStreamDefaultWriter写入器实例，同时改写入流会别锁定到这个实例
    

#### WritableStreamDefaultWriter

    WritableStreamDefaultWriter是写入器类型，用来在流中放入数据

    创建
        一般会用Writable.getWriter()获取实例，比较方便，也可以用构造函数WritableStreamDefaultWriter创建，有一个参数，是WritableStream的实例
    
    属性
 
        closed 只读
            如果writableStream被关闭或者被释放返回fulfilled状态的promise
            如果报错返回rejected的promise

        desireSize 只读
            返回写入流内部队列所需的大小
        
        ready 只读
            返回一个promise，如果状态为fulfilled表示内部对象所需大小从负向正转换，不再施加背压

    方法

        abort()
            终止写入流，并将其移至错误状态，并丢弃所有剩余的队列数据

        close()
            关闭流

        releaseLock()
            释放写入器与写入流的锁定

        write()
            将数据块写入可写流及其底层接收器，会返回一个promise用来判断写入的成功还是失败

 
```javascript

    const writableStream = new WritableStream({
        start(controller){
            console.log('start');
        },

        write(chunk,controller){
            console.log('write');
        },

        close(controller){
            console.log('close');
        },

        abort(reson){
            console.log(reson);
        }

    });


    const writer = writableStream.getWriter(); // write

```



   


### 转换流
    转换流是用来组合可读流和可写流

<br/>

## Stream执行过程

Stream会被拆分成块，翻入到一个队列中，队列中块的数量有个上限值，这个上限叫高水位线，当达到高水位线，如果队列不被消费，就会暂停流

<br/>

## Stream的使用场景有哪些

Stream可以用于视频处理、数据压缩、图像编码、JSON解析

<br/>

