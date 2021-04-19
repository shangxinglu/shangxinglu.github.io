# Async

## Async函数的作用

    异步函数让我们更简便的方式写出异步行为，也可以说是简化基于Promise使用的链式操作

<br/>

## Async函数的使用

    使用async关键字声明函数，内部可以使用await，注意await只能用在async函数内，async函数可以作为表达式使用


```javascript

    async function fun1(){
        await new Promise(resolve=>{
            console.log('resolve');
            resolve();
        });
    }

    fun1();

    console.log('out'); 
    // resolve
    // out
```

## Async函数的执行过程

    async函数在遇到await后，会返回一个promise，剩余代码会作为这个promise的resolve置值器运行，也就是then的回调函数体，也就意味着剩余代码会被作为微任务放入任务队列中

    async函数返回的promise在代码函数实例化之前就先创建了，所以以下异常会被catch捕获

```javascript
async function fun1([x]) {
}

const p1 = fun1();

p1.catch(()=>{
    console.log('catch');
});

// catch
```


```javascript

    async function fun1(){
        await new Promise(resolve=>{
            console.log('resolve');
            resolve();
        });

        console.log('fun1');


    }

    fun1();

    console.log('out'); 
    // resolve
    // out
    // fun1
```

## await关键字后面不同的数据类型会有什么不一样么

    await value相当于Promise.resolve(value)，所以不同数据类型的规则跟Promise.resolve()一样，只是await还会将微任务添加到任务队列，同时挂起函数

