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
    

    

