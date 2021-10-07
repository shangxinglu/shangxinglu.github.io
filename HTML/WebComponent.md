# Web Component

    WebComonent由三项技术组成
        1. custom element
        2. shadow DOM
        3. HTML template

## Custom element
    custom element使用CustomElementRegistry注册
    window.customElements指向一个CustomELementRegistry实例的引用

### CustomElementRegistry对象

    define(name,constructor,[,options])
        定义自主定制元素，独立元素，不会从内置HTML元素继承，但是必须继承HTMLElement
        定义自定义内置元素，继承并扩展内置HTML元素

        name
            自定义元素名，必须要有`-`
        
        constructor
            自定义元素构造器

        options 可选
            控制元素如何自定义，目前只有一个选项
                extends指定继承的已创建的元素，被用于创建自定义元素



## Shadow DOM

### Shadow DOM术语

    Shadow host
        一个常规的DOM节点，Shadow DOM会被附加到这个节点
    
    Shadow tree
        Shadow DOM内部的DOM树

    Shadow boundary
        Shadow DOM结束的地方，也是常规DOM开始的地方
    
    Shadow root
        Shadow tree的根节点


### Element.attachShadow()
    给指定元素挂载一个shadow DOM，并返回对ShadowRoot的引用

    attachShadow(shadowRootInit)
        shadowRootInit
            一个shadowRootInit的字典，包含下列字段
                mode    
                    指定Shadow DOM树封装模式的字符串

                    open
                        shadow root的元素可以从外部的JavaScript访问

                    closed
                        shadow root的元素不可以从外部的JavaScript访问
                
                delegatesFocus
                    焦点委托，当设置为true时，指定减轻自定义元素的聚焦性能问题，
                    当shadow DOM中不可聚焦的部分被点击时，让第一个可聚焦的部分称为焦点
            
        
        返回
            shadow对象或null


## template 和 slot

### template
    template元素及其内容不会在DOM中呈现，通过js获取template元素，content属性是template的内容

### slot
    插槽由其name属性标识，可以在模板中定义占位符

```HTML
    <template>
        <slot name="test">默认文本</slot>
    </template>

    <div slot="test">内容</div>
```
                        

