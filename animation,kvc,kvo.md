## 中间的提醒内容
- 指示器、HUD、遮盖、蒙板
- 半透明的指示器如何实现？
    - 指示器的alpha = 1.0
    - 指示器的背景色是半透明的

## 创建颜色
- 直接创建对应的颜色

```objc
+ (UIColor *)blackColor;      // 0.0 white
+ (UIColor *)darkGrayColor;   // 0.333 white
+ (UIColor *)lightGrayColor;  // 0.667 white
+ (UIColor *)whiteColor;      // 1.0 white
+ (UIColor *)grayColor;       // 0.5 white
+ (UIColor *)redColor;        // 1.0, 0.0, 0.0 RGB
+ (UIColor *)greenColor;      // 0.0, 1.0, 0.0 RGB
+ (UIColor *)blueColor;       // 0.0, 0.0, 1.0 RGB
+ (UIColor *)cyanColor;       // 0.0, 1.0, 1.0 RGB
+ (UIColor *)yellowColor;     // 1.0, 1.0, 0.0 RGB
+ (UIColor *)magentaColor;    // 1.0, 0.0, 1.0 RGB
+ (UIColor *)orangeColor;     // 1.0, 0.5, 0.0 RGB
+ (UIColor *)purpleColor;     // 0.5, 0.0, 0.5 RGB
+ (UIColor *)brownColor;      // 0.6, 0.4, 0.2 RGB
+ (UIColor *)clearColor;      // 透明色
```
- 根据RGB组合创建颜色

```objc
+ (UIColor *)colorWithRed:(CGFloat)red green:(CGFloat)green blue:(CGFloat)blue alpha:(CGFloat)alpha;
```

## 渐变动画
- 方式1：头尾式

```objc
[UIView beginAnimations:nil context:nil];
[UIView setAnimationDuration:2.0];

/* 需要执行动画的代码 */

[UIView commitAnimations];
```

- 方式2：block式

```objc
[UIView animateWithDuration:2.0 delay:1.0 options:kNilOptions animations:^{
    /* 需要执行动画的代码 */
} completion:nil]

// 1s后，再执行动画（动画持续2s）
```

## 按钮
- 自定义按钮：调整内部子控件的frame
    - 方式1：实现titleRectForContentRect:和imageRectForContentRect:方法，分别返回titleLabel和imageView的frame
    - 方式2：在layoutSubviews方法中设置
- 内边距

```objc
// 设置按钮内容的内边距（影响到imageView和titleLabel）
@property(nonatomic)          UIEdgeInsets contentEdgeInsets;
// 设置titleLabel的内边距（影响到titleLabel）
@property(nonatomic)          UIEdgeInsets titleEdgeInsets;
// 设置imageView的内边距（影响到imageView）
@property(nonatomic)          UIEdgeInsets imageEdgeInsets;
```

## 图片拉伸
- iOS5之前

```objc
// 只拉伸中间的1x1区域
- (UIImage *)stretchableImageWithLeftCapWidth:(NSInteger)leftCapWidth topCapHeight:(NSInteger)topCapHeight;
```

- iOS5开始

```objc
- (UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsets;
- (UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsets resizingMode:(UIImageResizingMode)resizingMode;
```

## 拷贝
- 实现拷贝的方法有2个
    - copy：返回不可变副本
    - mutableCopy：返回可变副本
- 普通对象实现拷贝的步骤
    - 遵守NSCopying协议
    - 实现-copyWithZone:方法
        - 创建新对象
        - 给新对象的属性赋值

## KVC
- 全称：Key Value Coding（键值编码）
- 赋值

```objc
// 能修改私有成员变量
- (void)setValue:(id)value forKey:(NSString *)key;
- (void)setValue:(id)value forKeyPath:(NSString *)keyPath;
- (void)setValuesForKeysWithDictionary:(NSDictionary *)keyedValues;
```

- 取值

```objc
// 能取得私有成员变量的值
- (id)valueForKey:(NSString *)key;
- (id)valueForKeyPath:(NSString *)keyPath;
- (NSDictionary *)dictionaryWithValuesForKeys:(NSArray *)keys;
```
## KVO
- 全称：Key Value Observing（键值监听）
- 作用：监听模型的属性值改变
- 步骤
    - 添加监听器
    ```objc
    // 利用b对象来监听a对象name属性的改变
    [a addObserver:b forKeyPath:@"name" options:NSKeyValueObservingOptionOld | NSKeyValueObservingOptionNew context:@"test"];
    ```
    - 在监听器中实现监听方法
    ```objc
    -(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change     context:(void *)context
    {
        NSLog(@"%@ %@ %@ %@", object, keyPath, change, context);
    }
    ```
