# Cpp_learning

用于C++项目分享中的知识点记录........

Git学习文档：[添加远程库 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/896043488029600/898732864121440)

开源项目汇总-持续更新：

- [Light-City/CPlusPlusThings: C++那些事 (github.com)](https://github.com/Light-City/CPlusPlusThings)
- [taylorconor/tinytetris: 80x23 terminal tetris! (github.com)](https://github.com/taylorconor/tinytetris)

学习路线：STL `->`  C++11 `->`  设计模式

## 项目一 MytinySTL

> 项目地址：[Alinshans/MyTinySTL: Achieve a tiny STL in C++11 (github.com)](https://github.com/Alinshans/MyTinySTL)
>
> 参考书籍：STL源码剖析
>
> 学习目标：熟练掌握常用容器和算法的使用，分析其设计方式和代码原理。

**Test**       	         // 测试文件夹，代码分析完成情况如下

- [x] test.cpp      // 程序入口
- [x] algorithm_performance_test.h
- [x] algorithm_test.h
- [x] deque_test.h
- [x] list_test.h
- [x] map_test.h
- [x] queue_test.h
- [x] set_test.h
- [x] stack_test.h
- [x] string_test.h
- [x] test.h
- [x] unordered_map_test.h
- [x] unordered_set_test.h
- [x] vector_test.h
- [x] CMakeLists.txt

**MyTinySTL**   //STL实现文件夹

 - [ ] algo.h
 - [ ] allocator.h
 - [ ] deque.h 
 - [x] algorithm.h
 - [x] astring.h
 - [ ] construct.h
 - [x] util.h   这个文件包含一些通用工具，包括 `move, forward, swap` 等函数，以及 `pair` 等 
 - [x] type_traits.h   作者未做实现，使用的是标准库里的`type_traits`

## C++ 11/14/17/20新特性 .....待续

### C++11

- [x] `NULL`,`nullptr`
- [x] `lamada`表达式
- [x]   可调用对象
- [x]  `std::function` 包装器
- [x] `std::bind` 绑定器
 - `auto`:用于自动推导已经被初始化后的变量的类型
 - `decltype`:用于自动类型推导,语法为 `
   int a=10; decltype(a) b = 100;`  主要应用于泛型编程中
- `noexcept`
- `&&`:右值引用
- 转移和完美转发：`std::move，std::forward`
- `final`
- `override`
- `默认模板参数`：c++11中允许模板有默认参数
- `using`：定义类型的别名，可以给模板定义别名而`typedef`不行。s

## 内存相关

![](./docs/static/img/memory.png)

## 设计模式

- [x] 单例模式
- [x] 工厂模式