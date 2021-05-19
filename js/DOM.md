# DOM

## DOM是什么

    DOM的全称是Document Object Model，文档对象模型，是HTML和XML文档的编程接口，为开发者提供了添加、删除、修改页面的功能，HTML和XML可以用DOM表示为一个由节点构成的层级结构

<br/>

## 节点的类型有哪些

    DOM中的节点一共有12中类型，可以由12个数值常量表示，全是大写，为方便阅读写成小写
    
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

## 节点Node的数据属性有哪些

    nodeName
        元素的标签名
    
```html
     <span id="n1"><span>
```

```javascript
    const node1 = document.getElementById('n1');
    console.log(node1.nodeName); // SPAN
```

    
    