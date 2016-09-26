
---
title: Label的自适应宽度
date: 2016-08-04 15:04:24
tags: 
---

**Label自适应高度与自适应宽度**

创建一个Category。

新建file->Objective—C File->下一步
![15:10:55.jpg](http://ww4.sinaimg.cn/large/006tKfTcgw1f6hqetxsqaj314k0ssn1f.jpg)

File Type选择：Category

Class :UILabel

下一步。
![15:11:27.jpg](http://ww3.sinaimg.cn/large/006tKfTcgw1f6hqfch4psj314k0ssq55.jpg)

在.h文件中添加两个方法

```C
/**
 *  传入字符串，字体自动计算宽度
 *
 *  @param title 字符串
 *  @param font  字体
 *
 *  @return 返回宽度
 */
+ (CGFloat)getWidthWithTitle:(NSString *)title font:(UIFont *)font;

/**
 *  传入宽度、字符串、字体。自动计算高度
 *
 *  @param width 宽度
 *  @param title 字符串
 *  @param font  字体
 *
 *  @return <#return value description#>
 */
+ (CGFloat)getHeightByWidth:(CGFloat)width title:(NSString *)title font:(UIFont*)font;
```

.m文件中实现相应的方法

```
+ (CGFloat)getHeightByWidth:(CGFloat)width title:(NSString *)title font:(UIFont *)font
{
    
    UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(0, 0, width, 0)];
    label.text = title;
    label.font = font;
    label.numberOfLines = 0;
    [label sizeToFit];
    CGFloat height = label.frame.size.height;
    return height;
}

+ (CGFloat)getWidthWithTitle:(NSString *)title font:(UIFont *)font {
    UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(0, 0, 1000, 0)];
    label.text = title;
    label.font = font;
    [label sizeToFit];
    return label.frame.size.width;
}
```

使用方法：

这里结合的是自定义cell。

```
   cell.userName.text = _dataArray1[indexPath.row];
   CGFloat width = [UILabel getWidthWithTitle:cell.userName.text font:cell.userName.font];
   cell.userName.frame = CGRectMake(5, 5, width, 30);
```

实现的效果：

两个Label连在一起，前面的Label的宽度要根据内容自适应，最后实现了想要的效果。

![15:22:54.jpg](http://ww4.sinaimg.cn/large/006tKfTcgw1f6hqra1s02j30da0bs74z.jpg)