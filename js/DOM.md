# DOM

## DOM是什么

    DOM的全称是Document Object Model，文档对象模型，是HTML和XML文档的
    编程接口，为开发者提供了添加、删除、修改页面的功能，HTML和XML可以
    用DOM表示为一个由节点构成的层级结构

<br/>

## Node
    Node是一个接口，各类的DOM API对象都会继承Node

## Node的类型有哪些

    DOM中的节点一共有12中类型，分别有12个数值常量表示，全是大写，为方便阅读写成小写
    
    node.element_node (1)
    node.attribute_node (2)
    node.text_node (3)
    node.cdata_section_node (4)
    node.entity_peference_node (5)
    node.entity_node (6)
    node.processing_instruction_node (7)
    node.comment_node (8)
    node.document_node (9)
    node.document_type_node (10)
    node.document_fragment_node (11)
    node.notation_node (12)

<br/>

## Node的数据属性有哪些

    childNodes  只读
        包含该节点所有子节点的实时NodeList,
        NodeList是动态的，会根据子节点自动更新

```html
    <div id="t1">1</div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    const t1El = document.createTextNode('2');
    node1.append(t1El);
    console.log(node1.childNodes); //[text, text]
```

    firstChild  只读
        该节点第一个子节点，如果没有就会返回null
        

```html
    <div id="t1">1</div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    console.log(node1.firstChild); // 1
```

    isConnected  只读
        返回一个布尔值用来检测该节点是否已经连接到一个上下文对象
        
```html
    <div id="t1">1</div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    console.log(node1.isConnected); // true
```

    lastChild  只读
        该节点最后一个子节点，如果没有就会返回null
        
```html
    <div id="t1">1<span>2</span></div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    console.log(node1.lastChild); // <span>2</span>
```

    nextSibling  只读
        该节点同级的下一个节点，如果没有就会返回null
        
```html
    <div id="t1">1</div><span>2</span>
```

```JavaScript
    const node1 = document.getElementById('t1');
    console.log(node1.nextSibling); // <span>2</span>
```

    nodeName
        节点的名字
    
```html
     <span id="t1"><span>
```

```JavaScript
    const node1 = document.getElementById('t1');
    console.log(node1.nodeName); // SPAN
```

    nodeType
        节点的类型

```html
     <div id="t1"></div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    console.log(node1.nodeType); // 1
```
    
    nodeValue
        返回或设置当前节点的值，对于元素节点，他的nodeValue始终是null

```html
     <div id="t1">1</div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    console.log(node1.nodeValue); // null
    console.log(node1.firstChild.nodeValue); // 1
```

    ownerDocument
        返回该节点属于的Document对象，如果没有就返回null
    
```html
     <div id="t1"></div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    console.log(node1.ownerDocument); // #document
```

    parentNode
        返回该节点的父节点，如果没有返回null

```html
     <div id="t1"></div>
```

```JavaScript
    const node1 = document.getElementById('t1');
   console.log(node1.parentNode); // <body>...</body>
```

    parentElement
        返回该节点的父节点元素,如果父节点不存在或不是元素，就返回null

```html
     <div id="t1"></div>
```

```JavaScript
    const node1 = document.getElementById('t1');
   console.log(node1.parentElement); // <body>...</body>
```
    
    previousSibling
        返回该节点同级的前一个元素，没有就返回null
    
```html
     <span>1</span><div id="t1"></div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    console.log(node1.previousSibling); // <span>1</span>
```

    textContent
        返回或设置该节点的所有子节点的文本内容
    
```html
     <div id="t1"><span>1<span>2</span></span></div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    console.log(node1.textContent); // 12
```


## Node的方法有哪些

    appendChild(child)
        作用
            将子节点添加到指定父节点的列表末尾，如果子节点之前已经存在于当前文档数，就相当于把他移动了位置

        参数
            child   子节点

        返回    
            添加的子节点
        
```html
     <div id="t1"></div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    const text1El = document.createTextNode('new');

    node1.appendChild(text1El);

    console.log(node1); // <div id="t1">new</div>
```

    cloneNode(deep)
        作用
            克隆一个节点

        参数
            [deep=false]   是否深度克隆，克隆所有后代节点，默认只克隆节点本身
        
        返回
            克隆生成的节点

```html
    <div id="t1"><span>1</span></div>
```

