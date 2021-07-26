# this

## this是什么

    this在JavaScript中，就是一个指向调用函数的对象的指针

## this的绑定

    this目前的绑定方式有以下4中，优先级从低到高

### 1.默认绑定

```JavaScript
var val = 10;
function f1(){
    console.log(this.val);
    function f2(){
        console.log(this.val);
    }
    f2(); // 10
}

f1(); // 10 
```

默认情况下，this指向全局对象，上述代码中val在全局环境下由var声明的变量，
会作为全局对象的属性，this默认指向全局对象window，所以
this.val===window.val,f2函数中的val也是同理

在严格模式下，函数内的this不会默认指向window


### 2.隐式绑定


```JavaScript
var val = 10;
function f1(){
    console.log(this.val);
    function f2(){
        console.log(this.val);
    }
    f2(); // 10
}

const obj = {
    val:200,
    f1,
};

obj.f1(); // 200
```
当函数在被作为对象方法调用时，函数的this会被隐式的绑定到这个对象，所以
f1会打印200,而不是10，而f2是直接调用，并没有作为对象方法，所以this默认
绑定到全局对象，打印10

### 3.显示绑定

JavaScript提供了3中显示绑定的方式

#### 1.bind()

```JavaScript
var val = 10;

const obj = {val:200};

function f1(){
    console.log(this.val);
    function f2(){
        console.log(this.val);
    }
    f2(); // 10
}

const f2 = f1.bind(obj);

f2(); // 200

```
bind函数返回一个将this强制绑定为参数对象的函数

#### 2.apply()

```JavaScript
var val = 10;

const obj = {val:200};

function f1(){
    console.log(this.val);
    function f2(){
        console.log(this.val);
    }
    f2(); // 10
}

f1.apply(obj); // 200

```
apply会直接调用函数并将第一个参数绑定到函数的this

#### 3.call()

```JavaScript
var val = 10;

const obj = {val:200};

function f1(){
    console.log(this.val);
    function f2(){
        console.log(this.val);
    }
    f2(); // 10
}

f1.call(obj); // 200

```
call会直接调用函数并将第一个参数绑定到函数的this,call与apply几乎一直，
只有给函数传参的方式不同，apply的第二参数为数组
(apply(context,[arg1,arg2]))，函数的参数都会放入到里面，而call是将参
数进行列举(call(context,arg1,arg2))

### 4.new绑定

```JavaScript
var val = 10;

function f1(){
    this.val = 200;
}

const obj = new f1;

console.log(val,obj.val); // 10 200
```
先看下new运算符实例化的过程
    
    1.创建一个空对象{}
    2.将新创建的对象的内部插槽[[prototype]]设为函数的prototype对象
    3.将函数内部的this绑定为这个对象
    4.执行函数，为this引用添加属性
    5.如果函数返回值为一个对象时会返回这个对象，不然返回this引用

可以从上面看到this在第三步的时候被修改了绑定






