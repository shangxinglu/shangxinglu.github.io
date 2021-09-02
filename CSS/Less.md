# Less

## 安装

### 全局安装

```NPM
    npm i less -g
```


### 局部安装

```NPM
    npm i less -D
```

## 编译Less文件

```NPM
    lessc file.less file.css
```

## 使用

### 变量

```LESS
    @width:10px;
    .div{
        width:@width;
    }
```

### 混合

```LESS
.div{
    width:100px;
    height:100px;
}

.div1{
    .div();
    background-color: cyan;
}

.div2{
    .div();
    background-color: orange;
}
```
    编译后

```CSS
.div {
  width: 100px;
  height: 100px;
}
.div1 {
  width: 100px;
  height: 100px;
  background-color: cyan;
}
.div2 {
  width: 100px;
  height: 100px;
  background-color: orange;
}

```

### 嵌套

```LESS
.test {
    .div{
        background: #000;
    }
}
```

编译后

```CSS
.test .div {
  background: #000;
}
```

### @规则嵌套和冒泡
    @规则可以与选择器以相同的方式进行嵌套，@规则会被放在前面，同一规则集中的其他元素
    的相对顺序保持不变，这叫冒泡

```LESS
.test1{
    width: 100%;

    @media (max-width:200px){
        width: 200px;

        @media (max-width:400px){
            width: 400px;
        }
    }

    @media (max-width:800px){
        width: 800px;
    }
}
```

    编译后

```CSS
.test1 {
  width: 100%;
}
@media (max-width: 200px) {
  .test1 {
    width: 200px;
  }
}
@media (max-width: 200px) and (max-width: 400px) {
  .test1 {
    width: 400px;
  }
}
@media (max-width: 800px) {
  .test1 {
    width: 800px;
  }
}

```


### 转义
    任何以`~"anything`或`~'anything'`形式的内容都将按原样数据，除非插值
    在3.5+的版本已经不需要引号转义

### 命名空间和访问符
    主要是为了提供封装的能力

```LESS
#ns {
    .div{
        width: 50px;
        height: 50px;
    }

    .bg{
        background-color: cyan;
    }
}

.test3{
    #ns();
}
```
    编译后

```CSS
#ns .div {
  width: 50px;
  height: 50px;
}
#ns .bg {
  background-color: cyan;
}
.test3 .div {
  width: 50px;
  height: 50px;
}
.test3 .bg {
  background-color: cyan;
}
```

### 映射
    3.5版本开始，可以将混合和规则集作为一组值的映射使用

```LESS
.test4(){
    test-name:orange;
}

.test5{
    background-color: .test4()[test-name];
}
```
    
    编译

```CSS
.test5 {
  background-color: orange;
}
```


