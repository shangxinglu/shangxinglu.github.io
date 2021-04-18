# Async

## Async函数的作用

Async只是使得异步操作变得更加简单，其实就是Generator的语法糖，主要效果有一下几点

    1. async函数时内置执行器的，不需要向Generator函数一样，需要手动执行，不需要额外的代码
    
    2. 将*换成了async，将yield换成了await，语义更加的明确了

    3. async函数的返回值是promise，Generator的返回值是IteratorResult对象，后续操作上更加的灵活

## Async函数的使用

    