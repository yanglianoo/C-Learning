## C++11



### `NULL`,`nullptr`

在C++中，`NULL`和`nullptr`都用于表示指针类型的空值。`NULL`是C++早期版本中用来表示空指针的宏定义，通常定义为0或(void*)0。但是这种定义有时会导致类型转换和重载问题。

为了解决这个问题，C++11引入了`nullptr`关键字，它是一个特殊的空指针常量，它的类型是`nullptr_t`，而不是整数或指针类型。这使得`nullptr`比`NULL`更安全，因为它不会导致类型转换和重载问题。

例如：

```c++
void func(char* str) { std::cout << "char* version" << std::endl; }
void func(int i) { std::cout << "int version" << std::endl; }

int main() {
    func(0);  // 调用int版本的func函数，输出 "int version"
    func(NULL);  // 如果NULL被定义为0，则也调用int版本的func函数，输出 "int version"
    func(nullptr);  // 调用char*版本的func函数，输出 "char* version"
    return 0;
}
```

在这个例子中，`func`函数有一个接受`char*`参数的版本和一个接受`int`参数的版本。当我们使用`0`作为参数时，会优先调用`int`版本的`func`函数，而不是`char*`版本的函数。同样地，如果我们使用`NULL`作为参数，如果`NULL`被定义为0，则也会调用`int`版本的`func`函数。这是因为0被解释为int类型，而不是char*类型。

但是，当我们使用`nullptr`作为参数时，编译器能够推断出我们想要调用`char*`版本的函数，因为`nullptr`的类型是空指针常量，而不是整数或指针类型的值。这可以避免类型转换和重载问题，使代码更加清晰和安全。



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



### 可调用对象

在C++中，可调用对象是指可以像函数一样被调用的实体，包括以下几种类型：

1. 函数：包括普通函数、函数模板、函数指针等。

2. 成员函数：包括非静态成员函数和静态成员函数，可以使用类对象或类指针/引用来调用。

3. Lambda表达式：是一种匿名函数对象，可以在语句块内定义。

   ```c++
   #include <iostream>
   int main() {
       auto func = [](int x, int y) -> int { return x + y; };
       int result = func(3, 4);
       std::cout << result << std::endl;  // 输出7
       return 0;
   }
   ```

4. 函数对象：是一种重载了函数调用运算符`operator()`的类对象，可以像函数一样调用。

   ```c++
   #include <iostream>
   class Calculator {
   public:
       int operator()(int x, int y) {
           return x + y;
       }
   };
   int main() {
       Calculator calc;
       int result = calc(3, 4);
       std::cout << result << std::endl;  // 输出7
       return 0;
   }
   ```

5. std::function：是C++11中的一个函数对象包装器，可以将任意可调用对象（函数、函数指针、成员函数指针、lambda表达式等）封装成一个可调用的对象。

6. std::bind：是C++11中的一个函数对象适配器，可以将一个可调用对象转换成另一个可调用对象，同时绑定一部分参数。

7. 函数指针：指向函数的指针，可以像函数一样调用。

   ```c++
   #include <iostream>
   int add(int x, int y) {
       return x + y;
   }
   int main() {
       int (*func)(int, int) = add;
       int result = func(3, 4);
       std::cout << result << std::endl;  // 输出7
       return 0;
   }
   ```



### std::function 包装器

`std::function`是C++11中的一个函数对象包装器，可以用于将任意可调用对象（函数、函数指针、成员函数指针、lambda表达式等）封装成一个可调用的对象。

以下是`std::function`的用法示例：

```c++
#include <iostream>
#include <functional>

void foo(int x, int y) {
    std::cout << x << " + " << y << " = " << x + y << std::endl;
}

class Bar {
public:
    int add(int x, int y) {
        std::cout << x << " + " << y << " = ";
        return x + y;
    }
};

int main() {
    std::function<void(int, int)> func1 = foo;
    func1(3, 4);  // 输出 "3 + 4 = 7"

    Bar bar;
    std::function<int(Bar&, int, int)> func2 = &Bar::add;
    int result = func2(bar, 5, 6);  // 输出 "5 + 6 = "，并返回11
    std::cout << result << std::endl;

    std::function<double(double)> func3 = [](double x) { return x * x; };
    double res = func3(2.5);  // 返回6.25
    std::cout << res << std::endl;

    return 0;
}
```

