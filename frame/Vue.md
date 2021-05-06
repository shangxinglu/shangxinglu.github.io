# Vue

## Vue是什么

    Vue是一个前端框架，有个一个响应系统，开发者可以省去对DOM节点的操作，专注视图和逻辑，提升开发效率

## Vue可以大致分为几个部分

    编译器
    响应对象
    依赖收集
    虚拟DOM

## Object是怎么实现响应的

    Object响应的核心在于Object.defineProperty，下面是个简单示例

```javascript
    const obj = {};
    let k1 = '';
    Object.defineProperty(obj, 'k1', {
        configurable: true,
        enumerable: true,
        set(val) {
            if (k1 !== val) {
                console.log('reactive');
                k1 = val;
            }
        },
    });

    obj.k1 =100; // reavtive

```


## 依赖怎么收集

    依赖在存储描述符的get中收集，通过set去触发依赖，对象的每个属性都可以有依赖，所以需要将对应属性的依赖收集到对应的对应的地方


```javascript
    function defineReactive(obj,key,val){
        const dep = [];

        Object.defineProperty(obj,key,{
            configurable:true,
            enumerable:true,

            set(newVal){
                if(val===newVal){
                    return;
                }

                val = newVal;
                for(let item of dep){
                    item();
                }
            },

            get(){
                if(dep.indexOf(window.target)===-1){
                    dep.push(window.target); // window.target是收集的依赖
                }
                return val;
            }
        });
    }

    window.target = ()=>{
        console.log('run dep');
    }

    const obj = {};

    defineReactive(obj,'k1','k1');

    obj.k1; // 触发依赖收集

    obj.k1 = 'k2'; // run dep

    console.log(obj.k1); // k2

```

## 