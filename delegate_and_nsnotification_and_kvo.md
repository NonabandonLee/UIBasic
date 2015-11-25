## iOS监听某些事件的方法
- 通知（NSNotificationCenter\NSNotification）
    - 任何对象之间都可以传递消息
    - 使用范围
        - 1个对象可以发通知给N个对象
        - 1个对象可以接受N个对象发出的通知
    - 必须得保证通知的名字在发出和监听时是一致的
- KVO
    - 仅仅是能监听对象属性的改变（灵活度不如通知和代理）
- 代理
    - 使用范围
        - 1个对象只能设置一个代理(假设这个对象只有1个代理属性)
        - 1个对象能成为多个对象的代理
    - 比`通知`规范
    - 建议使用`代理`多于`通知`


## 代理的使用步骤
- 定义一份代理协议
    - 协议名字的格式一般是：类名 + Delegate
        - 比如UITableViewDelegate
    - 代理方法细节
        - 一般都是@optional
        - 方法名一般都以类名开头
            - 比如`- (void)scrollViewDidScroll:`
        - 一般都需要将对象本身传出去
            - 比如tableView的方法都会把tableView本身传出去
    - 必须要遵守NSObject协议
        - 比如`@protocol XMGWineCellDelegate <NSObject>`
- 声明一个代理属性
    - 代理的类型格式：id<协议> delegate

```objc
@property (nonatomic, weak) id<XMGWineCellDelegate> delegate;
```
- 设置代理对象

```objc
xxx.delegate = yyy;
```

- 代理对象遵守协议，实现协议里面相应的方法

- 当控件内部发生了一些事情，就可以调用代理的代理方法通知代理
    - 如果代理方法是@optional，那么需要判断方法是否有实现

```objc
if ([self.delegate respondsToSelector:@selector(wineCellDidClickPlusButton:)]) {
    [self.delegate wineCellDidClickPlusButton:self];
}
```