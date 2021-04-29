# Generator

## Generator是什么

    Generator也叫生成器，是异步编程的一种解决方案，JavaScript通过Generator将执行栈的控制交给了用户，因为Generator可以暂停和恢复函数的执行，这也是它的最强大之处

<br/>

## Generator的使用

<br/>

### 基本使用

    在function后面加上一个*就是一个生成器函数，能定义函数的地方就能定义生成器，除了箭头函数

```JavaScript
    function* fun1(){

    }

    const fun2 = function* (){

    }


    class A{
        * fun3(){

        }
    }
```

### 生成器对象的方法

    生成器函数调用后不会执行，而是返回一个生成器对象，这个对象有几个方法: next,return,throw

<br/>

#### next

<br/>

    生成器函数的内部有一个yield关键字，当执行next()时，遇到yield关键字会暂停，返回值为一个对象，有两个属性：value，done
        value就是yield后面的值
        done表示生成器对象是否关闭

    next有一个参数，作为上次暂停时yield的值，继续执行

```javascript
    function* fun1(){
        console.log('start');

        const x = yield 100;

        console.log(x);

        return 'finally';
    }

    const gen = fun1();

    let result = gen.next(); // start

    console.log(result); // {value:100,done:false}

    result = gen.next(300); // 300

    console.log(result); // {value:'finally',done:true}

```
<br/>

#### return
    return用来强制关闭生成器对象，return方法还有一个参数，作为终止生成器对象的值

```javascript
    function* fun1(){
        console.log('start');

        const x = yield 100;

        console.log(x);

        return 'finally';
    }

     const gen = fun1();

     let result = gen.return('end');

     console.log(result); // {value:end,done:true}

```
    
<br/>

#### throw

    throw会在内部抛出一个错误，同时也相当于执行了一次next，如果错误在内部被处理了，那么还能继续执行，不然会关闭生成器对象

    这里需要注意一下，如果在未执行next的情况下，直接使用throw，相当于在外部抛出错误，因为此时Generator内部代码还未启动

```javascript
    function* fun1(){
        
        try{

            const x = yield 100;
            yield 200;
            yield 300;

        }catch(e){
            console.log('catch');
        }

      
        yield 400;

        return 'finally';
    }

     const gen = fun1();

     let result = gen.next();
     result = gen.throw('throw'); // catch
     console.log(result); // {value:400,done:false}

     result = gen.next();
     console.log(result); // {value:'finally',done:true}

```

<br/>

## 异步生成器

    异步生成器与生成器的区别在于，调用next方法后，返回的是一个promise对象，需要在then中才能获取到IteratorResult对象

```javascript
    async function* fun1(){
        const result = yield 1;

        console.log(result);
    }

    const gen = fun1();

    let result = gen.next();

    result.then(console.log); // { value:1,done:false}

    gen.next(200); // 200

```

    还有一点，异步生成器的yield后面会先自动加上await



```javascript
    function fun1(){
        return new Promise(resolve=>{
            setTimeout(resolve,1000);
        });
    }

    async function* fun2(){
        let  result = yield fun1();

        // 相当于
        // let result = yield await fun1().then(res=>{value:res,done:false};

        console.log('1');
        result = yield fun1();
        console.log('2');

    }

    const gen = fun2();

    let result = gen.next();
    gen.next(); // 1s后打印 1
```

## Generator的使用场景  