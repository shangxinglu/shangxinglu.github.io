# Reflect

    Reflect是一个内置对象，不是函数对象，他提供了拦截JavaScript的方法

## 静态方法

    apply(target,thisArg,argumentList)
        作用
            通过指定的参数列表发起对目标函数的调用
        
        参数
            target
                目标函数
            
            thisArg
                被调用时的上下文
            
            argumentList
                参数列表，一个类数组对象

        约束
            target对象必须是一个函数

```JavaScript

    const {log} = console;
    const ctx = {name:'ctx'};

    function f1(){
        log(...arguments);
        log(this.name);
    }

    Reflect.apply(f1,ctx,['arg1']); // arg1  ctx
```


    construct(target,argumentList[,newTarget])
        作用
            行为有点像new操作符构造函数，可以设置调用函数的new.target
        
        参数
            target
                构造函数

            argumentList    
                参数列表
            
            newTarget 可选
                新创建对象的原型对象的constructor属性，默认target

        返回
            以target(如果newTarget存在，则为newTarget)函数为构造函数的对象实例

```JavaScript
    function f1(){
        this.name = 'f1';
        log(new.target);
    }

    log(Reflect.construct(f1,[])); // function f1{}  {name:'f1'}
```


    defineProperty(target,property,descriptor)
        作用
            与Object.defineProperty()基本相同，
            唯一区别就是会返回布尔值
        
    
    deleteProperty(target,property)
        作用
            用于删除属性
        
        参数
            target
                目标对象
            
            property
                删除的属性名
        
        返回
            布尔值
    

    get(target,property[,receiver])
        作用
            从一个对象中取属性值

        参数
            target
                目标对象
            
            property
                属性名
            
            receiver  可选
                如果对象的属性有getter，receiver就是getter调用时的this


```JavaScript
    const obj = {name:'obj'};
    const receiver = {name:'receiver'}

    log(Reflect.get(obj,'name',receiver)); // obj

    Object.defineProperty(obj,'name',{
        get(){
            return this.name;
        }
    })
    log(Reflect.get(obj,'name',receiver)); // receiver

```


    getOwnPropertyDescruptor(target,property)
        作用
            与Object.getOwnPropertyDescriptor()基本相同，
            如果不存在给定属性描述符返回undefined



    getPrototypeOf(target)
        作用
            与Object.getPrototypeOf()几乎一样


    has(target,property)
        作用
            与in操作符相同


    isExtensible(target)
        作用
            与Object.isExtensible()几乎相同，
            唯一不同，如果参数不为对象，Reflect.isExtensibl()会抛出异常
            Object.isExtensible()会进行强制转换

        
    ownKeys(target)
        作用
            获取一个由目标对象自身属性名组成的数组，
            等同于Object.getOwnPropertyNames().concat(
                Object.getPropertySymbols()
            )

    
    preventExtensions(target)
        作用
            设置对象为不可扩展

            与Object.preventExtensions()几乎相同，
            唯一不同，如果参数不为对象，Reflect.preventExtensions()会抛出异常
            Object.preventExtensions()会进行强制转换


    set(target,property[,receiver])
        作用
            设置对象的属性值
        
        参数
            target
                目标对象
            
            property
                属性名
            
            receiver  可选
                如果对象属性有setter，receiver为setter的this值


    setPrototypeOf(target,prototype)
        作用
            设置目标对象的原型，与Object.setPrototypeOf()一样

        

        
        
        
                


    