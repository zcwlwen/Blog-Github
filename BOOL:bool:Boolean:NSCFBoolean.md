
---
title: BOOL/bool/Boolean/NSCFBoolean区别
date: 2016-08-02 17:23:28
tags:
---

#BOOL/bool/Boolean/NSCFBoolean区别
---
| Name | Typedef | Header | True value | False Value |
| --- | --- | --- | --- | --- |
| BOOL | signed char | objc.h | YES | NO |
| bool | _Bool(int) | studbook.h | true | false |
| Boolean | unsigned char | MacTypes.h | TRUE | FALSE |
| NSNumber | __NSCFBoolean | Foundation.h | @(YES) | @(NO) |
| CFBoolleanRef | struct | CoreFoundation.h | kCFBooleanTrue | kCFBooleanFals  |
