# Object

## Object的理解

    Object(对象)从语义上看，可以理解为一组组属性的集合，每个对象在内部
    会维护一张自有属性的属性表

<br/>

## Object属性的性质有什么

    Object属性性质有以下几种

        
        1.Witable
            可写性

        2.Enumerable
            可枚举性
        
        3.Configurable
            可配置性


<br/>

## Object属性的性质如何更改

    属性的性质是由属性描述符来定义的，所以可以通过属性描述符来进行对属
    性性质的更改，属性描述符也是一个对象
    
    属性描述符有两种类型

        1.数据属性描述符

        2.存储属性描述符

<br/>

### 数据属性描述符的属性有哪些

    数据属性描述符的属性有4个

         1.writable
            属性是否可重写

```JavaScript
    const obj = {};

    Object.defineProperty(obj,'k1',{
        value:'origin',
        writable:false,
    });

    console.log(obj); // {k1: "origin"}


    try{
        obj.k1 = 'change';
    }catch(e){
        console.log('error'); // error
    }

```

        2.enumerable
            属性是否可列举

```JavaScript
    const obj = {k2:'k2'};

    Object.defineProperty(obj,'k1',{
    value:'k1' ,
    enumerable:false,
    });

    for(let i in obj){
        console.log(i);
    }

    // k2

```
        
        3.configurable
            属性是否可重新配置

```JavaScript
    const obj = {};

    Object.defineProperty(obj,'k1',{
    value:'k1' ,
    configurable:false,
    });
    try{
        Object.defineProperty(obj,'k1',{
            value:'k1' ,
            writable:true,
            configurable:true,
        });
    } catch(e){
        console.log('不可重新定义描述符'); // 不可重新定义描述符
    }

```

        
        4.value
            属性的值

<br/>

### 存储属性描述符的属性有哪些

    存储属性的描述符属性也是有4个

        1.get
            这是一个取值函数

```JavaScript
    const obj = {};

    Object.defineProperty(obj,'k1',{
        get(){
            console.log('get k1');
            return 'k1'
        }
    });

    obj.k1; //get k1

```
        2.set
            这是一个置值函数

```JavaScript
    const obj = {};

    Object.defineProperty(obj,'k1',{
        set(value){
            this._k1 = value;
            console.log('set k1');
        }
    });

    obj.k1 = 1; //set k1
    console.log(obj); // {_k1: 1}
```

        3.enumerable
            属性是否可枚举

        4.configurable
            属性是否可配置

<br/>

### Object属性的描述符在某些场景下的默认值是啥

    使用Object.defineProperty定义描述符时的默认值
        数据描述符
            value:undefined
            writable:false
            enumberable:false
            configurable:false
        
        存取描述符
            set:undefined
            get:undefined
            enumberable:false
            configurable:false
        
    使用字面量声明时的默认值
        数据描述符 
            writable:true
            enumerable:true
            configurable:true

        已读取器方式声明

            enumerable:true
            configurable:true

<br/>

## 对Object的属性进行赋值时不同场景下会有什么不同

    当一个属性不存在时
        会隐式创建一个数据描述符，writable、enumerable、configurable
        都是true

```JavaScript
    const obj = {};

    obj.k1 = 'k1';

    console.log(Object.getOwnPropertyDescriptor(obj,'k1')); // {value: "k1", writable: true, enumerable: true, configurable: true}
```

    属性是对象自有的，如果writable为true就会更新描述符的value，不然不
    会有任何操作，严格模式下会抛出异常

```JavaScript
    const obj = {k1:'k1'};

    obj.k1 = 'k2';

    console.log(Object.getOwnPropertyDescriptor(obj,'k1')); // {value: "k2", writable: true, enumerable: true, configurable: true}

```
    如果属性是原型链上的可写属性，自身没有这个属性，会在自有属性表中创
    建一个默认的数据描述符，与原型链上属性的描述符无关

```JavaScript

    const pro = {k1:'p1'};

    Object.defineProperty(pro,'k1',{
        value:'p2',
    })

    const obj = Object.create(pro);

    obj.k1 = 'k1';

    console.log(Object.getOwnPropertyDescriptor(obj,'k1')); // {value: "k2", writable: true, enumerable: true, configurable: true}


```

    如果一个属性使用的是存取描述符，是不会新建属性描述符的，即使存在原
    型链上，也会调用存取器而不会在自有属性表新建

```JavaScript
    const pro ={};

    Object.defineProperty(pro,'k1',{
        set(val){
            console.log('set k1')
            this._k1 = val;
        },

        get(){
            return this._k1;
        }
    });

    const obj = Object.create(pro);

    obj.k1 = 'k1'; // set k1

    console.log(Object.getOwnPropertyDescriptor(obj,'k1')); // undefined


```
<br/>

## Object自有属性表如何操作

### [[extensible]]

    [[extensible]]是在对象内部的一个属性，用来影响自有属性表的相关行
    为，该属性的默认值是true，表明对象是可以被添加和删除属性的

<br/>

    属性表有两种类型的操作方法

        状态的维护
            Object.preventExtensions(obj)
                禁止表 add

            Object.seal(obj)
                禁止表 add/delete

            Object.freeze(obj)
                禁止表add/delete/update

        状态检查

            Object.isExtensible(obj)
                表是否可扩展

            Object.isSealed(obj)
                表是否密封

            Object.isFrozen(obj)
                表是否冻结


### Object.preventExtensions(obj)

    会将表的[[extensible]]属性设置为false，意味着表不能再添加属性

```JavaScript
    const obj = {};

    Object.preventExtensions(obj)

    try{
        obj.k1 = 'k1';

    } catch(e){
        console.log(obj); // {}
    }
```

### Object.seal(obj)

    会将表的[[extensible]]属性设置为false，同时将所有的自有属性的
    configurable改为false

```JavaScript

    const obj = {k1:'k1'};

    Object.seal(obj)

    try{
    delete obj.k1;

    } catch(e){
        console.log(Object.getOwnPropertyDescriptor(obj,'k1')); // {value: "k1", writable: true, enumerable: true, configurable: false}
    }

```

### Object.freeze(obj)

    会将表的[[extensible]]属性设置为false，同时将所有的自有属性的
    writable、configurable改为false

```JavaScript
    const obj = {k1:'k1'};

    Object.freeze(obj)

    try{
    obj.k1 = 'k2';

    } catch(e){
        console.log(Object.getOwnPropertyDescriptor(obj,'k1')); // {value: "k1", writable: false, enumerable: true, configurable: false}
    }

```

