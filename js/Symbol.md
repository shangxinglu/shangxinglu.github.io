# Symbol

    Symbol是一种基本数据类型，Symbol函数会返回一个Symbol类型的值，不支持new语法

    每个从Symbol()返回的symbol值都是唯一的


## 创建Symbol值
    Symbol([description])
        参数    
            description 可选
                一个字符串，用来对symbol进行描述



## 全局Symbol

### 创建和获取
    Symbol.for(key)
        作用
            根据指定键在symbol注册表中获取symbol，
            如果不存在就新建一个并放入symbol注册表
        
        返回
            symbol

### 根据symbol获取键

    Symbol.keyFor(sym)
        作用
            根据symbol来获取全局symbol注册表中关联的键


## 内置Symbol值

### 作用
    内置的Symbol值指向语言内部使用的方法

### 属性

    Symbol.iterator
        指向对象的默认迭代器方法  

    Symbol.asyncIterator
        指向对象默认的异步迭代器方法  


    Symbol.match
        指向对一个字符串进行匹配的方法，
        同时也用来判断一个对象能否作为正则表达式使用,
        被String.prototype.match()使用

```JavaScript
    const str = 'abcdefg';

    const reg = {
        [Symbol.match](value){
            
            return value+1;
        }
    }

    log(str.match(reg)) // abcdefg1
```


    Symbol.replace
        指定了当一个字符串替换所匹配字符串时调用的方法
        String.prototype.replace()会调用这个方法
    
```JavaScript
    const str = 'abcd1234';

    const obj = {
        [Symbol.replace](){
            return 'cs';
        }
    }

    log(str.replace(obj,'b')); // cs
```


    Symbol.search
        指定了一个搜索方法，当字符串与正则表达式相匹配会调用
        String.prototype.search()会调用这个方法

```JavaScript
    const str = 'qqqqqqqqqqqqqq';
    const obj = {
        [Symbol.search](){
            log(...arguments)

            return 9;
        }
    }

    log(str.search(obj)) // 9
```


    Symbol.split
        指向一个正则表达式的索引处分隔字符串的方法
        String.prototype.split()会调用
    
```JavaScript
    const str = 'qqqqqqqqqqqqqq';
    const obj = {
        [Symbol.split](){
            return [];
        }
    }
    
    log(str.split(obj)) // []
 ```
            


    Symbol.hasInstance
        指向一个用于判断对象是否是某个构造器的实例的方法
        可以用来自定义instanceof操作符在某个类上的行为

```JavaScript
  class A{};
        const obj = Object.create(null);
        obj[Symbol.hasInstance] = function(target){
        if([A].includes(target)){
            return true;
        }
        return false;
    }

    console.log(A instanceof obj); // true
```


    Symbol.isConcatSpreadable
        是一个布尔值，用于配置作为Array.prototype.concat()方法的参数时
        是否展开其数组元素，默认展开

```JavaScript
    const a = [1,2,3];
    const b = [4,5,6];

    log(a.concat(b)) // [1,2,3,4,5,6]

    b[Symbol.isConcatSpreadable] = false;
    log(a.concat(b)) // [1,2,3,[4,5,6]]
```


    Symbol.unscopables
        是一个对象，用于排除对象属性与with环境的绑定

```JavaScript
    const name = 'global',
    age = 'global';
    const obj = {
        name:'obj',
        age:12,
        [Symbol.unscopables]:{
            name:true,
            age:false,
        }
    };

    with(obj){
        log(name) // global
        log(age) // 12
    }
```


    Symbol.species
        是一个对象的函数值属性，被构造函数用来创建子类对象
        允许子类对象覆盖默认的构造函数
    
```JavaScript
    class A extends Array{    
        static get [Symbol.species](){
            return Array;
        }

    
    }

    const a = new A;

    const b = a.map(x=>x); // map获取调用a的[Symbol.species]创建一个新的对象
    log(b instanceof Array) // true
```


    Symbol.toPrimitive
        是对象的函数值属性，当一个对象转换为对应原始值时，会调用此函数

```JavaScript
    const obj = {
        [Symbol.toPrimitive](hint){
            log(hint)
            return 1;
        }
    }

    log(''+obj); // default 1
    log(1+obj); // number 2
```


    Symbol.toStringTag
        用于对象默认描述的字符串值
        object.prototype.toString()会调用

```JavaScript
    const obj = {
        get [Symbol.toStringTag](){
            return 'Custom'
        }
    }

    log(Object.prototype.toString.call(obj)); // [Object Custom]
```



