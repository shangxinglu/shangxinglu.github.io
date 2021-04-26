# Object

## Object的理解

    Object(对象)从语义上看，可以理解为一组组属性的集合，每个对象在内部会维护一张自有属性的属性表

<br/>

## Object属性的性质有什么

    Object属性性质有一下几种

        
        1.Witable
            可写性

        2.Enumerable
            可枚举性
        
        3.Configurable
            可配置性


<br/>

## Object属性的性质如何更改

    属性的性质是由属性描述符来定义的，所以可以通过属性描述符来进行对属性性质的更改，属性描述符也是一个对象
    
    属性描述符有两种类型

        1.数据属性描述符

        2.存储属性描述符

<br/>

### 数据属性描述符的属性有哪些

    数据属性描述符的属性有4个

         1.writable
            属性是否可重写

```javascript
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

```javascript
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

```javascript
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

```javascript
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

```javascript
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
        会会隐式创建一个数据描述符，writable、enumerable、configurable都是true

```javascript
    const obj = {};

    obj.k1 = 'k1';

    console.log(Object.getOwnPropertyDescriptor(obj,'k1')); // {value: "k1", writable: true, enumerable: true, configurable: true}
```

    属性是对象自有的，如果writable为true就会更新描述符的value，不然不会有任何操作，严格模式下会抛出异常

```javascript
    const obj = {k1:'k1'};

    obj.k1 = 'k2';

    console.log(Object.getOwnPropertyDescriptor(obj,'k1')); // {value: "k2", writable: true, enumerable: true, configurable: true}

```
<br/>

## Object自有属性表如何操作

