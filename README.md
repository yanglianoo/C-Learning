# Cpp_learning

用于C++项目分享中的知识点记录........

Github Token：**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**

## 项目一 MytinySTL

> 项目地址：[Alinshans/MyTinySTL: Achieve a tiny STL in C++11 (github.com)](https://github.com/Alinshans/MyTinySTL)
>
> 参考书籍：STL源码剖析

### STL（Standard Template Library）

STL 是 **C++ 标准库的一部分**，不用单独安装。C++ STL 借助模板（Template）把常用的 **数据结构**及其**算法**都实现了一遍，并且做到了数据结构和算法的**分离**（GP **vs.** OOP）。

C++标准库以头文件的形式呈现：

- 新式C++头文件不带`.h` 后缀，如`#include<vector>`
- 旧式C++头文件带`.h` 后缀，如`#include<stdio.h>`
- 新式头文件内的组件封装于`namespace std`
- 旧式头文件内的组件不封装于`namespace std`

### 六大组件

1. 分配器（Allocators）：内存管理。

2. 迭代器（Iterators）：泛化的指针，算法通过迭代器访问容器中的数据。

3. 容器（Containers）：封装了大量常用的数据结构。

4. 算法（Algorithms）：定义了一些常用算法，处理数据。

5. 仿函数（Functors）：具有函数特质的对象（重载`operator()`的类）。

6. 适配器（Adapters）：修改接口。

   <img src="https://static.getiot.tech/cpp-stl-6-components.png" alt="C++ STL 六大组件 – 人人都懂物联网" style="zoom:80%;" />

### 容器的分类与结构

- 顺序容器：
  - Array：长度固定的数组，存储空间连续，支持随机访问
  - Vector：动态数组，存储空间连续，支持随机访问
  - Deque：双端队列，存储空间分段连续，支持随机访问
  - List：双向链表，存储空间不连续，不支持随机访问
  - Forward-List：单向链表，存储空间不连续，不支持随机访问
- 关联容器：红黑树实现，有序。Multi的key可以重复
  - Set/MultiSet
  - Map/MultiMap
- 无序容器：哈希表（Separate Chaining）实现
  - Unoedered Set/MultiSet
  - Unoedered Map/MultiMap

<img src="https://images2018.cnblogs.com/blog/1363151/201805/1363151-20180512191511648-165743457.png" alt="img" style="zoom:80%;" />
