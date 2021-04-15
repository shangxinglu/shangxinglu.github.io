# Promise

## Promise是什么

    Promise是一种异步编程的解决方案，同时JavaScript通过Promise将任务队列管理交到了用户手中

<br/>

## Promise的使用
    
    Promise有3中状态: penging(待定)、fulfilled(已兑现)、rejected(已拒绝)，一旦当状态变为fulfilled或rejected，就不会在发生变化

    Promise构造函数有一个参数，是一个函数，这个函数有两个参数resolve和reject

    resolve:
        是一个函数，将Promise对象的状态改为fulfilled

    reject:
        是一个函数，将Promise对象的状态改为rejected

```javascript
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

    then方法有两个参数，都是函数，分别在状态未fulfilled和rejected时调用

```javascript
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
    then调用后会会返回一个新的Promise，他的两个参数方法执行会改变这个Promsie的状态和返回值

```javascript
    let res = null;
    const p = new Promise(resolve=>{
        res = resolve;
    });

    const p1 = p.then(()=>{});

    console.log(p===p1); // false

    console.log(p1); // Promise{pending}

    res();

    console.log(p); // Promise{fulfilled}
    console.log(p1); // Promise{pending}

```

    当then中的函数接受的参数为一个Promise对象，会有几种情况

    Promise(后面简称p1)为fulfilled状态，then返回的Promise对象(后面简称p2)的状态会变为fulfilled，p1传给then回调函数的参数值作为p2的then回调函数的参数值

```javascript
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

    Promise(后面简称p1)为rejected状态，then返回的Promise对象(后面简称p2)的状态会变为rejected，p1传给then回调函数的参数值作为p2的then回调函数的参数值

```javascript
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

    Promise(后面简称p1)为pending状态，then返回的Promise对象(后面简称p2)的状态会保持pending，当p1发生状态改变，p1的传给then回调函数的参数值传给p2的then回调函数的参数，变为相同状态

```javascript
    let res = null;
    const p1= new Promise((resolve,reject)=>{
        res = resolve;
    });

    const p2= new Promise((resolve,reject)=>{
        resolve(p1);
    });

    p2.then(console.log); // 200

    res(200);

```
<br/>

### catch

<br/>

### Promise.resolve()

<br/>

### Promise.reject()

<br/>

### Promise.all()

<br/>

### Promise.race()


<br/>

## Promise运行过程

<br/>

## 实现Promise

```javascript
class MyPromise {

    resolveQueue = []; // 解决回调
    rejectQueue = []; // 拒绝回调

    status = 'pending'; // 状态 pending:等待    resolve:已解决    reject:已拒绝

    constructor(fun) {
        if (typeof fun === 'function') {

            const resolve = this.resolve.bind(this),
                reject = this.reject.bind(this);
            fun(resolve, reject);
        } else {
            throw new Error('argument must is function');
        }

    }

    /**
     * 返回一个resolve状态的MyPromise
     */
    static resolve(result) {
        console.log('static resolve');
        if (result instanceof MyPromise) {
            return result;
        } else {

            const myPromise = new MyPromise((resolve) => {
                resolve();
            });

            myPromise._resolve = this.resolveQueue;

            return myPromise;
        }
    }

    /**
     * @description 设置已解决回调队列
     * 
     * @param then {Array} 回调队列
     * 
     */
    set _resolve(then) {
        console.log('_resolve',then);
        this.rejectQueue = then;
    }

    /**
     * @description 设置拒绝回调队列
     * 
     * @param then {Array} 回调队列
     * 
     */
    set _reject(then) {
        this.rejectQueue = then;
    }

    /**
     * 解决
     */
    resolve() {
        // console.log('resolve',this);

        setTimeout(() => {
            // console.log(this)
            this.resolveQueue.length && MyPromise.resolve((this.resolveQueue.shift())());
        }, 0);
    }

    /**
     * 拒绝
     */
    reject() {
        // console.log('reject');
        setTimeout(() => {
            // console.log(this)
            this.rejectQueue && (this.rejectQueue.shift())();
        }, 0);
    }

    /**
     * 回调收集
     */
    then(resolveCall, rejectCall) {
        console.log('回调收集');
        if (resolveCall) {
            this.resolveQueue.push(resolveCall);
            // console.log(this.resolveCall)
        }

        if (rejectCall) {
            this.rejectQueue.push(rejectCall);
        }
        console.log(this.resolveQueue)

        return this;
    }

}



const p1 = new MyPromise((resolve, reject) => {
    resolve();
    // reject();
}).then(() => {
    console.log('then');
}, () => {
    console.log('rejectCall');
}).then(() => {
    console.log('then1');

});



```