---
title: Objective－C高级编程：iOS与OS X多线程和内存管理
date: 2016-08-02 16:27:18
tags: iOS
---
#Objective－C高级编程：iOS与OS X多线程和内存管理

**自动引用计数**
自动引用计数（ARC，Automatic Reference Counting）
“在LLVM编辑器中设置ARC为有效状态，就无需再次键入retain或者是release代码”

**引用计数的内存管理**
生成对象->持有对象->释放对象->废弃对象

**内存管理的思考方式**
- 自己生成的对象，自己所持有
- 非自己生成的对象，自己也能持有
- 不在需要自己持有的对象时释放
- 非自己持有的对象无法释放

**对象操作与Objective-C方法的对应**

| 对象操作      |    Objective-C方法 |
| :--------    | --------:|
| 生成并持有对象 |alloc/new/copy/mutableCopy等| 
| 持有对象      |   retain  | 
| 释放对象      |   release | 
|  废弃对象     |   dealloc |

Objective-C内存管理方法并不包括在该语言中，而是包含在Cocoa框架中用于iOS、OS X开发。Cocoa框架中Foundation框架类库的NSObject类担负着内存管理的职责。其中alloc/retain/release/dealloc方法分别指的是NSObject类中的alloc类方法，retain实例方法，release实例方法，dealloc实例方法（   +alloc、-retain、-release、 -deallc）。

**自己生成的对象，自己所持有**
- alloc
- new
- copy
- mutableCopy


alloc方法
```Objective-C
id obj = [[NSObject alloc] init];
//自己生成持有对象
```
new方法
```Objective-C
id obj = [NSObject new];
//自己生成持有对象
```
copy方法利用基于NSCopying方法约定，由各类实现的copyWithZone：方法生成并持有该对象副本。
mutableCopy方法基于NSMutableCopying方法约定由各类实现的mutableCopyWithZone：方法生成并持有该对象副本。


------
**非自己生成的对象，自己也能持有**

这里用NSMutableArray类的array类方法举例：

```Objective-C
id obj = [NSMutableArray array];
//取得非自己生成的对象，但自己并不持有

［obj retain］;
// 自己持有对象

```
通过retain方法使非自己生成的对象成为自己所持有的。

----
**不在需要自己持有的对象时释放**

```Objective-C
id obj = [[NSObject alloc] init];
//自己生成持有对象

［obj release];
//释放对象
```

```Objective-C
id obj = [NSMutableArray array];
//取得非自己生成的对象，但自己并不持有

［obj retain］;
// 自己持有对象

［obj release];
//释放对象
```
用alloc等方法生成并自己持有的对象，用release方法释放
用retain方法持有非自己生成的对象，也用release方法释放
用上面方法持有的对象在不使用时要用release方法释放


-----

###ARC规则

**所有权修饰符**

OC中为了处理对象，可将变量类型定义成id类型或者各种对象类型（指向NSObject这样的的Objective－C类的指针，如NSObject＊）。
ARC有效时，id类型和对象类型和C语言其他类型不同，一定要加上所有权修饰符。
- _ _strong 修饰符
- _ _weak 修饰符
- _ _unsafe_unretained 修饰符
- _ _autoreleasing 修饰符

在没有明确指定所有修饰符时默认为_ _strong 修饰符，
_ _strong 修饰符表示对对象的强引用，持有其变量，在超出其作用域时被废弃，随着强引用的消失，引用的对象也会随之释放。

_ _unsafe_unretained 修饰符
和weak一样并不能持有对象，所以生成的对象会被立即释放。
在使用_ _unsafe_unretained 修饰符时，赋值给附有__strong修饰符的变量时要确保被赋值的对象存在。
_ _unsafe_unretained在iOS5在没有引入__weak之前使用现在基本不使用了。



关于_ _strong和_ _weak的区别我觉得下面这篇文章讲的很清楚、更直观，比这本书中讲到要直观很多。
[strong和weak](http://blog.csdn.net/q199109106q/article/details/8565017)

为什么会出现这两种修饰符？
因为 __strong并不能解决内存管理中一些问题，其中就是循环引用、类成员变量的循环引用（互相强引用）、对自身的强引用问题，这些很容易造成内存泄漏，应该废弃的对象在超出其生命周期后继续存在。这时使用 _weak 修饰符就不会造成循环引用。因为使用 _ _weak时并不持有该对象，所以不会出现应该废弃的对象在超出其生命周期后继续存在。



**ARC规则**

- 不能使用retain/release/retainCount/autorelease
- 不能使用NSAllocateObject/NSDeallocateObject
- 必须遵守内存管理的命名规则
- 不要显示调用dealloc
- 使用@autoreleasepool块替代NSAutoreleasePool；
- 不能使用区域（NSZone）
- 对象型变量不能作为C语言结构体（struct/union）的成员
- 显示转换"id"和"void * "


