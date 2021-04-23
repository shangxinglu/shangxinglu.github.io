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

    还要就是原型链查找是找的[[prototype]]，而不是prototype


<br/>



## Prototype的继承

```javascript

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




