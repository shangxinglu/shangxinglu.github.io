# RegExp

## 正则表达式的作用
    用来匹配字符串中字符的组合模式

## 正则表达式的创建

    有两种方式
        字面量  
            /aa/
            字面量的性能比构造函数好
    
        构造函数
            new RegExp('aa')
            构造函数相比字面量的优势是具有动态性



## 断言
    断言的组成之一是边界，对于文本、词或模式，边界可以用来表明它们的起始或终止部分

## 断言的类型

    边界类断言

        ^
            匹配输入的开头

        &
            匹配输入的结束
    
    向前断言
        x(?=y)
            只有当x被y跟随时，才会匹配x

```JavaScript
    const reg1 = /aaa(?=kkk)/;

    const text1 = 'aaak',
    text2 = 'aaaaakkkkk';

    console.log(reg1.test(text1)); // false
    console.log(reg1.test(text2)); // true

```

    向前否定断言
        x(?!y)
            x没有被y跟随的时候，才会匹配

```JavaScript

    const reg1 = /aaa(?!kkk)/;

    const text1 = 'aaak',
    text2 = 'aaakkkkk';

    console.log(reg1.test(text1)); // true
    console.log(reg1.test(text2)); // false

```

    向后断言
        (?<=y)x
        x跟随y的时候，才会被匹配


```JavaScript

    const reg1 = /(?<=aaa)kkk/;

    const text1 = 'aaaakkk',
    text2 = 'aakkkkk';

    console.log(reg1.test(text1)); // true
    console.log(reg1.test(text2)); // false

```

    向后否定断言
        (?<!y)x

```JavaScript

    const reg1 = /(?<!aaa)kkk/;

    const text1 = 'aaaakkk',
    text2 = 'aakkkkk';

    console.log(reg1.test(text1)); // false
    console.log(reg1.test(text2)); // true

```


## 字符类
    字符用来区分不同类型的字符

    .
        用来匹配除了行终止符之外的任意单个字符

        但是在字符集内.会失去特殊意义，只是单纯的.符号
    

```JavaScript

    const regexp1 = /.a/,
    regexp2 = /[.]a/;

    const text1 = 'aa',
        text2 = 'ba',
        text3 = '.a';

    console.log(regexp1.test(text1)); // true
    console.log(regexp1.test(text2)); // true
    console.log(regexp2.test(text1)); // false
    console.log(regexp2.test(text3)); // true

```

    \d
        匹配任何数字
        相当于[0-9]


    \D
        匹配任何非数字的字符
        相当于[^0-9]

    \w
        匹配基本拉丁字母的任何字母数字下划线
        相当于[a-zA-Z0-9_]
    
    \W
        匹配不是来自基本拉字母的字符
        相当于[^a-zA-Z0-9_]


    \s
        匹配单个空白字符，包括空格、制表符、换页、换行和其他的
        Unicode空格

    
    \S
        匹配单个非空白字符

    
    \t
        匹配水平制表符

    
    \r 
        匹配回车
    
    
    \n 
        匹配换行

    
    \v
        匹配垂直制表符
    
    
    \f
        匹配换页
    
    
    [\b]
        匹配退格
    
    \0
        匹配一个NUl字符


## 组和范围

    x|y
        匹配x或y中的任意字符


```JavaScript

    const regexp = /name|age/;

    const text1 = 'my name is sxl',
    text2 = 'my age is 99';
    console.log(regexp.test(text1)); // true
    console.log(regexp.test(text2)); // true

```

    [xyz]
    [x-z]


    [^abc]
    [^a-c]

    
    (x)
        捕获组,匹配x并记住匹配项

    \n
        n是一个正整数，是对括号匹配的第n个匹配项的反引用

```JavaScript
    const regexp = /name(\d)(\d)\2/,

    text1 = 'name233',
    text2 = 'name232';

    log(regexp.test(text1)); // true
    log(regexp.test(text2)); // false
```
    
    (?<Name>x)
        具名捕获组，匹配x并将其存储在返回匹配项的groups对象中，
        Name就是键名
    

    (?:x)
        非组捕获，匹配x，但不记得匹配
 



## 量词

    x*
        匹配x项0次或多次

    x+
        匹配x项1次或多次
    
    x?
        匹配x项0次或1次
    
    x{n}
        匹配x项n次
    
    x{n,}
        匹配x项n次或n次以上
    
    x{n,m}
        匹配x项最少n次最多m次
    
    默认情况下*和+这样的量词是贪婪的，会试图匹配更多字符串,
    ?后面的字符会使量词变的非贪婪


```JavaScript
    const regex1 = /\d+?/,
    regex2 = /\d+/;

    const text1 = '1111111';

    log(regex1.exec(text1)[0]); // 1
log(regex2.exec(text1)[0]); // 1111111
```