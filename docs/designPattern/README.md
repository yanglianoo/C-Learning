> 参考资料：    
>[1] https://subingwen.cn/design-patterns/#%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F-%E6%B5%B7%E8%B4%BC%E7%8E%8B%E7%89%88  
>[2] chatgpt      
  
23种设计模式指的是《设计模式：可复用面向对象软件的基础》（Design Patterns: Elements of Reusable Object-Oriented Software）一书中所介绍的23种常见的软件设计模式，这些模式被分为三种类型：

1. 创建型模式（Creational Patterns）    
    - 工厂方法模式（Factory Method Pattern）   
    - 抽象工厂模式（Abstract Factory Pattern）    
    - 单例模式（Singleton Pattern）    
    - 建造者模式（Builder Pattern）   
    - 原型模式（Prototype Pattern）    
2. 结构型模式（Structural Patterns）    
    - 适配器模式（Adapter Pattern）    
    - 桥接模式（Bridge Pattern）    
    - 组合模式（Composite Pattern）   
    - 装饰模式（Decorator Pattern）    
    - 外观模式（Facade Pattern）    
    - 享元模式（Flyweight Pattern）    
    - 代理模式（Proxy Pattern）   
3. 行为型模式（Behavioral Patterns）    
    - 责任链模式（Chain of Responsibility Pattern）    
    - 命令模式（Command Pattern）    
    - 解释器模式（Interpreter Pattern）   
    - 迭代器模式（Iterator Pattern）    
    - 中介者模式（Mediator Pattern）    
    - 备忘录模式（Memento Pattern）   
    - 观察者模式（Observer Pattern）   
    - 状态模式（State Pattern）   
    - 策略模式（Strategy Pattern）   
    - 模板方法模式（Template Method Pattern）    
    - 访问者模式（Visitor Pattern）   
# 1.单例模式     

```diff
+---------------------------------+
|             Singleton           |
+---------------------------------+
| - instance: Singleton*          |
+---------------------------------+
| + getInstance(): Singleton*     |
+---------------------------------+
| - Singleton()                   |
| - Singleton(const Singleton&)   |
| - operator=(const Singleton&)  |
+---------------------------------+
```
 - `Singleton` 类表示单例模式的实现。
 - `instance` 是指向 `Singleton` 类的唯一实例的指针，必须是 private，因此无法从外部直接访问。
 - `getInstance()` 是静态方法，用于获取唯一的 `Singleton` 实例。它使用懒汉模式，即只有在需要时才创建实例。
 - `Singleton` 的默认构造函数、拷贝构造函数和赋值运算符都被声明为 `private`，以确保 `Singleton` 实例只能在内部创建和修改，从而保证单例模式的正确性。

在一个项目中，全局范围内，某个类的实例有且仅有一个，通过这个唯一实例向其他模块提供数据的全局访问，这种模式就叫单例模式。

由于类的实例全局唯一，因此不支持在类外创建新的实例，所以需要手动在类里禁用掉默认的拷贝构造和拷贝赋值函数，防止使用者调用生成新的类实例。禁用的方式有两种，一种是直接显示的删除在`public`中使用`= delete`来删除，第二种方式就是将拷贝构造和拷贝赋值函数声明为私有的。

同时需要创建全局唯一的实例，因此在类中声明一个**静态变量指针**来存储这个实例，类中的静态变量成员不能在类中赋值，需要在类外赋值初始化。当这个指针在类加载时直接初始化了就是饿汉模式，类指针在加载时被声明为`nullptr`则是懒汉模式。

## 1.1 饿汉模式

```c++
// 饿汉模式
class TaskQueue
{
public:
    // = delete 代表函数禁用, 也可以将其访问权限设置为私有
    TaskQueue(const TaskQueue& obj) = delete;
    TaskQueue& operator=(const TaskQueue& obj) = delete;
    static TaskQueue* getInstance()
    {
        return m_taskQ;
    }
private:
    TaskQueue() = default;
    static TaskQueue* m_taskQ;
};
// 静态成员初始化放到类外部处理
TaskQueue* TaskQueue::m_taskQ = new TaskQueue; //直接创建全局唯一实例

int main()
{
    TaskQueue* obj = TaskQueue::getInstance();
}
```

