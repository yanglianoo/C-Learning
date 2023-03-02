## STL（Standard Template Library）

STL 是 **C++ 标准库的一部分**，不用单独安装。C++ STL 借助模板（Template）把常用的 **数据结构**及其**算法**都实现了一遍，并且做到了数据结构和算法的**分离**（GP **vs.** OOP）。

C++标准库以头文件的形式呈现：

- 新式C++头文件不带`.h` 后缀，如`#include<vector>`
- 旧式C++头文件带`.h` 后缀，如`#include<stdio.h>`
- 新式头文件内的组件封装于`namespace std`
- 旧式头文件内的组件不封装于`namespace std`

## 六大组件

1. 分配器（Allocators）：内存管理。

2. 迭代器（Iterators）：泛化的指针，算法通过迭代器访问容器中的数据。

3. 容器（Containers）：封装了大量常用的数据结构。

4. 算法（Algorithms）：定义了一些常用算法，处理数据。

5. 仿函数（Functors）：具有函数特质的对象（重载`operator()`的类）。

6. 适配器（Adapters）：修改接口。

   <img src="../static/img/cpp-stl-6-components.png" alt="C++ STL 六大组件 – 人人都懂物联网" style="zoom:80%;" />

## 容器的分类与结构

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

<!-- <img src="../static/img/containers.png" alt="img" style="zoom:80%;" /> -->
 ![](../static/img/containers.png)
## 测试

### 主要文件结构

```c++
MyTinySTL
│      alloc.h       // 分配器
│      allocator.h   
│      construct.h
│      uninitialized.h
│      memory.h
│      iterator.h    // 迭代器
│      type_traits.h // 萃取器
│      list.h        // 容器
│      vector.h
│      deque.h
│      rb_tree.h
│      set.h
│      map.h
│      hashtable.h
│      unordered_map.h
│      unordered_set.h
│      astring.h
│      basic_string.h
│      queue.h
│      stack.h
│      algo.h		 // 算法
│      algobase.h
│      algorithm.h
│      numeric.h
│      heap_algo.h
│      set_algo.h
│      functional.h  //仿函数
│      exceptdef.h   //其他
│      util.h
│
└─Test       	     // 测试文件
    │  test.cpp      // 程序入口
    │  algorithm_performance_test.h
    │  algorithm_test.h
    │  deque_test.h
    │  list_test.h
    │  map_test.h
    │  queue_test.h
    │  set_test.h
    │  stack_test.h
    │  string_test.h
    │  test.h
    │  unordered_map_test.h
    │  unordered_set_test.h
    │  vector_test.h
    │  CMakeLists.txt
    │  README.md
```

### 测试主程序

```c++
int main()
{

  using namespace mystl::test;

  std::cout.sync_with_stdio(false);

  //算法测试: 包含了 mystl 的 81 个算法测试 algorithm_test.h
  RUN_ALL_TESTS();
  // 仅仅针对 sort, binary_search 做了性能测试 algorithm_performance_test.h
  algorithm_performance_test::algorithm_performance_test();
  // vector test : 测试 vector 的接口与 push_back 的性能
  vector_test::vector_test();
  // list test : 测试 list 的接口与 insert, sort 的性能
  list_test::list_test();
  // deque test : 测试 deque 的接口和 push_front/push_back 的性能
  deque_test::deque_test();
  // queue test : 测试 queue, priority_queue 的接口和它们 push 的性能
  queue_test::queue_test();
  queue_test::priority_test();
  // stack test : 测试 stack 的接口 和 push 的性能
  stack_test::stack_test();
  // map test : 测试 map, multimap 的接口与它们 insert 的性能
  map_test::map_test();
  map_test::multimap_test();
  // set test : 测试 set, multiset 的接口与它们 insert 的性能
  set_test::set_test();
  set_test::multiset_test();
  // unordered_map test : 测试 unordered_map, unordered_multimap 的接口与它们 insert 的性能
  unordered_map_test::unordered_map_test();
  unordered_map_test::unordered_multimap_test();
  // unordered_set test : 测试 unordered_set, unordered_multiset 的接口与它们 insert 的性能
  unordered_set_test::unordered_set_test();
  unordered_set_test::unordered_multiset_test();
  // string test : 测试 string 的接口和 insert 的性能
  string_test::string_test();

#if defined(_MSC_VER) && defined(_DEBUG)
  _CrtDumpMemoryLeaks();
#endif // check memory leaks
}
```

