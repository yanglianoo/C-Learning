## **`typedef` 和 `typename`**

  >  `typedef` 用来定义别名，`typename`用来告诉编译器后面的东西为一个类型即`type`

## **拷贝构造和拷贝赋值**

  > 拷贝赋值用于赋值。
  >
  > ```c++
  > Person p;
  > Person p1;
  > p = p1;//此时调用拷贝构造函数
  > ```
  >
  > - 初始化是对于一个新的对象来说的，在构造这个对象的时候，给这片内存一个初始值
  > - 赋值是对于一个已经存在的对象来说的，给一个已经存在的对象重新覆盖值

  > 拷贝构造的调用有如下几个场景：
  >
  > - 用`=`初始化一个新的类： `Person p = p1;`，此时的`=`代表初始化，并非赋值。
  >
  > - 显示调用拷贝构造函数来初始化一个新的类：`Person p(p1)`
  >
  > - 函数传参时，传入一个类的实参会导致拷贝构造生成一个临时对象：
  >
  >   ```c++
  >   void f(Person p1) { ... }
  >   Person p;
  >   f(p)
  >   ```

## **c++在函数后加const的意义**

  > 我们定义的类的[成员函数](https://so.csdn.net/so/search?q=成员函数&spm=1001.2101.3001.7020)中，常常有一些成员函数不改变类的数据成员，也就是说，这些函数是"只读"函数，而有一些函数要修改类数据成员的值。如果把不改变数据成员的函数都加上const关键字进行标识，显然，可提高程序的可读性。其实，它还能提高程序的可靠性，**已定义成const的成员函数，一旦企图修改数据成员的值，则编译器按错误处理**。       
  >const成员函数和const对象 实际上，const成员函数还有另外一项作用，即常量对象相关。对于内置的数据类型，我们可以定义它们的常量，用户自定义的类也一样，可以定义它们的常量对象。   
  >  - 1、非静态成员函数后面加const（加到非成员函数或静态成员后面会产生编译错误）   
  >  - 2、表示成员函数隐含传入的this指针为const指针，决定了在该成员函数中， 任意修改它所在的类的成员的操作都是不允许的（因为隐含了对this指针的const引用）；   
  >  - 3、唯一的例外是对于mutable修饰的成员。加了const的成员函数可以被非const对象和const对象调用，但不加const的成员函数只能被非const对象调用     


## **C语言实现面向对象**

- **静态绑定**

  - 在这个例子中，我们使用结构体定义了一个Person对象，并在结构体中定义了一个函数指针，用于指向对象的方法。我们还定义了一个say_hello函数，用于实现对象的方法。
  - 在main函数中，我们创建了一个Person对象，并调用了它的方法。通过这种方式，我们就可以在C语言中实现一个简单的面向对象编程的例子。      

  ```c
  #include <stdio.h>
  
  // 定义一个结构体来表示Person对象
  typedef struct Person {
      char* name;
      int age;
      void (*say_hello)(struct Person*); // 函数指针，用于定义对象的方法
  } Person;
  
  // 定义一个say_hello方法，用于打印Person对象的信息
  void say_hello(Person* p) {
      printf("Hello, my name is %s and I am %d years old.\n", p->name, p->age);
  }
  
  int main() {
      // 创建一个Person对象
      Person p = {"Alice", 25, &say_hello};
  
      // 调用对象的方法
      p.say_hello(&p);
  
      return 0;
  }
  ```

- 动态绑定

  - 在上面的代码中，我们使用了函数指针来实现对象的方法，这种方式在编译时就确定了函数的调用地址，也称为静态绑定。如果想要实现动态绑定，需要在运行时根据对象的类型动态确定方法的调用地址。    


  - 实现动态绑定的一种常见方式是使用虚函数表（Virtual Function Table，简称vtable），也称为动态绑定表。虚函数表是一个指向函数指针数组的指针，每个对象都有一个指向自己的虚函数表指针。虚函数表中存储了对象所属的类的所有虚函数的地址，派生类可以重载父类的虚函数，通过虚函数表指针，可以实现基类指针指向派生类对象时的动态绑定。

- 在下面这个例子中，我们定义了一个虚函数表，并在`Person`和`Student`结构体中添加了一个指向虚函数表的指针`vptr`。虚函数表中存储了`say_hello`函数的地址。在创建`Person`对象和`Student`对象时，分别初始化了它们的虚函数表指针，使得它们可以动态绑定到不同的方法。在调用对象的方法时，通过虚函数表指针实现了动态绑定。

  ```c
  #include <stdio.h>
  
  // 定义一个结构体来表示Person对象
  typedef struct Person {
      char* name;
      int age;
      struct vtable* vptr; // 虚函数表指针
  } Person;
  
  // 定义虚函数表
  typedef struct vtable {
      void (*say_hello)(Person*);
  } vtable;
  
  // 定义一个say_hello方法，用于打印Person对象的信息
  void say_hello(Person* p) {
      printf("Hello, my name is %s and I am %d years old.\n", p->name, p->age);
  }
  
  // 定义一个Student对象，继承自Person
  typedef struct Student {
      char* name;
      int age;
      char* school;
      struct vtable* vptr; // 虚函数表指针
  } Student;
  
  // 重载say_hello方法
  void student_say_hello(Student* s) {
      printf("Hello, my name is %s and I am %d years old. I study at %s.\n", s->name, s->age, s->school);
  }
  
  int main() {
      // 创建一个Person对象
      vtable person_vtable = {&say_hello};
      Person p = {"Alice", 25, &person_vtable};
  
      // 创建一个Student对象
      vtable student_vtable = {&student_say_hello};
      Student s = {"Bob", 20, "University of ABC", &student_vtable};
  
      // 调用对象的方法
      p.vptr->say_hello(&p);
      s.vptr->say_hello((Person*)&s);
  
      return 0;
  }
  
  ```

  `&student_say_hello` 实际上就是一个指向 `Student::student_say_hello` 函数的指针，它的类型是 `void (*)(Person*)`，即一个参数为 `Person*` 类型，返回值为 `void` 类型的函数指针。由于 `(*say_hello)(Person*)` 类型的函数指针和 `void (*)(Person*)` 类型的函数指针是兼容的，因此可以将 `&student_say_hello` 赋值给 `(*say_hello)(Person*)` 类型的函数指针。