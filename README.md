# 聊聊this

## this是什么

    this在JavaScript中，就是一个指向调用函数的对象的指针

## this的绑定

    this目前的绑定方式有以下4中，优先级从低到高

### 1.默认绑定

```javascript
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

默认情况下，this指向全局对象，上述代码中val在全局环境下由var声明的变量，会作为全局对象的属性，this默认指向全局对象window，所以this.val===window.val,f2函数中的val也是同理


### 2.隐式绑定


```javascript
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
当函数在被作为对象方法调用时，函数的this会别隐式的绑定到这个对象，所以f1会打印200,而不是10，有f2是直接调用，并没有作为对象方法，所以this默认绑定到全局对象，打印10

### 3.显示绑定

JavaScript提供了3中显示绑定的方式

#### 1.bind()

```javascript
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

```javascript
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

```javascript
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
call会直接调用函数并将第一个参数绑定到函数的this,call与apply几乎一直，只有给函数传参的方式不同，apply的第二参数为数组(apply(context,[arg1,arg2]))，函数的参数都会放入到里面，而call是将参数进行列举(call(context,arg1,arg2))

### 4.new绑定

```javascript
var val = 10;

function f1(){
    this.val = 200;
}

const obj = new f1;

console.log(obj.val);
```

