# instancetype与id,类前缀
- instancetype在类型表示上，跟id一样，可以表示任何对象类型

- instancetype只能用在返回值类型上，不能像id一样用在参数类型上

- instancetype比id多一个好处：编译器会检测instancetype的真实类型


#类前缀
- 使用Objective-C开发iOS程序时，最好在每个类名前面加一个前缀，用来标识这个类的“老家”在哪

- 目的是防止N个人开发了一样的类，冲突了
比如Jake Will、Kate Room在同一个项目中都各自开发了个Button类，这样的程序是不能运行起来的
- 解决方案：Jake Will的类名叫做JWButton，Kate Room的类名叫做KRButton


#Property
## @property的使用策略
- assign
    - `基本数据类型`、`枚举`、`结构体`等非OC对象类型
- weak
    - OC对象类型（比如NSArray、NSDate、NSNumber、模型类）
- strong
    - OC对象类型（比如NSArray、NSDate、NSNumber、模型类）
    - 一个对象只要有强指针引用着，就不会被销毁
- copy
    - 一般用在`NSString`、`block`类型上