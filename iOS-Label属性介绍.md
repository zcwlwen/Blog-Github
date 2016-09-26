---
title: iOS Label属性介绍
date: 2016-08-04 15:34:16
tags:
---


**Label属性介绍**

```
    //创建Label
    UILabel *label = [[UILabel alloc]initWithFrame:CGRectMake(0, 0, 100, 40)];
    
    //设置Label的Text
    label.text = @"这是Label的Text";
    
    //设置文字颜色
    label.textColor = [UIColor redColor];
    
    //设置Label的字体
    label.font = [UIFont systemFontOfSize:20];//系统默认字体
    label.font = [UIFont boldSystemFontOfSize:20];//系统默认加粗字体
    label.font = [UIFont italicSystemFontOfSize:20];//系统默认倾斜字体
    label.font = [UIFont fontWithName:@"Arial" size:20];//设置字体类型和大小
    
    //设置Label的对其方式
    label.textAlignment = NSTextAlignmentCenter;//居中对齐
    label.textAlignment = NSTextAlignmentRight;
    label.textAlignment = NSTextAlignmentLeft;
    
    //设置label显示的行数
    label.numberOfLines = 0;//设置0代表自动换行
    
    //高亮
    label.highlighted = YES;
    label.highlightedTextColor = [UIColor blueColor];
    
    //设置阴影颜色与偏移量
    [label setShadowColor:[UIColor purpleColor]];
    [label setShadowOffset:CGSizeMake(-1, -1)];
    
    
    /**
     *  adjustsFontSizeToFitWidth设置为YES来控制文本基线行为
     *  UIBaselineAdjustmentAlignBaselines = 0,默认，文本最上端与中线对齐。
        UIBaselineAdjustmentAlignCenters,  文本中线与label中线对齐。
        UIBaselineAdjustmentNone, 文本最低端与label中线对齐。
     */
    label.adjustsFontSizeToFitWidth = YES;
    label.baselineAdjustment = UIBaselineAdjustmentNone;
    
    //设置字体大小适应label宽度
    label.adjustsFontSizeToFitWidth = YES;
    
    //设置边框,所有UIView控件都有下面的属性都可用同样的方法设置
    label.layer.borderColor = [[UIColor redColor]CGColor];
    label.layer.borderWidth = 2.0;
    label.layer.masksToBounds = YES;
    label.layer.cornerRadius = 10;
```
