# Set与WeakSet

## Set

### 作用
    Set对象用来存储任何类型的唯一值，无论是原始值或者是对象引用

    在Set中NaN之间被视为相同值

### 创建
    new Set([iterable])
        作用
            创建一个Set对象
        
        参数
            iterable 可选
                可迭代对象

### 属性

    size
        Set对象中值的个数
    
### 方法

    add(value)
        作用
            在Set对象末尾添加一个指定值

        返回
            Set对象本省


    clear()
        作用
            清空Set对象中的所有元素


    delete(value)
        作用
            删除Set对象中的指定元素

        返回
            布尔值，表示操作是否成功


    entries()
        作用
            获取一个新的迭代器，迭代器的元素是[value,value]形式的数组，
            value是集合对象中的每个元素，迭代顺序就是对象中元素的插入顺序


    forEach(callFn[,thisArg])
        作用
            按照集合中元素的插入顺序，依次执行提供的回调函数

        参数
            callFn
                每个元素执行的回调函数
                    参数
                        currentValue 可选
                            正在被操作的元素

                        currentKey 可选
                            正在被操作的元素

                        set 可选
                            当前集合对象

            thisArg 可选
                回调函数执行过程中的this值


    has(value)
        作用
            判断指定元素是否在集合对象中存在
        
        返回
            布尔值
            

    keys()  
        作用
            是values()方法的别名


    values()
        作用
            获取一个包含Set对象每个元素的新的迭代器对象

    
    [@@iterator]()
        作用
            [Symbol.iterator]与values是同一个函数




## WeakSet

### 作用
    用来存储弱引用的对象

### 与Set的区别
    1. 与Set相比，WeakSet只能是对象的集合，而不能是任意值
    2. WeakSet集合中的对象是弱引用，不会算入垃圾回收机制中的引用次数，
       所以当WeakSet中的对象没有被其他引用，会被垃圾回收，因此WeakSet是不可枚举的

### 创建
    new WeakSet([iterable])
        作用
            创建一个WeakSet对象
        
        参数
            iterable 可选
                可迭代对象，null被认为是undefined
        
### 方法
    add(value)
        作用
            在WebSet对象的最后添加一个新的对象
        
        返回
            WeakSet对象


    delete(value)
        作用
            从WebSet对象中删除指定元素
        
        返回
            布尔值

    
    has(value)
        作用    
            判断指定元素是否存在
        
        返回
            布尔值
