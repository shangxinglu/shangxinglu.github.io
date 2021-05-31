# Promise

## Promise是什么

    Promise是一种异步编程的解决方案，同时JavaScript通过Promise将任务队列管理交到了用户手中

<br/>

## Promise的使用
    
    Promise有3中状态: penging(待定)、fulfilled(已兑现)、rejected(已拒绝)，
    一旦当状态变为fulfilled或rejected，就不会在发生变化

    Promise构造函数有一个参数，是一个函数，这个函数有两个参数resolve和reject

    resolve:
        是一个函数，将Promise对象的状态改为fulfilled

    reject:
        是一个函数，将Promise对象的状态改为rejected

```JavaScript
    let res = null,
    rej = null;
    const p = new Promise((resolve,reject)=>{
        res = resolve,
        rej = reject;
    });
    console.log(p); // Promise{pending}

    res();

    console.log(p); //Primise{fulfilled}

    rej();

    console.log(p); //Primise{fulfilled}

```

<br/>

### then()

    当Promise对象状态变为fulfilled或rejected时，会调用后面的then方法

    then方法有两个参数，都是函数，分别在状态为fulfilled和rejected时调用

```JavaScript
    const p = new Promise((resolve,reject)=>{
       resolve()
    }).then(()=>{
        console.log('fulfilled');
    }); // fulfilled

    const p1 = new Promise((resolve,reject)=>{
       reject()
    }).then(null,()=>{
        console.log('rejected');
    }); // rejected
```
    then调用后会会返回一个新的promise

```JavaScript
    let res = null;
    const p = new Promise(resolve=>{
        res = resolve;
    });

    const p1 = p.then(()=>{});

    console.log(p===p1); // false

    console.log(p1); // Promise{pending}

    res();

    console.log(p); // Promise{fulfilled}
    console.log(p1); // Promise{fulfilled}

```

  当then中的函数接受的参数为一个Promise对象，会有几种情况

    Promise(后面简称p1)为fulfilled状态，then返回的Promise对象(后面简称p2)的状态会
    变为fulfilled，p1传给then回调函数的参数值作为p2的then回调函数的参数值

```JavaScript
    const p1= new Promise(resolve=>{
        resolve('p1');
    });

    const p2= new Promise(resolve=>{
        resolve(p1);
    });

    p2.then(res=>{
        console.log(res); // p1
    });

```

    Promise(后面简称p1)为rejected状态，then返回的Promise对象(后面简称p2)的状态会
    变为rejected，p1传给then回调函数的参数值作为p2的then回调函数的参数值

```JavaScript
    const p1= new Promise((resolve,reject)=>{
        reject('p1');
    });

    p1.catch(error=>{});

    const p2= new Promise((resolve,reject)=>{
        resolve(p1);
    });

    p2.catch(res=>{
        console.log(res); // p1
    });

```

    Promise(后面简称p1)为pending状态，then返回的Promise对象(后面简称p2)的状态会保
    持pending，当p1发生状态改变，p1的传给then回调函数的参数值传给p2的then回调函数
    的参数，变为相同状态

```JavaScript
    let res = null;
    const p1= new Promise((resolve,reject)=>{
        res = resolve;
    });

    const p2= new Promise((resolve,reject)=>{
        resolve(p1);
    });

    p2.then(console.log);

    res(200);  // 200

```

    当then的回调函数返回一个promise(p1)，这个then返回的promise(p2)的state和result
    都会跟p1同步，虽然p1!==p2，但是效果上相当于p1===p2，方便理解

```JavaScript
    let res = null;

    const p1 = new Promise((resolve, reject) => {
        // resolve('p1');
        res = resolve;
        // reject('p1');
    });

    const p2 = new Promise(resolve => {
        resolve('p2');

    }).then(result => {
        console.log(result);
        res(2222222);
        return p1;
    });
    
    p2.then(res => {
        console.log(res); // 2222222
        console.log(p1 === p2); // false

    });

```

<br/>

### catch()

    catch(call)相当于then(undefined,call)

```JavaScript
    const p1 = new Promise((resolve,reject)=>{
        reject('reject');
    }).then(undefined,error=>{
        console.log(error); // reject
    });

    const p2 = new Promise((resolve,reject)=>{
        reject('reject');
    }).catch(error=>{
        console.log(error); // reject
    })

```

<br/>

### Promise.resolve(value)
    
    作用
        将直接返回给定值解析后的fulfilled状态的promise

    参数
        value
            任意值

    返回值
        promise
    
  value在以下几种不同类型时，有所区别

    promsie
        直接返回这个promise对象

```JavaScript
    const p1 = new Promise(()=>{});
    const p2 = Promise.resolve(p1);

    console.log(p1===p2); // true

```
    thenable对象(有then方法的对象)
        then方法相当于这个promise的执行器，也就是Promise构造函数的参数，拥有一样的
        参数，promise的状态会有then方法决定

```JavaScript
    let res = null;
    const obj = {
        then(resolve, reject) {
            res = resolve;
        }
    };

    const p1 = Promise.resolve(obj);

    // 相当于
    // Promise.resolve(obj).then(result=>{
    //     return new Promise(result.then);
    // })
    

    console.log(p1); //Promise {<pending>}
    Promise.resolve().then(()=>{
        res();
        console.log(p1); //Promise {<fulfilled>}
    })
```
    其他
        返回给定值的fulfilled状态的promise

<br/>

### Promise.reject(reson)

    作用
        返回一个带有拒绝原因的rejected状态的promise
    
    参数
        reson
            拒绝原因

    返回值
        promise

<br/>   

### Promise.all()

    作用
        用来判断给定的一组promise对象是否都为resolve状态
    
    参数
        iterator
            一个promise的iterator类型

    返回值
        promise

<br/>

### Promise.race()

    作用
        根据参数中最先改变状态的promise决定自身promise的状态
    
    参数
        iterator
            一个promise的iterator类型

    返回值
        promise

<br/>

### Promise.allSettled()

    作用
        当参数中的所有状态都改变，会返回promise
    
    参数
        iterator
            一个promise的iterator类型

    返回值
        promise

<br/>

### Promise.any()

    作用
        当参数中的任意一个promise变为fulfilled，就会返回fulfilled的promise，不然返回rejected的promise
    
    参数
        iterator
            一个promise的iterator类型

    返回值
        promise


<br/>

