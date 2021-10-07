# Module

## export命令
    export命令规定的是对外的接口必须与模块内部的变量建立一一对应的关系
    export输出的接口，与其对应的值是动态绑定的关系，即通过该接口，可以获取到模块内部实时的值


### 默认导出

```JavaScript
    export default {

    }
```

### 变量导出

```JavaScript
    export const x = 1;
```

```JavaScript
    const y = 2;
    const z = 3;
    
    export {y,z}
```

### 重命名

```JavaScript
    const y = 2;
    const z = 3;
    
    export {
        y as name,
        y as age,
        z
    }
```

## import命令
    import命令输入的变量都是只读的
    多次执行同一句import语句只会执行一次

### 默认导入

```JavaScript
    import test from './test.js'
```

### 变量导入

```JavaScript
    import {x,y,z} from './test.js'
```
### 变量重命名

```JavaScript
    import {y as name} from './test.js'
```
### 模块的整体加载

```JavaScript
    import * as test from './test.js'
```
### export与import的复合写法
    需要注意，复合写法并不会在当前模块导入其他模块，只是对外转发，所以不能当前模块中使用

```JavaScript
   export {x,y}  from './test.js'

   // 理解为
   import {x,y} from './test.js'
   export {x,y}

```
