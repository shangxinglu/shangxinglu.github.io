# Proxy

## 作用

    Proxy就是一个代理，可以用于拦截和过滤对目标对象的访问，
    还可以在访问前后进行操作

## 语法
    new Proxy(target,handler)

## 参数

    target
        要使用Proxy包装的目标对象
    
    handler
        通常以一个函数作为属性的对象，
        各个属性中的函数用来定义了执行各个操作是的代理行为
    
## handler对象方法

    getPrototypeOf(target)
        作用
            当读取代理对象的原型是，会被调用
        
        参数
            target
                目标对象
            
        返回值
            必须是一个对象或null
        
        触发
            Object.getPrototypeOf()
            Reflect.getPrototypeOf()
            __proto__
            Object.prototype.isPrototypeOf()
            instanceof
        
```JavaScript
    const targetObj ={
        name:'target',
    };

    const p = new Proxy(targetObj,{
        getPrototypeOf(target){
            log('getPrototypeOf')
            return null;
        }
    })

    log(p instanceof Object); // getPrototypeOf

```


    setPrototypeOf(target,prototype)
        作用
            拦截Object.setPrototypeOf()
        
        参数
            target
                目标对象
            
            prototype
                对象新原型
            
        返回值
            布尔值，如果[[Prototype]]修改成功返回true，否则返回false

        触发
            Object.setPrototypeOf()  false将抛出TypeError错误
            Reflect.setPrototypeOf() 

```JavaScript
    const p = new Proxy(targetObj,{
        setPrototypeOf(target,prototype){
            return false;
        }
    })

    log(Object.setPrototypeOf(p,{})); // TypeError

```

    isExtensible(target)
        作用
            拦截对象的Object.isExtensible()方法
        
        参数
            target
                目标对象

        返回
            必须返回一个布尔值或可以转换为布尔的值
            Object.isExtensible(proxy)必须与Object.isExtensible(target)返回相同值，
            不然抛出TypeError

        触发
            Object.isExtensible()
            Reflect.isExtensible()

```JavaScript

    const p = new Proxy(targetObj,{
        isExtensible(target){
            return false;
        }
    })

    log(Object.isExtensible(p)); // TypeError
```
        

    preventExtensions(target)
        作用
            设置对Object.preventExtensions()的拦截

        参数
            目标对象
        
        返回值
            布尔值
            如果目标对象可扩展，那么只能返回false
        
        触发
            Object.preventExtensions()
            Reflect.preventExtensions()

```JavaScript
    const p = new Proxy(targetObj, {
        preventExtensions(target) {
            return true ;
        }
    })

    log(Object.preventExtensions(p)); // TypeError
```


    getOwnPropertyDescriptor(target,prop)
        作用
            对Object.getOwnPropertyDescriptor()的拦截
        
        参数
            target
                目标对象

            prop
                获取描述符的属性名
        
        返回
            必须返回一个object或undefined
            如果目标对象存在描述符必须则返回一个对象，且configurable必须相同

        触发
            Object.getOwnPropertyDescriptor()
            Reflect.getOwnPropertyDescriptor()


```JavaScript
    const p = new Proxy(targetObj, {
        getOwnPropertyDescriptor(target,prop) {
            log(prop);
            return {configurable:true} ;
        }
    })

    log(Object.getOwnPropertyDescriptor(p,'name')); // {value: undefined, writable: false, enumerable: false, configurable: true}
```

    defineProperty(target,property,descriptor)
        作用
            拦截Object.defineProperty()

        参数
            target
                目标对象

            property
                要定义的属性名
            
            descriptor
                描述符对象

        返回
            必须返回一个布尔值，表示操作成功与否
            如果目标对象不可扩展，将不能添加属性
            如果过属性在目标对象是可配置的，不能添加或修改为不可配置的属性
            严格模式下返回false会抛出异常

        触发
            Object.defineProperty()
            Reflect.defineProperty()

```JavaScript
const p = new Proxy(targetObj, {
    defineProperty(target, prop, desc) {
        // Object.defineProperty(...arguments)
        return true;
    }
})

log(Object.defineProperty(p, 'name', { configurable: false })); // TypeError

```


    has(target,prop)
        作用
            拦截in操作符的返回值

        参数
            target
                目标对象

            prop
                检查的属性名
        
        返回
            布尔值
            如果目标对象属性不可被配置，则不能被隐藏
            如果目标对象不可扩展，则不能被隐藏
        
        触发
            属性查询 foo in proxu
 
            继承属性查询 foo in Object.create(proxy)

            with检查

            Reflect.has()

