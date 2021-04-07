# 实现一个Promise

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