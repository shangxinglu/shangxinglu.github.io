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
        这是一个对象，包含几个方法

        start(controller)
            当对象被构造时会立即调用

        pull(controller) 可选
            当流内部队列不满时，会重复调用这个方法，直到补满

        cancel(reson) 可选
            当应用程序表示将取消流时，会调用此方法

    queueingStrategy:

```javascript

```

<br/>

### 可写流

<br/>

### 转换流

<br/>

## Stream执行过程

Stream会被拆分成块，翻入到一个队列中，队列占用的内存有个上限值，这个上限叫高水位线，当达到高水位线，如果队列不被消费，就会暂停流

<br/>

## Stream的使用场景有哪些

Stream可以用于视频处理、数据压缩、图像编码、JSON解析

## 使用Stream去完成一些数据的