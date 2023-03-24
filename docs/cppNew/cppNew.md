## C++11

- `NULL`,`nullptr`

- `constexpr`

  > `const`既可修饰只读变量，也可修饰常量
  >
  > `constexpr`只用于修饰常量表达式

 - `auto`:用于自动推导已经被初始化后的变量的类型
 - `decltype`:用于自动类型推导,语法为 `
   int a=10; decltype(a) b = 100;`  主要应用于泛型编程中
- `noexcept`

- `&&`:右值引用

- 转移和完美转发：`std::move，std::forward`

- `final`

- `override`

- `默认模板参数`：c++11中允许模板有默认参数

- `using`：定义类型的别名，可以给模板定义别名而`typedef`不行。   

### `lambad`表达式   
  C++11引入了lambda表达式，它是一种匿名函数，可以在代码中直接定义和使用。Lambda表达式的一般形式如下：
  ```css
    [capture list] (parameters) -> return_type { function body }
  ```
  其中：
   - `capture list`：捕获外部变量列表，用于访问lambda表达式外部的变量。
   - `parameters`：参数列表，用于接收函数调用时传递的参数。
   - `return_type`：返回值类型，表示lambda表达式的返回类型。
   - `function body`：函数体，包含lambda表达式的具体实现。   

以下是一个简单的示例：
  ```c++
    #include <iostream>
    int main() {
        int x = 5;
        auto f = [x](int y) -> int {
            return x + y;
        };
        std::cout << f(3) << std::endl;
        return 0;
      }
  ```
在上面的示例中，`lambda`表达式定义了一个函数对象 `f`，它接受一个整数参数 `y`，并返回 `x+y` 的值。`x` 是在`lambda`表达式外部定义的变量，通过捕获列表 `[x] `捕获。

C++11引入`lambda`表达式的主要目的是为了方便`STL`中算法的使用，例如 `std::sort `和 `std::for_each` 等算法，这些算法都接受函数对象作为参数。使用lambda表达式可以避免定义一些单独的函数对象。

`Lambda`表达式的捕获列表用于指定在`lambda`表达式中访问的外部变量。捕获列表可以为空，也可以包含以下三种类型的捕获：
1. 值捕获：捕获外部变量的值，并在lambda表达式中以副本的形式使用。例如：
    ```c++
    int x = 42;
    auto f = [x] { return x; };
    ```
    上述lambda表达式捕获了外部变量 x 的值，并在lambda表达式中使用。
2. 引用捕获：捕获外部变量的引用，并在lambda表达式中以引用的形式使用。例如：    
    ```c++
    int x = 42;
    auto f = [&x] { return x; };
    ```
    上述lambda表达式捕获了外部变量 x 的引用，并在lambda表达式中使用。    
3. 隐式捕获：捕获外部变量的值或引用，并根据变量使用情况自动选择值或引用捕获。隐式捕获有两种形式：   
   - [=]：捕获所有外部变量的值。
   - [&]：捕获所有外部变量的引用。   
 




## C++14

## C++17

## C++20