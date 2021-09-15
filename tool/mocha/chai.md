# chai


## expect

### 语义性接口
    以下接口用来提高语义性，本身并不具有作用

        .to
        .be
        .been
        .is
        .that
        .which
        .and
        .has
        .have
        .with
        .at
        .of
        .same
        .but
        .does
        .still
        .aslo

### .not
    否定链中跟随的所有断言

### .deep
    导致链中所有的.equal、.include、.member、.keys、.property断言使用深度相等而不是严格相等

### .nested
    在链中的所有.property和.include断言都启用`.`和括号表示法
    不能和.own一起用

### .own
    导致链中所有的.property和.include断言忽略继承的属性

### .ordered
    导致链中所有.merber断言要求成员顺序相同

### .any
    导致链中所有的.key断言至少有一个给定的键

### .all
    导致链中要满足所有给定的.key,同时只含有所有的key

### .a|.an
    断言目标的类型等于给定的字符串，不区分大小写

    

    