```JavaScript
    const node1 = document.getElementById('t1');
   const cloneEl = node1.cloneNode(true);

    console.log(cloneEl); // <div id="t1"><span>1</span></div>
```

    compareDocumentPosition(otherNode)
        作用
            用来比较当前节点和文档中任一节点的位置关系

        参数
            otherNode 用于比较位置的节点
        
        返回
            一个数值,数值结果是一下结果中的一个或多个相加
            1   两个节点不在同一文档中
            2   otherNode在node之前
            4   otherNode在node之后
            8   otherNode包含node
            16  otherNode被node包含
        
```html
   <div id="t2"><div id="t1"></div></div>
```

```JavaScript
    const node1 = document.getElementById('t1');
   const node2 = document.querySelector('#t2');

    const positon = node1.compareDocumentPosition(node2);
    console.log(positon); // 10 同时满足8和2
```

    contains(otherNode)
        作用
            判断传入节点是否为该节点的后代节点
        
        参数
            otherNode
        返回
            布尔值

```html
   <div id="t1"><div id="t2"></div></div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    const node2 = document.querySelector('#t2');

    const result = node1.contains(node2);

    console.log(result); // true

```

    getRootNode
        作用
            获取上下文的根节点
        
        参数
            [options] 可选参数对象

            [options.composed=false]    如果过为true则跳过shadow Root检索

        返回
            根节点

```html
   <div id="t1"></div>
```

```JavaScript
    const node1 = document.getElementById('t1');

    const rootEl = node1.getRootNode();
    console.log(rootEl); // #document

```

    hasChildNodes()
        作用
            判断该节点是否有子节点
        
       返回
            布尔值

```html
   <div id="t1"></div>
```

```JavaScript
    const node1 = document.getElementById('t1');

    const result = node1.hasChildNodes();
    console.log(result); // false

```

    insertBefore(newNode,referenceNode)
        作用
            在当前节点下添加一个子节点，位于参考节点之前，
            如果新节点原本就存在节点树，相当于把节点移动了位置

        参数
            newNode
                需要插入的节点
            
            referenceNode
                参考节点


```html
   <div id="t1"><span id="t2"> old</span></div>
```

```JavaScript
    const node1 = document.getElementById('t1');

    const node2 = document.querySelector('#t2'),
    newNode = document.createTextNode('new node');

    node1.insertBefore(newNode,node2); // new node old

```

    isEqualNode(otherNode)
        作用
            判断两个节点是否相等
        
        参数
            otherNode
        
        返回
            布尔值

```html
      <div id="t1">1 2 3</div>
    <div class="t2">1 2 3</div>
    <div class="t2">1 2 3</div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    const [node2,node3] = document.querySelectorAll('.t2');

    console.log(node1.isEqualNode(node2),node2.isEqualNode(node3)); // false true 

```

    normalize()
        作用
            将点前节点和他的后代节点规范化，规范化后不会存在一个空的文本节点或者两个相邻的文本节点，空的文本节点不包括空白字符(空格、换行等)构成的文本节点

```html
    <div id="t1"></div>
```

```JavaScript
    const node1 = document.getElementById('t1');
    const node2 = document.createTextNode('n2 '),
    node3 = document.createTextNode('n3 ');
    node1.append(node2,node3);
    console.log(node1.childNodes.length); // 2
    node1.normalize();
    console.log(node1.childNodes.length);// 1

```

    removeChild()
        作用
            删除该节点的一个子节点

        参数
            child 需要移除的节点
        
        返回删除的节点

```html
    <div id="t1"><span id="t2"></span></div>
```

```JavaScript
    const node2 = document.querySelector('#t2');

    console.log(node1.removeChild(node2)); // <span id="t2"></span>
    console.log(node1); // <div id="t1"></div>
```

    replaceChild(newChild,oldChild)
        作用
            替换当前节点的一个子节点
        
        参数
            newChild 用来替换的节点    

            oldChild 被替换的节点
        
        返回
            被替换的节点

```html
    <div id="t1"><span id="t2">old child</span></div>
```

```JavaScript
    const node2 = document.getElementById('t2');
    const newChild = document.createTextNode('new child');

    node1.replaceChild(newChild,node2);

    console.log(node1); // <div id="t1">new child</div>
```

    isDefaultNamespace(url)
        作用
            判断url是否是当前节点的默认命名空间
        
        参数
            url 命名空间
        
        返回
            布尔值

```html
    <div id="t1"></div>
```

```JavaScript
    console.log(node1.isDefaultNamespace('http://www.w3.org/1999/xhtml')); // true

    const divEl = document.createElementNS('test','div');

    console.log(divEl.isDefaultNamespace('test')); // true
```