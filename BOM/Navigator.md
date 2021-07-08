# Navigator

    navigator接口表示用户代理的状态和标识

## 属性
    
    cookieEnabled 只读
        返回一个布尔值，用来表明当前页面是否启用了cookie

    
    geolocation 只读
        获取Geolocation对象，用来访问设备的位置信息
    
    hardwareConcurrency 只读
        获取可用的逻辑处理器核心数
    
    language  只读
        获取用户的首选语言

    maxTouchPoints 只读
        获取当前设备能够支持的最大同时触摸的点数

    onLine  只读
        布尔值，获取当前浏览器的联网状态

    oscpu
        获取当前使用的操作系统类型
    
    serviceWorker  只读
        获取ServiceWorkerContainer对象，
        用来提供注册、删除、更新以及ServiceWorker对象之间的通信
    
    storage 只读
        获取StorageManager对象，用来维持数据的持久化存储

    userAgent 只读
        获取当前浏览器的用户代理字符串