---
title: iOS五种传值方式
date: 2016-08-03 16:32:48
tags:
---

#iOS中的五种传值方式

**属性传值**
-
**场景**：正向传值，界面A push 到界面B。把A中的值传递给B

**使用**：只需要在界面B的头文件声明属性，在A中初始化B的实例后，直接给B的实例的属性赋值。就可以实现A的值传给B

1、在B的.h文件中声明属性(userName和userAge就是你要从A中获得的值)

```
@property (nonatomic, copy)NSString *userName;
@property (nonatomic, copy)NSString *userAge;
```
2、在A中push到B前的时候给B的实例的属性赋值。(_userName就是A中的值，把这个值赋值给B的属性)

```
    //初始化一个实例，给实例中的属性赋值，这样就实现了OneVC的值传递给TwoVC
    TwoViewController *vc = [[TwoViewController alloc]init];
    vc.userName = _userName;
    vc.userAge  = _userAge;
    [self.navigationController pushViewController:vc animated:YES];
```
**总结**：
使用简单，只能是在正向传值的场景中使用。

-----

**代理传值**
-
**场景：**逆向传值，A push到 B ，B pop回 A ,把B的值传递给A。

**使用：**

1、在B（.h文件）中声明协议、和协议方法

```
//协议的命名规范：类名+delegate
@protocol DelegateTwoViewControllerDelegate <NSObject>
//将要传的值作为协议方法的参数
- (void)getString:(NSString *)string;
@end
```

2、添加代理人属性（在B的.h文件中）

```
@interface DelegateTwoViewController : UIViewController
@property (nonatomic,weak) id<DelegateTwoViewControllerDelegate> delegate;
@end

```

3、让代理人执行协议方法（在B的.m文件中）这里_textfiled.text就是要传递给A的值

```
//这一步一般是在B跳转到A的方法中实现的，我是在B中创建了一个Button,让这个通知代理在Button中跳转方法中实现。
if ([self.delegate respondsToSelector:@selector(getString:)]) {
  // 加入if语句就是先判断在界面A中是否有getStrin这个方法，当有这个方法的时候，才进行代理传值。
    [self.delegate getString:_textfiled.text];
}
```
4、A遵守协议（在A的.m文件中）

```
@interface DelegateOneViewController ()<DelegateTwoViewControllerDelegate>
```
5、指定代理人（在A的.m文件中）

```
- (void)pushEvent{
    DelegateTwoViewController *vc = [[DelegateTwoViewController alloc]init];
    vc.delegate = self;
    [self.navigationController pushViewController:vc animated:YES];
}
```

6、实现协议方法（在A的.m文件中）这里的string就是从B中传来的值

```
- (void)getString:(NSString *)string{
    _label.text = string;
}
```


