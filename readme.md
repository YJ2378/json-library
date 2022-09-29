# 从零开始的 JSON 库学习

* YJ2378
* 2022/9/20

## 01实现过程中学习到的知识

### json相关知识：

JSON（JavaScript Object Notation）是一个用于数据交换的文本格式，源至于 JavaScript 语言，现时具类似功能的格式有 XML、YAML。
例如，一个动态网页想从服务器获得数据时，服务器从数据库查找数据，然后把数据转换成 JSON 文本格式：
~~~js
{
    "title": "Design Patterns",
    "subtitle": "Elements of Reusable Object-Oriented Software",
    "author": [
        "Erich Gamma",
        "Richard Helm",
        "Ralph Johnson",
        "John Vlissides"
    ],
    "year": 2009,
    "weight": 1.8,
    "hardcover": true,
    "publisher": {
        "Company": "Pearson Education",
        "Country": "India"
    },
    "website": null
}
~~~

网页的脚本代码就可以把此 JSON 文本解析为内部的数据结构去使用。
从此例子可看出，JSON 是树状结构，而 JSON 只包含 6 种数据类型：

* null: 表示为 null
* boolean: 表示为 true 或 false
* number: 一般的浮点数表示方式，在下一单元详细说明
* string: 表示为 "..."
* array: 表示为 [ ... ]
* object: 表示为 { ... }

我们要实现的 JSON 库，主要是完成 3 个需求：

1. 把 JSON 文本解析为一个树状数据结构（parse）。
2. 提供接口访问该数据结构（access）。
3. 把数据结构转换成 JSON 文本（stringify）。

![image](https://user-images.githubusercontent.com/73393686/191437001-14ed21fb-4266-4840-a8f1-54bb1b5de08a.png)


此次任务JSON 的语法子集（空白+一个值+空白）：

~~~
JSON-text = ws value ws
ws = *(%x20 / %x09 / %x0A / %x0D)
value = null / false / true 
null  = "null"
false = "false"
true  = "true"
~~~

参考：https://github.com/miloyip/json-tutorial/blob/master/tutorial01/tutorial01.md

### git的版本管理：
* git主要有四个区域，分别是工作空间、暂存区、本地仓库和远程仓库，具体关系如下图。
![image](https://user-images.githubusercontent.com/73393686/191430745-4473ad1b-7e06-4b37-abdc-b898f55ec135.png)

* 选择一个目录作为工作空间，键入git init 初始化一个本地仓库，此时目录下会多出一个.git的隐藏目录，Git就是通过这个目录里面的内容实现版本控制的。
* git status 命令可以查看工作空间的状态。
* git add file 命令将文件file添加到了暂存区。
* git commit -m "message" 命令将暂存区所有文件提交到本地仓库。
* git branch branch0 命令可以创建一个名为branch0的分支。
* 等等命令...

### ssh的创建与添加：

参考：https://docs.github.com/cn/authentication/connecting-to-github-with-ssh

### cmake的使用：

新建一个CMakeLists.txt文件(注意该文件的命名不能自己随便改，不然编译时会报错)，打开该文件使用cmake语法编写一下内容。然后新建一个名为build的目录，在build目录下执行cmake ..（使用cmake对上一层目录进行编译），在执行make进行编译即可。

参考：https://zhuanlan.zhihu.com/p/415911935

### 关于c中的宏和预处理：

宏的名字必须是唯一的，通常习惯以 _H__ 作为后缀。由于 leptjson 只有一个头文件，可以简单命名为 LEPTJSON_H__。如果项目有多个文件或目录结构，可以用 项目名称_目录_文件名称_H__ 这种命名方式。

.c文件中如果多次引用了.h文件，就会产生重复声明的问题，可以利用如下形式的代码防止重复声明。

~~~
#ifndef LEPTJSON_H__
#define LEPTJSON_H__

/* ... */

#endif /* LEPTJSON_H__ */
~~~

如果宏里有多过一个语句（statement），就需要用 do { /*...*/ } while(0) 包裹成单个语句，否则无法进行正确的预处理。

参考：https://www.runoob.com/cprogramming/c-preprocessors.html

### 断言assert()的使用：

断言（assertion）是 C 语言中常用的防御式编程方式，减少编程错误。最常用的是在函数开始的地方，检测所有参数。有时候也可以在调用函数后，检查上下文是否正确。

C 语言的标准库含有 assert() 这个宏（需 #include <assert.h>），提供断言功能。当程序以 release 配置编译时（定义了 NDEBUG 宏），assert() 不会做检测；而当在 debug 配置时（没定义 NDEBUG 宏），则会在运行时检测 assert(cond) 中的条件是否为真（非 0），断言失败会直接令程序崩溃。

关于何时使用断言，何时抛出异常：如果一个错误是由于程序员错误编码所造成的（例如传入不合法的参数），那么应用断言；如果那个错误是程序员无法避免，而是由运行时的环境所造成的，就要处理运行时错误（例如开启文件失败）。
