# Class

## Class的作用是什么

    class关键字是ES6提出的，是一个语法糖，可以用写类的方式来实现原型继承
    

## Class的使用

### 简单使用

    class的实例化中，运算符()是作用是传值参数表，并没有调用函数的意义，所以不需要给构造函数传递参数表时可以不加

```JavaScript
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

```JavaScript
    const A = class {
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

    继承使用extends关键字，在extends后面是要继承的父类parentClass，这个父类的prototype必须是Object或者null，可以使用表达式的返回值作为父类，如果在子类中定义construct，必须先调用super
    
```JavaScript
    
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

    super是用来在子类中访问父类的关键字，可以作为父类构造函数调用，也可以用来调用父类中的方法

### [[HomeObject]]

    [[HomeObject]]是函数的一个内部插槽，当函数不是箭头函数时，用来保存在语法分析阶段确定的、声明方法时所基于的对象，默认值为undefined，[HomeObject]是不可变的，绑定的对象一般有两种

        1. 函数是对象方法
            [[HomeObject]]绑定该对象的prototype
        
        2. 函数是静态方法
            [[HomeObject]]绑定该对象本身



### Super的执行过程

    super的查找就是利用[[HomeObject]]，会先通过[[HomeObject]]找到绑定该方法的对象(obj)，然后再查找到obj的[[prototype]]，这样就找到了父类，由于[[HomeObject]]是固定不变的，所以super也是不变的

```JavaScript
    const obj1 = {
        fun1(){
            console.log(super.attr);
        },
    }

    Object.setPrototypeOf(obj1,{attr:'obj1'});

    const obj2 = {};
    Object.setPrototypeOf(obj2,{attr:'obj2'});

    obj2.fun2 = obj1.fun1;

    obj2.fun2(); // obj1
```
    

    



## Class实例化是怎么用原型写法实现的

    以下下两段代码是等效的

```JavaScript
    class A {
        k1 = 'k1';

        getSupName(){
            return 'A';
        }
    }


    class B extends A{
        k2 = 'k2';

        getSubName(){
            return 'B';
        }
    }


    const b = new B;

    console.log(b.k1,b.k2,b.getSupName(),b.getSubName()); // k1 k2 A B

```


```JavaScript
    function A(){
        this.k1 = 'k1';
    }

    A.prototype.getSupName = function(){
        return 'A';
    }

    function B(){
        A.apply(this); // 就是super()
        this.k2 = 'k2';
    }

    Object.setPrototypeOf(B,A); // extends的作用
    Object.setPrototypeOf(B.prototype,A.prototype); //extends的作用

    B.prototype.getSubName = function(){
        return 'B';
    }

    const b = new B;

    console.log(b.k1,b.k2,b.getSupName(),b.getSubName()); // k1 k2 A B

```

    更加完善的一个class继承模拟

```JavaScript
    // class A {
    //     k1 = 'A';

    //     constructor() {
    //         console.log('A',arguments);
    //     }

    //     static getSup(){
    //         console.log('static','A');
    //     }

    //     getSupName() {
    //         console.log('A');
    //     }
    // }

    const A = Function;
    class B extends A {
        k2 = 'B';

        constructor() {
            super(...arguments);
            this.k3 = 'B';
            console.log('B',arguments);
        }

        getSubName() {
            console.log('B');
        }
    }

    const b = new B([],'console.log(1);')

    b(); // 1
```

```JavaScript
    // function A() {
    //     this.k1 = 'A';
    //     console.log('A', arguments);

    // }

    // A.getSup = function(){
    //     console.log('static','A');
    // }

    // A.prototype.getSupName = function () {
    //     console.log('A');
    // }

    const A = Function;

    function B(){

        const instance = new (Object.getPrototypeOf(B))(...arguments);
        
        // const instance = {};
        // (Object.getPrototypeOf(B)).apply(instance,[...arguments]);
        // debugger
        function construcotr(){
            this.k2 = 'B';
            this.k3 = 'B';

            console.log('B',arguments);
        }

        construcotr.apply(instance,[...arguments]);
        
        Object.setPrototypeOf(instance, B.prototype);

        return instance;
    }


    // function B() {
    //     A.apply(this);
    //     this.k2 = 'B';
    //     this.k3 = 'B';
    //     console.log('B', arguments);
    // }


    Object.setPrototypeOf(B, A),
        Object.setPrototypeOf(B.prototype, A.prototype);

    B.prototype.getSubName = function () {
        console.log('B');
    }

    const b = new B([],'console.log(1);');

    b(); //1
```


<br/>


## Class与Prototype继承的区别

    class的实例是创建自基类的，类构造函数的调用是逆向的，
    prototype的实例是创建自子类的
