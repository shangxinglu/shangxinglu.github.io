# Prototype

## Prototype是什么

    原型(Prototype)是一个对象，JavaScript通过原型来实现对象之间的继承，原型在构造器用于生成实例的模板


<br/>


## Prototype chain原型链是什么

    原型链就是对象的父类和祖先类的原型所形成的的、可以上溯访问的链表

<br/>

## prototype和[[prototype]]的区别

    prototype是函数特有的，[[prototype]]是对象的属性

    含义上prototype是构造器生成实例的模板，[[prototype]]指向构造函数的prototype，当然可以在使用中改变[[prototype]]的引用对象

    还有就是原型链查找是找的[[prototype]]，而不是prototype


<br/>



## Prototype的继承

```JavaScript

    function A(){

    }

    A.prototype.getSupName = function(){
        console.log('A')
        return 'A';
    }

    function B(){

    }

    Object.setPrototypeOf(B.prototype,new A);
    
    B.prototype.getSubName = function (){
        console.log('B')
        return 'B';
    }

    const b = new B;

    b.getSubName(); // B
    b.getSupName(); // A

```

## instanceof运算符的判断机制是怎么样的

    使用语法 object instanceof target

    当target是函数时，instanceof是用来检测构造函数的prototype属性是否出现在原型链上
    当在target是对象时，会先访问对象的@@hasInstance属性，该属性必须要指向一个函数，该函数的返回值作为判断结果，不存在该属性或不为函数都会抛出错误

```JavaScript
    const obj = {
        sex: 1,
        [Symbol.hasInstance](target){
            console.log(target);
            return true;
        },
    }


    const obj1 = {
        age: 18,
    };

    console.log(obj1 instanceof obj); // true


```