## 1.2 懒汉模式

```c++
// 懒汉模式
class TaskQueue
{
public:
    // = delete 代表函数禁用, 也可以将其访问权限设置为私有
    TaskQueue(const TaskQueue& obj) = delete;
    TaskQueue& operator=(const TaskQueue& obj) = delete;
    static TaskQueue* getInstance()
    {
        if(m_taskQ == nullptr)
        {
            m_taskQ = new TaskQueue;
        }
        return m_taskQ;
    }
private:
    TaskQueue() = default;
    static TaskQueue* m_taskQ;
};
TaskQueue* TaskQueue::m_taskQ = nullptr; //初始化为nullptr
```

在调用` getInstance() `函数获取单例对象的时候，如果在单线程情况下是没有什么问题的，如果是多个线程，调用这个函数去访问单例对象就有问题了。假设有三个线程同时执行了`getInstance()` 函数，在这个函数内部每个线程都会 new 出一个实例对象。此时，这个任务队列类的实例对象不是一个而是 3 个，很显然这与单例模式的定义是相悖的。

### 1.2.1 线程安全问题

对于饿汉模式是没有线程安全问题的，在这种模式下访问单例对象的时候，这个对象已经被创建出来了。要解决懒汉模式的线程安全问题，用到的方法有多种，这里提供一种双重锁机制的代码
```c++
class TaskQueue
{
public:
    TaskQueue(const TaskQueue& obj) = delete;
    TaskQueue& operator=(const TaskQueue& obj) = delete;
    static TaskQueue* getInstance()
    {
        if (m_taskQ == nullptr)
        {
            std::lock_guard<std::mutex> lock(m_mutex);  // 1
            if (m_taskQ == nullptr)                     // 2
            {
                m_taskQ = new TaskQueue;                // 3
            }
        }
        return m_taskQ;
    }
private:
    TaskQueue() = default;
    static TaskQueue* m_taskQ;
    static std::mutex m_mutex;
};
TaskQueue* TaskQueue::m_taskQ = nullptr;
std::mutex TaskQueue::m_mutex;
```
具体解释如下：

1. 在进入临界区之前，使用 `std::lock_guard` 对象来锁定互斥量，这样可以保证同一时刻只有一个线程进入临界区。
2. 由于多个线程可能在 `if (m_taskQ == nullptr)` 处等待，所以需要再次检查指针是否为 `nullptr`，如果是，则进入临界区创建实例，否则直接返回指针。
3. 创建实例后，指针不再为 `nullptr`，临界区结束，`std::lock_guard` 对象自动释放互斥量。

# 2. 工厂模式
## 2.1 简单工厂模式
```diff
+----------------+       +-----------------------+
|     Product    |       |         Factory       |
+----------------+       +-----------------------+
|  +use(): void  |       |  +createProduct(): ... |
+----------------+       +-----------------------+
        ▲                           ▲
        |                           |
+----------------+       +-----------------------+
| ConcreteProductA|       | ConcreteProductB       |
+----------------+       +-----------------------+
|  +use(): void  |       |  +use(): void          |
+----------------+       +-----------------------+
```
```c++
#include <iostream>
#include <string>

// 定义一个抽象的产品类
class Product
{
public:
    virtual ~Product() {}
    virtual void use() = 0;
};

// 定义两个具体的产品类
class ConcreteProductA : public Product
{
public:
    void use() override
    {
        std::cout << "Using ConcreteProductA" << std::endl;
    }
};

class ConcreteProductB : public Product
{
public:
    void use() override
    {
        std::cout << "Using ConcreteProductB" << std::endl;
    }
};

// 定义一个简单工厂类
class Factory
{
public:
    // 工厂方法，用于创建产品对象
    Product* createProduct(const std::string& type)
    {
        if (type == "A")
        {
            return new ConcreteProductA();
        }
        else if (type == "B")
        {
            return new ConcreteProductB();
        }
        else
        {
            return nullptr;
        }
    }
};

int main()
{
    Factory factory;

    // 通过工厂方法创建不同的产品对象
    Product* productA = factory.createProduct("A");
    Product* productB = factory.createProduct("B");

    // 使用产品对象
    productA->use();
    productB->use();

    // 释放资源
    delete productA;
    delete productB;

    return 0;
}
```
解释一下：
 - `Product` 是一个抽象的产品类，它定义了产品的接口，这里只有一个纯虚函数 use()，用于使用产品。
 - `ConcreteProductA` 和 `ConcreteProductB` 是具体的产品类，它们实现了 `Product` 接口中的函数。
 - `Factory` 是一个简单工厂类，它有一个工厂方法 `createProduct()`，用于根据客户端的需求创建不同的产品对象。
 - 在 `main()` 函数中，首先创建了一个 `Factory` 对象，然后通过工厂方法创建了两个不同的产品对象，并调用它们的 `use()` 方法，最后释放资源。