```JavaScript
    const p = new Proxy(targetObj, {
        has(target, prop) {
            // Object.defineProperty(...arguments)
            return false;
        }
    })

    log('name' in p); // false
    Object.defineProperty(targetObj, 'name', { configurable: false })
    log('name' in p); // TypeError

```


    get(target,property,receiver)
        作用
            拦截对代理对象的读取属性操作
        
        参数
            target
                目标对象
            
            property
                被获取的属性名
            
            receiver
                proxy或继承proxy的对象
        
        返回
            任何值
            如果要访问的目标属性是不可写以及不可配置的，则返回的值必须与该目标的值相同
            如果目标属性没有配置访问方法，即get方法是undefined，则返回值必须是undefined


        触发
            访问属性
            访问原型链上的属性

```JavaScript
    const p = new Proxy(targetObj, {
        get() {
            return 888;
        }
    })

    log(p.name); // 888

    Object.defineProperty(targetObj, 'name', { configurable: false, writable: false, value: 'name' });
    log(p.name); // TYpeError
```


    set(target,property,value,receiver)
        作用
            拦截属性值设置操作

        参数
            target
                目标对象
            
            property
                将被设置的属性名或Symbol
            
            value
                新属性值

            receiver
                被调用的对象

        返回
            布尔值
            严格模式下，返回false抛出TypeError
            
            如果目标属性是不可写及不可配置的，则不能改变他的值
            如果目标属性没有配置存储方法，则不能设置他的值

        触发
            设置属性值
            设置原型链上的值
            Reflect.set()
        
```JavaScript
    const p = new Proxy(targetObj, {
        set() {
            return true;
        }
    })

    log(p.name = 888); // true

    Object.defineProperty(targetObj, 'name', { configurable: false, writable: false, value: 'name' });
    log(p.name = 888); // TYpeError
```


    deleteProperty(target,property)
        作用 
            拦截对对象属性的delete操作
        
        参数
            target
                目标对象

            property
                待删除的属性名
            
        返回值
            布尔值，表示是否删除成功
            如果目标属性不可配置，则不能被删除

        触发
            删除属性
            Reflect.deleteProperty()
        
```JavaScript
    const p = new Proxy(targetObj, {
        deleteProperty() {
            return true;
        }
    })

    Object.defineProperty(targetObj, 'name', { configurable: false });
    log(delete p.name); // TypeError
```


    ownKeys(target)
        作用
            拦截获取对象自身的可枚举属性的操作
        
        返回
            一个可枚举的数组

            数组元素类型只能是String或Symbol
            必须包含目标对象的所有不可配置的自有属性的key
            如果目标对象不可扩展，除了自有的不能有其他值

        触发
            Object.getOwnPropertyNames()
            Object.getOwnPropertySymbols()
            Object.keys()
            Reflect.ownKeys()

```JavaScript
    const p = new Proxy(targetObj, {
        ownKeys() {
            return [];
        }
    })

    Object.defineProperty(targetObj, 'name', { configurable: false });
    log(Object.keys(p)); // TypeError
```


    apply(target,thisArg,argumentList)
        作用
            拦截函数的调用
        
        参数
            target
                目标对象
            
            thisArg
                被调用时的上下文
            
            argumentList
                参数数组

        约束
            target必须是一个函数

        触发
            proxy()
            Function.prototype.apply()
            Function.prototype.call()
            Reflect.apply()

```JavaScript
    function targetFn(){
        log(this.name);
    };


    const p = new Proxy(targetFn,{
        apply(target,thisArg,args){
            const obj = {name:'proxy'};

            target.apply(obj);
        }
    })

    p(); // proxy
```


    construct(target,argumentList,newTarget)
        作用
            拦截new操作符
        
        参数
            target
                目标对象
            
            areumentList
                参数数组
            
            newTarget
                最初被调用的构造函数

        返回值
            必须返回一个对象

        触发
            new proxy
            Reflect.construct()

```JavaScript

    function A(){

    }

    const p = new Proxy(A,{
        construct(){
            return {};
        }
    })

    log(new p); // {}
```


## 可撤销代理

    Proxy.revocable(target,handler)
        作用
            创建一个可撤销代理
        
        返回
            一个对象，结构为{proxy,revoke}

                属性
                    proxy
                        代理对象
                    
                    revoke
                        撤销方法