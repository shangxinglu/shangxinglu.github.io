# Map与WeakMap

## Map

### 作用
    用来存储键值对，并且能记住插入顺序，任何值都能作为键和值

### 创建
    new Map([iterable])
        作用
            创建一个Map对象
        
        参数
            iterable 可选
                可迭代对象，元素为键值对，就是两个元素的数组

### 属性
    size
        Map对象中键值对的数量


### 方法
    clear()
        作用
            清除Map对象的所有键值对


    delete(key)
        作用    
            删除Map对象中的指定元素

        返回
            布尔值，存在且移除成功返回true，否则返回false


    entries()
        作用
            获取一个新的迭代器对象，包含[key,value]对，
            迭代器的迭代顺序与插入顺序相同

    forEach(callback([value][,key][,map])[,thisArg])
        作用
            按照插入顺序对Map中每个键/值对执行一次给定的函数
        
        参数
            callback
                每个元素需要执行的函数
                    参数    
                        value 可选
                            迭代的值

                        key 可选
                            迭代的键

                        map 可选
                            迭代的Map对象
            
            thisArg 可选
                函数执行时的this
        

    get(key)
        作用
            获取Map对象中的指定元素

    
    has(key)
        作用
            判断Map对象中是否存在指定元素

    
    keys()
        作用
            获取一个新的迭代器对象，包含Map对象中的每个key元素

    
    set(key,value)
        作用
            更新或添加指定键的值

        返回
            Map对象

    
    values()
        作用
            获取一个新的迭代器对象，包含Map对象的每个value值

    [@@iterator]()
        作用
            [Symbol.iterator]与entries是同一个函数


## WeakMap
            
        
### 与Map的区别
    1. WeakMap中的键必须是对象，Map中可以是任意值
    2. WeakMap中的键是弱引用



### 创建
    new WeakMap([iterable])
        作用
            创建一个WeakMap对象


        参数
            iterable 可选
                一个可迭代对象，元素是两个元素的数组，第一个元素必须是对象


### 方法
    delete(key)
        作用
            删除指定键的元素

        返回
            布尔值
    

    get(key)
        作用
            获取指定键的元素
        

    has(key)
        作用
            判断指定键是否存在

    
    set(key,value)
        作用
            更新或添加指定键的值
            

