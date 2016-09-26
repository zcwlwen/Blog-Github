---
title: pch文件使用
date: 2016-08-02 16:46:28
tags:
---
###pch文件的使用

----
#####什么是pch文件
全称（precompiled header）也就是预编译头文件。pch文件的内容能被项目中的其他有资源的文件共享。

---
#####pch的作用
存放一些整个项目中不经常修改的东西，比如一些第三方类库的头文件、API接口。
存放一些全局的宏
还有一个好处是打开或者关闭日志输出功能

#####pch的创建
1、新建->iOS->Other->PCH File。
2、工程的TARGETS里面的Building Setting中搜索Prefix Header，然后把Precompile Prefix Header右边的NO改为YES
3、然后在Precompile Prefix Header下边的Prefix Header右边双击，添加刚刚创建的pch文件的工程路径，添加格式：“YourProjectName/YourProject-Prefix.pch 或 $(SRCROOT)/YourProject-Prefix.pch/项目名称/pch文件名” 。$(SRCROOT)的意思就是工程根目录的意思。添加完成后会自动帮你生成完整的路径，如果编译报错，检查路径是不是有错。
4、将Precompile Prefix Header为YES，预编译后的pch文件会被缓存起来，可以提高编译速度
#####到底要不要用pch
苹果在Xcode6以后舍弃了.pch文件，应该是因为预编译影响软件时长。
以及使用pch文件可能导致依赖关系不明确。
可以查看这些资料后权衡：

[知乎关于代替方案](http://www.zhihu.com/question/30417704)

[知乎](http://www.zhihu.com/question/30489034)

[stack overflow更详细的讨论](https://stackoverflow.com/questions/24158648/why-isnt-projectname-prefix-pch-created-automatically-in-xcode-6)

