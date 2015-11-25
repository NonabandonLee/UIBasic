#自定义等高的cell
- 等高的cell
    - 所有cell的高度是一样的


##纯代码创建
###frame
- 1,新建一个继承自`UITableViewCell`的子类，比如XMGTgCell
```objc
@interface TgCell : UITableViewCell
@end
```
- 2,在TgCell.m文件中
 - 重写`-initWithStyle:reuseIdentifier:`方法
    - 在这个方法中添加所有需要显示的子控件
    - 给子控件做一些初始化设置（设置字体、文字颜色等）

```objc
/**
 *  在这个方法中添加所有的子控件
 */
- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier
{
    if (self = [super initWithStyle:style reuseIdentifier:reuseIdentifier]) {
        // ......
    }
    return self;
}
```

- 重写`-layoutSubviews`方法
    - 一定要调用`[super layoutSubviews]`
    - 在这个方法中计算和设置所有子控件的frame

```objc
/**
 *  在这个方法中计算所有子控件的frame
 */
- (void)layoutSubviews
{
    [super layoutSubviews];

    // ......
}
```

- 3,在TgCell.h文件中提供一个模型属性，比如Tg模型

```objc
@class Tg;

@interface TgCell : UITableViewCell
/** 团购模型数据 */
@property (nonatomic, strong) Tg *tg;
@end
```


- 4,在TgCell.m中重写模型属性的set方法
 - 在set方法中给子控件设置模型数据

```objc
- (void)setTg:(Tg *)tg
{
    _tg = tg;

    // .......
}
```

- 5,在控制器中
 - 注册cell的类型
    
```objc
[self.tableView registerClass:[TgCell class] forCellReuseIdentifier:ID];
```
  
  

 - 给cell传递模型数据



```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    // 访问缓存池
    TgCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];

    // 设置数据(传递模型数据)
    cell.tg = self.tgs[indexPath.row];

    return cell;
}
```



##Autolayout
- 1,新建一个继承自`UITableViewCell`的子类，比如TgCell
```objc
@interface TgCell : UITableViewCell
@end
```
- 2,在TgCell.m文件中
 - 重写`-initWithStyle:reuseIdentifier:`方法
    - 在这个方法中添加所有需要显示的子控件
    - 给子控件做一些初始化设置（设置字体、文字颜色等）
    - `添加子控件的完整约束`

```objc
/**
 *  在这个方法中添加所有的子控件
 */
- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier
{
    if (self = [super initWithStyle:style reuseIdentifier:reuseIdentifier]) {
        // ......
    }
    return self;
}
```

- 3在模型Cell.h文件中提供一个模型属性，比如Tg模型

```objc
@class Tg;

@interface TgCell : UITableViewCell
/** 团购模型数据 */
@property (nonatomic, strong) Tg *tg;
@end
```

- 4,在TgCell.m中重写模型属性的set方法
 - 在set方法中给子控件设置模型数据

```objc
- (void)setTg:(Tg *)tg
{
    _tg = tg;

    // .......
}
```

- 5,在控制器中
 - 注册cell的类型

```objc
[self.tableView registerClass:[TgCell class] forCellReuseIdentifier:ID];
```

 - 给cell传递模型数据

```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    // 访问缓存池
    TgCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];

    // 设置数据(传递模型数据)
    cell.tg = self.tgs[indexPath.row];

    return cell;
}
```



##XIB
## 新建一个继承自`UITableViewCell`的子类，比如TgCell
```objc
@interface TgCell : UITableViewCell
@end
```

## 新建一个xib文件（文件名最好跟类名一致，比如TgCell.xib）
- 修改cell的class为XMGTgCell

![](Snip20150629_245.png)

- 绑定循环利用标识

![](Snip20150629_246.png)

- 添加子控件，设置子控件约束

![](Snip20150629_251.png)

- 将子控件连线到类扩展中

```objc
@interface TgCell()
@property (weak, nonatomic) IBOutlet UIImageView *iconImageView;
@property (weak, nonatomic) IBOutlet UILabel *titleLabel;
@property (weak, nonatomic) IBOutlet UILabel *priceLabel;
@property (weak, nonatomic) IBOutlet UILabel *buyCountLabel;
@end
```

## 在TgCell.h文件中提供一个模型属性，比如Tg模型
```objc
@class Tg;

@interface TgCell : UITableViewCell
/** 团购模型数据 */
@property (nonatomic, strong) Tg *tg;
@end
```

## 在TgCell.m中重写模型属性的set方法
- 在set方法中给子控件设置模型数据

```objc
- (void)setTg:(Tg *)tg
{
    _tg = tg;

    // .......
}
```

## 在控制器中
- 注册xib文件

```objc
[self.tableView registerNib:[UINib nibWithNibName:NSStringFromClass([XMGTgCell class]) bundle:nil] forCellReuseIdentifier:ID];
```

- 给cell传递模型数据

```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    // 访问缓存池
  TgCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];

    // 设置数据(传递模型数据)
    cell.tg = self.tgs[indexPath.row];

    return cell;
}
```

##storyBoard
- 基本上与XIB一致,当在缓存池中没有找到数据,会自动去storyboard当中去寻找,注意在要设置 identify否则在缓存池中会找不到这数据