# Class

## Class的作用是什么

    class关键字是ES6提出的，是一个语法糖，可以用写类的方式来实现原型继承
    

## Class的使用

### 简单使用

    class的实例化中，运算符()是作用是传值参数表，并没有调用函数的意义，所以不需要给构造函数传递参数表时可以不加

```javascript
    class A{
    k1 = 'k1';

    constructor(){
        this.k2 = 'k2';
    }

    getVal(){
        return this.k2;
    }
}

const a = new A;

console.log(a.k1,a.getVal()); // k1 k2

```

    

<br/>

## Class继承

    继承使用extends关键字，在extends后面是要继承的父类parentClass，这个父类的prototype必须是Object或者null，可以使用表达式的返回值作为父类
    
```javascript
    
    class A{
        getSupName(){
            return 'A';
        }
    }

    function fun1(){
        return A;
    }

    class B extends fun1(){

    }

    const b = new B;

    console.log(b.getSupName()); // A
```


<br/>

## Super的作用

    



## Class实例化过程

<br/>


## Class与Prototype继承的区别