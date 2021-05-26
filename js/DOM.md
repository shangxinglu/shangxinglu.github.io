# DOM

## DOM是什么

    DOM的全称是Document Object Model，文档对象模型，是HTML和XML文档的编程接口，为开发者提供了添加、删除、修改页面的功能，HTML和XML可以用DOM表示为一个由节点构成的层级结构

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

```javascript
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

```javascript
    const node1 = document.getElementById('t1');
    console.log(node1.firstChild); // 1
```

    isConnected  只读
        返回一个布尔值用来检测该节点是否已经连接到一个上下文对象
        
```html
    <div id="t1">1</div>
```

```javascript
    const node1 = document.getElementById('t1');
    console.log(node1.isConnected); // true
```

    lastChild  只读
        该节点最后一个子节点，如果没有就会返回null
        
```html
    <div id="t1">1<span>2</span></div>
```

```javascript
    const node1 = document.getElementById('t1');
    console.log(node1.lastChild); // <span>2</span>
```

    nextSibling  只读
        该节点同级的下一个节点，如果没有就会返回null
        
```html
    <div id="t1">1</div><span>2</span>
```

```javascript
    const node1 = document.getElementById('t1');
    console.log(node1.nextSibling); // <span>2</span>
```

    nodeName
        节点的名字
    
```html
     <span id="t1"><span>
```

```javascript
    const node1 = document.getElementById('t1');
    console.log(node1.nodeName); // SPAN
```

    nodeType
        节点的类型

```html
     <div id="t1"></div>
```

```javascript
    const node1 = document.getElementById('t1');
    console.log(node1.nodeType); // 1
```
    
    