## 2.2 工厂模式
```diff
+------------------+         +------------------+
|     IFactory     |         |    Operation     |
+------------------+         +------------------+
|+ createOperation |         |+ getResult()     |
+------------------+         +------------------+
            ^                             ^
            |                             |
+------------------+         +------------------+
|    AddFactory    |         |   AddOperation   |
+------------------+         +------------------+
|+ createOperation |         |+ getResult()     |
+------------------+         +------------------+
            ^
            |
+------------------+         +------------------+
|    SubFactory    |         |   SubOperation   |
+------------------+         +------------------+
|+ createOperation |         |+ getResult()     |
+------------------+         +------------------+
```

```c++
    // 抽象产品类
class Operation {
public:
    virtual double getResult(double numA, double numB) = 0;
};

// 具体产品类 - 加法
class AddOperation : public Operation {
public:
    double getResult(double numA, double numB) override {
        return numA + numB;
    }
};

// 具体产品类 - 减法
class SubOperation : public Operation {
public:
    double getResult(double numA, double numB) override {
        return numA - numB;
    }
};

// 抽象工厂类
class IFactory {
public:
    virtual Operation* createOperation() = 0;
};

// 具体工厂类 - 加法工厂
class AddFactory : public IFactory {
public:
    Operation* createOperation() override {
        return new AddOperation();
    }
};

// 具体工厂类 - 减法工厂
class SubFactory : public IFactory {
public:
    Operation* createOperation() override {
        return new SubOperation();
    }
};

// 客户端代码
int main() {
    IFactory* factory = new AddFactory();  // 创建加法工厂
    Operation* operation = factory->createOperation();  // 创建加法对象
    double result = operation->getResult(1, 2);  // 3.0
    delete operation;
    delete factory;

    factory = new SubFactory();  // 创建减法工厂
    operation = factory->createOperation();  // 创建减法对象
    result = operation->getResult(1, 2);  // -1.0
    delete operation;
    delete factory;

    return 0;
}

```

工厂模式（Factory Method Pattern）和简单工厂模式（Simple Factory Pattern）都是创建型设计模式，用于创建对象。它们的主要区别在于：

1. 抽象程度不同   
简单工厂模式是由一个工厂类根据传入的参数决定创建哪种产品对象，它不属于 GoF（四人帮）的 23 种设计模式之一。   
工厂方法模式则是针对每种产品都提供一个工厂类，通过工厂类来创建相应的产品对象。工厂方法模式是 GoF 23 种设计模式之一。

2. 扩展性不同   
简单工厂模式需要修改工厂类的代码，才能支持新产品的创建，违背了开闭原则。而工厂方法模式是通过增加相应的工厂类来支持新产品的创建，符合开闭原则。

3. 灵活性不同  
在简单工厂模式中，工厂类是完全掌控整个创建过程的，客户端只需要提供参数即可，无法控制对象的创建过程。而在工厂方法模式中，客户端通过选择相应的工厂来控制对象的创建过程，具有更高的灵活性。

## 2.3 抽象工厂模式