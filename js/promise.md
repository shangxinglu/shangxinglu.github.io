# 实现一个Promise

```javascript
class MyPromise {

    resolveCall = null; // 解决回调
    rejectCall = null; // 拒绝回调

    constructor(fun){
        if (typeof fun === 'function'){
            console.log(this);
            const resolve = this.resolve.bind(this),
            reject = this.reject.bind(this);
            fun(resolve,reject);
        } else {
            throw new Error('argument must is function');
        }

    }

    /**
     * 解决
     */
    resolve(){
        console.log('resolve',this);

        setTimeout(()=>{
            // console.log(this)
            this.resolveCall&&this.resolveCall();
        },0);
    }

    /**
     * 拒绝
     */
    reject(){
        console.log('reject');
       
    }

    then(resolveCall,rejectCall){
        if (resolveCall){
            this.resolveCall = resolveCall;
            console.log(this.resolveCall)

        }
    }

}



const p1 = new MyPromise((resolve,reject)=>{
    resolve();
}).then(()=>{
    console.log('then');
});


```