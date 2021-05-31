# Iterator
    
    Iterator也就是迭代器，迭代器是通过next方法实现迭代协议的对象

## 迭代协议有什么

    迭代协议其实包含了两个协议，一个可迭代协议，还有一个迭代器协议

### 可迭代协议

    可迭代协议要求两种能力:
        支持迭代的自我识别能力
        创建实现Iterator接口的对象能力
    
    自我识别的表现就是通过检测@@Symbol.iterator属性来判断
    @@Symbol.iterator引用一个函数，函数调用后必须返回一个新的迭代器

### 迭代器协议

    迭代器是一种一次性使用的对象，迭代器对象必须要有next方法，该方法的返回值必须是IteratorResult对象(就是含有done和value属性的对象)


```JavaScript

// 迭代器创建
const createIterator = function(){
    let done = false,
    value = 0;
    
    return {
        next(){
            if(value<10){
                value++
            } else {
                done = true;
            }
            return {
                done,
                value,
            }
        }
    }
   
}

const obj = {
    [Symbol.iterator]:createIterator,
}

for(let item of obj){
    console.log(item); // 1 2 3 4 5 6 7 8 9 10
}
```



## 异步迭代器

    异步迭代器用来迭代异步操作，会在异步操作完成后再返回

### 异步迭代器与迭代器的不同
    
    1. 异步迭代使用的不是@@Symbol.iterator而是@@Symbol.asyncIterator

    2. 异步迭代器调用next方法后返回可以是一个promise，但这个promise要返回
    IteratorResult对象，也可以直接返回IteratorResult对象，相当于在执行体内部先给
    item包了一层async，利用await达到了等待和包装的效果，会返回一个promise

所以iterator的代码也可以直接用在asyncIterator中

```JavaScript

    // 创建异步迭代器
    function createAsyncIterator() {
        let done = false,
            value = 0;

        return {
            async next() {
                if (value < 10) {
                    value++
                } else {
                    done = true;
                }
                // return await new Promise((resolve) => {
                //     resolve({
                //         done,
                //         value,
                //     });

                // });

            
                return {
                    done,
                    value,
                }
            }
        }
    }

    const obj = {
        [Symbol.asyncIterator]: createAsyncIterator,
    }


    async function fun1(){
        for await (let item of obj) {
            console.log(item);
        }

        // item 赋值过程
        // let iterator = obj[Symbol.asyncIterator]();
        // let {value:item} = await Promise.resolve(iterator.next());
        // console.log(item);
    }

   

    fun1();  // 1 2 3 4 5 6 7 8 9 10
```