在上述示例中，我们首先定义了一个普通函数`foo`和一个类`Bar`，其中`Bar`包含一个成员函数`add`。然后，我们使用`std::function`定义了三个可调用对象`func1`、`func2`和`func3`，分别封装了`foo`函数、`Bar::add`成员函数和一个`lambda`表达式。我们可以像调用普通函数一样使用这些`std::function`对象来调用这些可调用对象，并得到正确的结果。

需要注意的是，`std::function`的类型参数包括函数的返回类型和参数列表，使用`auto`关键字可以方便地省略类型。

```c++
#include <iostream>
#include <functional>

void foo(int x, int y) {
    std::cout << x << " + " << y << " = " << x + y << std::endl;
}
int main() {
    auto func = std::function<void(int, int)>(foo);
    func(3, 4);  // 输出 "3 + 4 = 7"
    return 0;
}
```





### std::bind 绑定器

`std::bind` 是一个函数模板，可以用于绑定函数或成员函数的参数，并返回一个新的函数对象，可以延迟调用原始函数，或者改变原始函数的参数顺序。

`std::bind` 的常见用法如下：

1. 绑定函数的参数

   ```c++
   #include <iostream>
   #include <functional>
   
   int add(int x, int y) {
       return x + y;
   }
   
   int main() {
       auto func = std::bind(add, 3, std::placeholders::_1);
       int result = func(4);
       std::cout << result << std::endl;  // 输出7
       return 0;
   }
   ```

   在上面的代码中，我们通过 `std::bind` 绑定了 `add` 函数的第一个参数为 3，第二个参数使用占位符 `std::placeholders::_1`，表示在调用返回的函数对象时需要传入一个参数，该参数将被作为 `add` 函数的第二个参数。最终，我们调用返回的函数对象时传入 4，得到了 `add(3, 4)` 的结果 7。

2. 绑定成员函数的参数

   ```c++
   #include <iostream>
   #include <functional>
   
   class Calculator {
   public:
       int add(int x, int y) {
           return x + y;
       }
   };
   
   int main() {
       Calculator calc;
       auto func = std::bind(&Calculator::add, &calc, 3, std::placeholders::_1);
       int result = func(4);
       std::cout << result << std::endl;  // 输出7
   
       return 0;
   }
   ```

   在上面的代码中，我们通过 `std::bind` 绑定了 `Calculator` 类的 `add` 成员函数的第一个参数为 `&calc`，即指向 `Calculator` 类对象的指针，第二个参数为 3，第三个参数使用占位符 `std::placeholders::_1`，表示在调用返回的函数对象时需要传入一个参数，该参数将被作为 `add` 函数的第三个参数。最终，我们调用返回的函数对象时传入 4，得到了 `calc.add(3, 4)` 的结果 7。

3. 改变函数参数顺序

   ```c++
   #include <iostream>
   #include <functional>
   
   int add(int x, int y) {
       return x + y;
   }
   
   int main() {
       auto func = std::bind(add, std::placeholders::_2, std::placeholders::_1);
       int result = func(3, 4);
       std::cout << result << std::endl;  // 输出7
   
       return 0;
   }
   ```

   在上面的代码中，我们通过 `std::bind` 绑定了 `add` 函数的第一个参数为占位符 `std::placeholders::_2`，表示在调用返回的函数对象时需要传入一个参数，该参数将被作为 `add` 函数的第二个参数；第二个参数为占位符 `std::placeholders::_1`，表示在调用返回的函数对象时需要传入一个参数，该参数将被作为 `add` 函数的第一个参数。最终，我们调用返回的函数对象时传入 3 和 4，

## C++14

## C++17

## C++20