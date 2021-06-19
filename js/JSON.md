# JSON

## JSON是什么
    JSON是一种轻量级数据格式，可以方便的表示复杂数据结构

## JSON的格式有哪几种

    字符串
        "A"
    
    数值
        1
    
    布尔值
        false
    
    null
        null
    
    对象
        {"k1":"k1"}

   
    数组
        ["A",1,false,{"k1":"k1"},[1]]


## 方法

    JSON.stringify(value,[,replace[,space]])

    作用
        将一个JSON对象或值转换成JSON字符串

    参数

        value
            要转换的值

        replace 可选
            如果是一个函数，则对象的每个属性都会经过该函数的转换和处理，函数有两个参数key和value，分别是键和值
            需要注意第一次执行时key是空字符串，value是需要序列化的对
            象，这个时候可以用来对序列化对象的已处理，第一次return的
            值才是真正的序列化对象

            如果是一个数组，则只有数组内的属性才会被序列化到JSON字符串

            如果未提供或为null，所有属性都会被序列化

        space  可选
            用来指定缩进的空格，用来美化输出
            
            如果是数字，数字范围要满足[0,10]
            如果是字符串，字符串将被用作空格，字符串最大长度为10

    以下是replace为函数的示例

```JavaScript
    const {log} = console;

    const obj = {
        name:123,
        age:24
    };

    const transObj = {
        name:value=>{
            return 'My name is'+ value;
        },
        age:value=>{
            log(value);

            return value/2;
        },
    }

    function trans(key,value){
        // log(arguments);
        if(key==='') {
            return {name:111111,age:1000};
            // return value;
        }
        // debugger
        log(key);
        return transObj[key](value);
    }

    const str = JSON.stringify(obj,trans);

    log(str); // {"name":"My name is111111","age":500}


```

    以下是replace为数组的示例

```JavaScript
    const obj = {
        name:123,
        age:24,
        sex:1,
    };

    const str = JSON.stringify(obj,['name']);

    log(str); // {"name":123}
```

    以下是space为数字的示例

```JavaScript

    const obj = {
        name:123,
        age:24,
        sex:1,
    };

    const str = JSON.stringify(obj,null,5);
    log(str);   // {
                //     "name": 123,
                //     "age": 24,
                //     "sex": 1
                // }

```

  以下是space为字符串的示例

```JavaScript

    const obj = {
        name:123,
        age:24,
        sex:1,
    };

    const str = JSON.stringify(obj,null,'1111');
    log(str);   // {
                //     1111"name": 123,
                //     1111"age": 24,
                //     1111"sex": 1
                // }

```


    JSON.parse(string[,reviver])

        作用
            将JSON字符串转换成JSON对象

        参数
            string
                JSON字符串，注意JSON字符串不允许逗号作为结尾

            reviver 可选
                是一个函数，将每个解析出来的属性值进行一次转化，
                属性解析顺序是从最里层开始的
                
                有两个参数key和value，属性名和属性值

                如果返回undefined则该属性会被忽略

                需要注意最后一次执行时key是空字符串，value是解析完成后的对象
                这个时候可以用来对对象的进行最终处理，最后一次return
                的值才解析完成返回对象



    以下是reviver的示例

```JavaScript
    const str = '{"name":"My name is111111","age":500}';

    const transObj = {
        name:value=>{
            const name = /is(.*)/.exec(value);
            return name[1];
        },
        age:value=>{

            return value*2;
        },
    }

    function trans(key,value){
        if(key===''){
            return value;
        }
        // log(key,value);
        return transObj[key]?.(value);
    }

    const obj = JSON.parse(str,trans)

    log(obj); // {name: "111111", age: 1000}

```