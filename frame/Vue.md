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

```JavaScript
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


```JavaScript
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

## 依赖指的是什么

    依赖就是当响应数据发生变化，需要通知用到该数据的地方，比如watch，模板，Vue有个Watcher用来负责依赖的收集

    下面是一个初步的实现

```JavaScript

    /**
     * @description 将字路径解析成数组，解析成功返回一个读取指定对象该路径的属性
     * 
     * @param {String} path 属性路径
     * 
     * @returns {undefined|Function}
     */
    const pathReg = /[^\w.]/;

    function parsePath(path) {
        // 过滤格式错误的路径
        if (pathReg.test(path)) return;

        const pathArr = path.split('.');

        return function (obj) {

            for (let item of pathArr) {
                if (!obj) return;

                obj = obj[item];
            }

            return obj;

        }
    };

    function setTarget(target){
        Dep.target = target;
    }

    class Wathcer {

        /**
            * @param {Object} 响应数据对象
            * @param {String|Function} expOrFn 需要收集依赖的属性路径或者获取该属性的方法
            * @param {Function} cb 回调函数
            */
        constructor(vm,expOrFn, cb) {
            this.vm = vm,
            this.cb = cb;

            let getter = null;

            if(typeof expOrFn === 'function'){
                getter = expOrFn;
            } else {
                getter = parsePath(expOrFn);
                if(!getter){
                    throw new Error('expOrFn');
                }
            }
            this.getter = getter;

            this.get();
        }

        // 触发依赖的收集
        get() {
            setTarget(this.cb);

            this.getter(this.vm);

            setTarget(null);

        }
    }
```

<br/>

## Obsever的作用
    逐个添加响应数据太麻烦，Observer会把一个对象被的所有属性都转成响应式数据

<br/>


## 目前为止对Vue中的响应式的流程的理解

    我们在data中定义数据，Vue会用Observer进行转换，转成响应式数据，当使用Watcher定义依赖时，会自动读取一次，也就是会触发对应属性的getter，将依赖收集到了Dep中，在设置属性时便会触依赖

<br/>

## ObserverArray如何实现
    数组不能直接用存储描述符实现依赖的收集和监听，当数组方法改变数组自身时，并不会触发setter，所以需要在调用方法上进行考虑

    我们可以通过改造数组实例的原型方法来实现，在数组原型和数组之间再加个原型，然后在该原型上加入会改变数组自身的同名方法，在利用原型链的访问规则，使得数组实例依然可以使用其他原生的方法

    现在可以通过新增的原型监听到数组的变化，但是还需要依赖的收集和调用，依赖的调用可以在改造的方法中，至于依赖的收集那应该还是在Observer中实现

    先在碰到一个问题就是数组收集的依赖在原型方法中访问不到，所以只能改变依赖的存储位置

<br/>

## __ob__的作用

    __ob__用来存储Observer的实例，是的数组原型方法中能访问到依赖，还有一个就是用来判断对象是否是响应式的的，所以对象中也有__ob__属性

    __ob__在扩展方法中有重要作用，使开发者可以在defineReactive外访问到dep

<br/>

## watcher收集和执行回调的逻辑

    1.首先watcher将自身实例装载到Dep.target
    2.dep通过Dep.target将自身实例传到watcher(为了watcher能够操作dep)
    3.watcher将自身实例传递存放到dep(为了能够通知依赖发生变化)
    4.当依赖发生变化dep会通过存放的watcher执行回调
    
<br/>

## 深度监听的实现关键
    其实很简单，在依赖挂载后，去触发内部属性的getter,这样每个属性都能添加这个依赖，也就实现了深度监听


<br/>

## 虚拟DOM的作用
    虚拟DOM是对DOM映射的一种结构，通过虚拟DOM可以生成对称对应的DOM，有了虚拟DOM，就可以在生成DOM前进行vnode的对比，尽可能减少对DOM的操作，因为DOM操作还是比较贵的，所以减少DOM操作对性能提升的效果还是相当不错的



## VNode

    作用
        有了VNode就可以创建各种类型的节点，然后可以组成VDOM树，利用patch进行比对

    类型
        VNode有三种类型
        1.元素节点
        2.文本节点
        3.注释节点


## patch逻辑

    oldvnode是否存在
        不存在 
            使用vnode创建节点并插入视图
        
    oldvnode与vnode是否是同一节点
        不是
            为vnode创建node，添加到视图
            将oldvnode对应的node移除

    更新节点
        oldvnode与vnode是否完全相等
            是
                退出程序

        oldvnode与vnode是否是静态节点
            是
                退出程序

        vnode是否是有text属性
            是
                vnode的文本和oldvnode的文本不同
                    是
                        将vnode的text替换oldvnode的text
            

        vnode的子节点和oldvnode的子节点都存在
            是
                vnode的子节点和oldvnode的子节点不相同
                    是
                        更新子节点

        只有vnode的子节点存在
            是
                oldvnode有文本
                    没有
                        把vnode的子节点添加到oldvnode对应node的中
                    
                    有
                        清空oldVnode对应node中的文本
                        把vnode的子节点添加到oldvnode对应node的中



        只有oldvnode的子节点存在
            是
                把oldvnode对应node的子节点清空


         oldvnode有文本
            是
                把oldvnode对应node的文本清空




## HTML解析器
    利用正则对字符串模板解析，根据对应的匹配内容，触发钩子函数，
    然后在对应的钩子函数创建对应的AST的节点