# Event Loop

我们先来看一个简单的代码示例

```JavaScript
console.log('1');

setTimeout(()=>{
    console.log('2');
},0);

Promise.resolve().then(()=>{
    console.log('3');
}).then(()=>{
    console.log('4');
});

console.log('5');

// 1 5 3 4 2

```

这里主要是涉及到了js的事件循环机制的问题，JavaScript是单线程非阻塞的，所有的异步任务会被放入队列中，当执行上下文堆栈是为空，就会去读取任务队列，这个过程会不断重复，所以也叫Event Loop(事件循环)

<br/>
<br/>

### 队列
<br/>

ECMAScript规范中规定，队列中的任务在没有正在执行上下文且执行上下文堆栈为空时才会调用，所以1，5会在异步回调之前调用完成
至于2为什么会在3后面，是因为宏队列和微队列的关系
<br/>
<br/>

### 微队列
<br/>

微队列会在每个宏任务执行前，全部按顺序执行，直到清空，如果在此期间又产生了一个新的微任务，会添加到微队列末尾，也就是说会在此次宏任务执行前执行

微任务: Promise、MutationObserver
<br/>
<br/>

### 宏队列
<br/>

宏队列的优先级比微队列要低

宏队列:setTimeOut、setInterval、UI

因为3，4是微任务，2是宏任务，所以3，4在2之前执行
