# Blob

## 初识Blob

Blob是一个对象，用来表示一个不可变的，原始数据的类文件对象
他的数据可以按照文本或二进制进行读取，也可以转化成ReadableStream用来数据操作

<br/>

## Blob使用

### 创建

使用Blob构造函数
new Blob(array,options)

这里有两个参数 array和options

  array

        是ArrayBuffer,ArrayBufferView,Blob,DOMString等对象构成的Array

  options

    是一个可选的对象，它有两个属性:

        type:默认值'',代表了将被放入blob的数组内容的MIME编码

        endings: 默认值是'transparent',指定包含结束符\n的字符串被如何写入,
        有两个值transparent和native
        
        transparent:代表会保持blob中的结束符不变
        native:代表结束符会被更改为合适宿主操作系统文件系统的换行符

```JavaScript
const buffer = new Int8Array(2);
const blob = new Blob(buffer);
```


### Blob对象属性

  size

    表示Blob对象包含数据的字节大小，只读

```JavaScript
blob.size; // 2
```

  type

    表示Blob对象包含内容的MIME类型，类型未知则为空字符串，只读

```JavaScript
blob.type; // ''
```

### Blob对象的方法

Blob.slice(start,end[,contentType])

    返回一个新的Blob对象，包含指定范围的数据

```JavaScript

const buffer = new Int8Array(10);

const blob = new Blob(buffer); // {size:10,type:''}

const newBlob = blob.slice(3,6); // {size:3,type:''}
```
<br/>

Blob.stream()

    返回一个可读取blob内容的stream

```JavaScript
    const content = '111222';
    const blob = new Blob([content],{
        type:'text/plain'
    });

    const readableStream = blob.stream()

    const reader = readableStream.getReader();

    reader.read().then(console.log);// {value: Uint8Array(8), done: false}

```
<br/>

Blob.text()

    返回一个promise且包含blob所有内容的UTF-8格式的USVString

```JavaScript
const blob = new Blob(['<h1>TEXT</h1>']);

blob.text().then(res=>{
    console.log(res); // <h1>TEXT</h1>
})
```
<br/>

Blob.arrayBuffer()

    返回一个promise且包含blob所有内容的二进制格式数据的ArrayBuffer

```JavaScript
const blob = new Blob(['<h1>TEXT</h1>']);

blob.arrayBuffer().then(res => {
    console.log(res); // ArrayBuffer(13)...
})

```



<br/>


## Blob的使用场景