**关于宏的几点：**

1. 特殊符号 `#` 和 `##`

   `#` ： 预处理时，将`#`后连接的实参字符串化

   `##` ：一种分隔连接方式，它的作用是先分隔，然后进行强制连接。在普通的宏定义中，预处理器一般把空格解释成分段标志，对于每一段和前面比较，相同的就被替换。但是这样做的结果是，被替换段之间存在一些空格。如果我们不希望出现这些空格，就可以通过添加一些`##`来替代空格。

例如：

```c++
#define TYPE1(type,name)    name_##type##_type
#define TYPE2(type,name)    name##_##type##_type

TYPE1(int, c); // int 　name_int_type; (因为##号将后面分为 name_ 、type 、 _type三组，替换后强制连接)
TYPE2(int, d); // int 　d_int_type;    (因为##号将后面分为 name、_、type 、_type四组，替换后强制连接)
```

 2. `#define  .....  do{   }while(0)`

    由于宏是直接拼接，当出现如下情况时，就会出现逻辑错误：

```c++
  #define  print  print1();printf(2)
  if(0)
  	print
  else
  	return 0;
//此时，由于if逻辑判断后未加括号，所以上述代码会被替换为
  if(0)
  	print1();
	print2();
  else
  	return 0;
//此时就会出现错误，加入do{ }while(0)后就能避免出现上述的错误
```

​	3.巧妙利用宏的特性：文本替换拼接

```c++
void TESTCASE_NAME(testcase_name)::Run()

/*
Run()后边没有写实现，是为了用宏定义将测试用例放入到 Run 的实现里，例如：
TEST(copy_test)
{
EXPECT_EQ(3, Add(1, 2));
EXPECT_EQ(2, Add(1, 1));
}
上述代码将 { EXPECT_EQ(3, Add(1, 2)); EXPECT_EQ(2, Add(1, 1)); } 接到 Run() 的后面
*/
```

​	作者在写`Run`这个成员函数时，并没有写实现，这是因为此时的成员函数被定义为一个宏，所以可以不用加`{ }`，在调用的时候，使用如上的方式，即在后面加`{ ......Run函数实现 }`，直接拼接在一起，很是巧妙。

### Test文件夹

- **algorithm_performance_test.h**

测试了排序算法和二分查找算法，对比了STL中提供的`sort`和`binary_search`。发现代码`bug`，在二分查找测试时没有进行排序，代码如下

```c++
#define FUN_TEST2(mode, fun, count) do {                       \
    std::string fun_name = #fun;                               \
    srand((int)time(0));                                       \
    char buf[10];                                              \
    clock_t start, end;                                        \
    int *arr = new int[count];                                 \
    for(size_t i = 0; i < count; ++i)  *(arr + i) = rand();    \                                                                                   
    start = clock();                                           \
    for(size_t i = 0; i < count; ++i)                          \
        bool a=mode::fun(arr, arr + count, rand());            \ //出现错误的地方，没有对数组进行排序而直接二分查找
    end = clock();                                             \
    int n = static_cast<int>(static_cast<double>(end - start)  \
        / CLOCKS_PER_SEC * 1000);                              \
    std::snprintf(buf, sizeof(buf), "%d", n);                  \
    std::string t = buf;                                       \
    t += "ms   |";                                             \
    std::cout << std::setw(WIDE) << t;                         \
    delete []arr;                                              \
} while(0)
```

- 其余测试文件，大多是调用不同容器的构造函数，对容器的提供的接口进行测试。

## MytinySTL

### `util.h`

这个文件包含一些通用工具，包括 `move, forward, swap` 等函数，以及 `pair` 等

- 数组的引用

  ```c++
  //数组的引用
  template <class Tp, size_t N>
  void swap(Tp(&a)[N], Tp(&b)[N]) 
  {
    mystl::swap_range(a, a + N, b);
  }
  //形参中的(&a) 代表数组a的引用，[N],代表数组的大小,要指定数组大小，如果没有后面的数组大小，不知道是变量还是数组
  ```

  

