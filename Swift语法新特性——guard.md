---
title: Swift语法新特性——guard
date: 2016-08-12 14:42:49
tags:
---

**Swift语法新特性——guard**

**与if语句相同的是，guard也是基于一个表达式的布尔值去判断一段代码是否该被执行。与if语句不同的是，guard只有在条件不满足的时候才会执行这段代码。你可以把guard近似的看做是Assert，但是你可以优雅的退出而非崩溃。**
**从下面的例子中就可以看出guard的优美之处，不再用很多嵌套的if else 知道了guard语法后，代码的可读性变强。**
```objectivec
//if else
func buy( money: Int , price: Int , capacity: Int , volume: Int){
    
    if money >= price{
        if capacity >= volume{
            print("I can buy it!")
            print("\(money-price) Yuan left.")
            print("\(capacity-volume) cubic meters left")
        }
        else{
            print("No enough capacity")
        }
    }
    else{
        print("Not enough money")
    }
}

//guard guard 只有在不满足条件的情况下才会执行括号中的内容，这样就可更专注于符合的逻辑，增强可读性
func buy2( money: Int , price: Int , capacity: Int , volume: Int){
    
    guard money >= price else{
        print("Not enough money")
        return
    }
    
    guard capacity >= volume else{
        print("Not enough capacity")
        return
    }
    print("I can buy it")
    print("\(money-price) Yuan left.")
    print("\(capacity-volume) cubic meters left")
}

let money = 100
let price = 170

let capacity = 50
let volume = 20

buy( money, price: price, capacity: capacity, volume: volume)
buy2( money, price: price, capacity: capacity, volume: volume)

```