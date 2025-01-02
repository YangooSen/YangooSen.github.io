---
title: effectiveCPP
katex: true
date: 2024-06-08 10:02:04
excerpt: 《effective c++》读书笔记
tag: cpp
---
> C++ 自从诞生以来，经过了多个版本的发展，每个版本都引入了新的特性和改进，增强了语言的功能和性能。以下是 C++ 主要版本的发展历程：
> 
> C++98（1998）
> 这是第一个标准化的 C++ 版本，标志着 C++ 成为国际标准。C++98 的主要特性包括：
> 
> - 类和对象的支持
> - 多重继承
> - 运算符重载
> - 模板
> - 异常处理
> - 标准模板库（STL），包括容器、迭代器、算法和函数对象
> 
> C++03（2003）
> 这是对 C++98 的一个小幅修订版本，主要修正了 C++98 标准中的一些缺陷和模糊之处。C++03 没有引入新的语言特性。
> 
> C++11（2011）
> 这是一次重大更新，给 C++ 带来了许多新特性和改进，大大增强了语言的表达能力和性能。主要特性包括：
> 
> - 自动类型推断（`auto` 关键字）
> - 智能指针（`std::unique_ptr` 和 `std::shared_ptr`）
> - lambda 表达式
> - 移动语义（`move` 语义和 `std::move` 函数）
> - 并发支持（`<thread>` 库）
> - 范围基于 for 循环
> - 常量表达式（`constexpr` 关键字）
> - 新的标准库组件（如 `std::array`, `std::tuple`, `std::unordered_map` 等）
> 
> C++14（2014）
> 这是对 C++11 的一次增量更新，主要对 C++11 特性进行完善和改进。主要特性包括：
> 
> - 通用 lambda 捕获（lambda 表达式中使用 `auto` 捕获）
> - 变量模板
> - `std::make_unique` 工厂函数
> - 扩展的 `constexpr`
> 
> C++17（2017）
> 这是又一次重要的更新，进一步提升了语言的功能和性能。主要特性包括：
> 
> - 可选值类型（`std::optional`）
> - 变长模板参数包展开
> - 文件系统库（`<filesystem>`）
> - 并行算法（`<execution>`）
> - 内联变量（`inline` 变量）
> - `if` 和 `switch` 语句中的初始化语法
> - 结构化绑定（解构绑定）
> 
> C++20（2020）
> 这是一次大规模的更新，带来了许多新的语言特性和库组件。主要特性包括：
> 
> - 模块（Modules）
> - 协程（Coroutines）
> - 概念（Concepts）
> - 范围库（Ranges）
> - 三路比较运算符（`<=>`，宇宙飞船操作符）
> - 日志库（`<format>`）
> - 扩展的 constexpr
> - 异步支持（`std::jthread`，取消机制）
> 
> C++23（2023）
> C++23 标准在进一步完善和扩展语言特性方面做了许多工作。主要特性包括：
> 
> - 静态反射
> - 更完善的模式匹配
> - 标准库扩展，包括新容器和算法
> - 增强的并发和异步支持
> 
> 未来版本
> C++ 标准委员会（ISO/IEC JTC 1/SC 22/WG 21）将继续推动 C++ 的发展，预计未来版本将进一步增强语言的现代化特性和性能，满足不断变化的编程需求。
> 
> C++ 的发展历程展示了语言的不断演进和优化，适应了现代软件开发的需求，同时保留了其高性能和灵活性的核心优势。

> 在 C++ 中，`::` 是**作用域解析运算符**，用于指定作用域或全局命名空间的元素。当 `::` 前没有任何限定时，表示从**全局命名空间**中查找符号。这意味着在当前作用域中没有找到符号时，它会直接查找全局作用域中的符号。
> 
> 具体含义如下：
> 
> 1. **调用全局函数或全局变量**：
>    如果你在局部作用域中有与全局作用域同名的变量或函数，但你想调用全局作用域中的函数或变量，可以使用 `::` 来明确指示查找全局命名空间中的定义。例如：
> 
>    ```cpp
>    int x = 10;  // 全局变量
> 
>    void foo() {
>        int x = 20;  // 局部变量
>        std::cout << x << std::endl;   // 输出 20，局部变量
>        std::cout << ::x << std::endl; // 输出 10，全局变量
>    }
>    ```
> 
>    在这个例子中，`::x` 明确指示要使用全局作用域中的变量 `x`。
> 
> 2. **调用全局的 `operator new`**：
>    在自定义类的 `operator new` 时，常常会使用 `::operator new` 来调用全局默认的 `operator new`，确保调用的是标准库中的内存分配函数。例如：
> 
>    ```cpp
>    class MyClass {
>    public:
>        void* operator new(size_t size) {
>            std::cout << "Custom operator new\n";
>            return ::operator new(size);  // 调用全局的 new 操作符
>        }
>    };
>    ```
> 
>    这里的 `::operator new` 表示调用标准库提供的默认全局 `operator new`，而不是递归调用自定义的 `operator new`。
> 
> ### 总结
> 当 `::` 前没有任何限定时，表示查找全局命名空间中的符号，通常用于在局部作用域中避免名称冲突，或者明确调用全局作用域中的函数或变量。


# introduction

## Terminology

- A `declaration` tells compiliers about the name and the type of something, but it omits certain details.
```cpp
extern int x; //declaration

class Widget;
```
- A `definition` provides compilers with the details a declaration omits. For an object, the definition is where compilers set aside memory for the object
```cpp
int x  //definition
class Widget{
public:
    Widget();
    ~Widget();
};
```

- `Initialization` is the process of giving an object its first value.

> Unless you have a good reason for allowing a ctor to be used for implicit type conversions, you declare it `explicit`

- The `copy ctor` is used to **initialize** an object with a different object of the same type (new object), and `copy assignment operator (operator=)` is used to **copy** the value from one object to another of the same type (no new object).

> `copy ctor`is called when an object passed by value

# 1.Accutoming Yourself to C++

## Item1: View C++ as a federation of languages

View C++ as a federation of 4 sublanguage:
- C
- Object-Oriented C++
- Template C++
- The STL

## Item2: Prefer constS, enumS, and inlineS to #defineS

It better be called "prefer the compilier to preprocessor"

![pipeline](1-pipeline.png)


```cpp
#define ratio 1.6     //change
const double ratio=1.6
```

- #define can be processed by preprocessor, "ratio" may never be seen by compilier. This can be confused if you get error during compilation.
- #define cost more memory.That's because the preprocessor blind substitution of the macro name with 1.6, it result in multiple copies of 1.6 in object code.
- There is no "private" #define, no way to create a class-specific constant using a #define

```cpp
const std::string name("ys");

class foo{
private:
    static const int num=5 //it's a declare not definition,there is no memory allocation
    int score[num];         //use
};

```
> Older compilers may not accept the syntax above, because you can not provide initial value at its point of declaration.

> Usually, C++ requires that you provide a definition for anything you usem but **class-specific constants that are static and of integral type are an exception**. As long as you donot take their address, you can declare them and use them without providing a definition. If you do take the address of a class constant, You provide a separate definition:


```cpp
const int foo::num; //definition. 
//initial value of class constant is provided where the constant is declared (5), 
//no initial value is permitted at the point of definition
```

in cases where the above syntax cant't be used, you put the initial value at the point of definition:

```cpp
class foo{
    static const double num;
    //... 
};

const double foo::num=1.35;

```

This is all need almost all the time. The only exception is **when you need the value of a class constant during compilation of the class**, such as in the declaration of the array (where compilers insist on knowing the size of the array during compilation). Then the accepted way to compensate for compilers that fobid the in-class specification of initial value for static integral class constants is to use what is known as `the enum hack`


> C++ 中的枚举类型（`enum`）是一种用户定义的数据类型，用于将一组整数常量命名为易读的符号。它可以帮助代码更加简洁易读，同时避免直接使用魔法数字（magic numbers）。枚举主要分为两种：**传统枚举（C风格枚举）** 和 **类枚举（C++11 引入的）**。
> 
> ### 1. 传统枚举（C风格枚举）
> 
> ```cpp
> enum Color { RED, GREEN, BLUE };
> ```
> 
> #### 特点：
> - 默认情况下，枚举中的第一个枚举量值为 0，后续枚举量的值在前一个枚举值的基础上递增 1。
>   - 在上面的例子中，`RED=0`, `GREEN=1`, `BLUE=2`。
> - 可以为枚举量显式指定值：
>   ```cpp
>   enum Color { RED = 2, GREEN = 4, BLUE = 8 };
>   ```
>   - 此时，`RED=2`, `GREEN=4`, `BLUE=8`。
> - 枚举类型的变量可以用作整数，并且可以与整数进行比较和运算：
>   ```cpp
>   Color color = RED;
>   if (color == 0) {
>       // 等同于 color == RED
>   }
>   ```
> 
> #### 限制：
> - 枚举量是全局的，命名空间较为混乱。如果在不同的枚举中使用了相同的枚举量名（如 `RED`），可能会产生冲突。
> - 它们可以被隐式转换为整数类型，可能会引发类型不安全的问题。
> 
> ### 2. 类枚举（C++11 引入的枚举类）
> 
> 为了克服传统枚举类型的缺点，C++11 引入了 **类枚举**（`enum class`）。类枚举增强了类型安全性和可读性，防止隐式转换为整数。
> 
> ```cpp
> enum class Color { RED, GREEN, BLUE };
> ```
> 
> #### 特点：
> - **作用域控制**：枚举量不能隐式转换为整数，且只能通过枚举的作用域访问。这意味着不能直接访问 `RED`，必须使用 `Color::RED`。
>   ```cpp
>   Color color = Color::RED;
>   ```
> - **类型安全**：类枚举不能隐式转换为整数，需要显式转换。
>   ```cpp
>   int redValue = static_cast<int>(Color::RED);
>   ```
> - **枚举底层类型**：可以显式指定枚举的底层类型（默认是 `int`），这在需要使用特定大小的枚举类型时非常有用。
>   ```cpp
>   enum class Color : char { RED, GREEN, BLUE }; // 使用 char 作为底层类型
>   ```
> 
> ### 3. 枚举的常见使用场景
> 
> - **状态机**：例如，表示一个系统的不同状态。
>   ```cpp
>   enum class State { INIT, RUNNING, STOPPED };
>   ```
> - **标识符**：例如，用于表示不同的颜色、方向或日常物体。
>   ```cpp
>   enum class Direction { NORTH, SOUTH, EAST, WEST };
>   ```
>   
> - **位标志（Flag）**：可以将枚举与位操作结合使用来表示标志集。
>   ```cpp
>   enum Flags { FLAG_A = 1, FLAG_B = 2, FLAG_C = 4 };
>   ```
> 
> ### 4. 总结
> 
> - **传统枚举**：更简单，但缺乏类型安全性，枚举量可以隐式转换为整数，容易引发命名冲突。
> - **类枚举**：引入了作用域和类型安全，防止了隐式转换，但需要使用显式作用域和类型转换，适合更复杂的场景。
> 
> 类枚举是更推荐的做法，尤其是在大项目中，这种枚举能大幅提高代码的安全性和可维护性。



> 在C++中，当你在类内声明并初始化一个`static const int`这样的静态常量（如`myConst`），如果你不在类外对其进行定义，编译器并不会为这个常量分配内存，而是将它作为一个常量表达式，直接在编译时内联到代码中使用。这意味着：
> 
> - **没有定义**（即没有类外的定义）：编译器在使用`myConst`时会直接将其值（如`10`）内联到代码中，而不会为其单独分配内存。
>   
>   例如：
> 
>   ```cpp
>   class MyClass {
>   public:
>       static const int myConst = 10;
>   };
> 
>   int main() {
>       int x = MyClass::myConst;  // 编译器会直接将10放入x
>   }
>   ```
> 
>   在这种情况下，`myConst`并没有被分配单独的内存，编译器只会在需要使用它的地方插入字面值。
> 
> - **需要定义的情况**：如果你尝试获取`myConst`的地址，例如：
> 
>   ```cpp
>   const int* ptr = &MyClass::myConst;
>   ```
> 
>   这时编译器需要为`myConst`分配内存，因为你要求获取它的地址。这种情况下，你就必须在类外提供一个定义：
> 
>   ```cpp
>   const int MyClass::myConst;  // 类外定义
>   ```
> 
>   这样，编译器会为`myConst`分配内存空间，以便你能够获取其地址。
> 
> ### 总结：
> - 如果只是使用`myConst`的值而不需要获取它的地址，那么在类内声明并初始化即可，编译器会内联其值而不分配内存。
> - 如果你需要获取`myConst`的地址，必须在类外定义它，这时它才会被分配内存。


> 这段代码不能通过C++编译器。原因在于，你在类的内部定义了一个静态常量 `num`，但在类的成员中使用这个静态常量时，编译器要求这个常量必须是一个编译时常量（`constexpr`），而且 `num` 的定义缺少分号。要修复代码，你可以做如下修改：
> 
> 1. 将 `num` 定义为 `constexpr` 或在类外定义。
> 2. 添加缺失的分号。
> 
> 修正后的代码如下：
> 
> ```cpp
> class foo {
> private:
>     static constexpr int num = 5; // Use constexpr for compile-time constant
>     int score[num];               // Use constexpr variable
> };
> ```
> 
> 或者你可以使用以下方式将 `num` 在类外定义（这适用于静态常量的非 `constexpr` 定义）：
> 
> ```cpp
> class foo {
> private:
>     static const int num = 5;   // Declare static constant
>     int score[num];             // Use static constant
> };
> 
> // Define the static constant outside the class definition
> const int foo::num;
> ```
> 
> 但通常来说，推荐使用 `constexpr` 来定义编译时常量，这样代码更简洁且更符合现代 C++ 的标准。



> 在C++中，“enum hack” 是一种用于解决 C++ 早期版本中没有 `constexpr`、`static_assert` 等功能的场景中的技巧。它通过使用 `enum` 定义来实现一种类似于常量表达式的效果，尤其是用于类成员中的常量。尽管这种技巧在现代 C++ 标准中已经不再那么必要，但在早期版本中它被广泛使用。
> 
> ### 1. `enum hack` 的基本原理：
> `enum` 类型是编译期常量，它的值可以用于许多上下文中，因此可以用于定义常量而无需使用宏。C++ 中的 `enum` 值在编译时是已知的，因此可以将其用作替代 `const` 或其他编译时常量的方式。
> 
> ### 2. 传统场景：
> 
> 在 C++98/03 标准中，如果你想在类中定义一个静态的整数常量，常用的方式是：
> 
> ```cpp
> class MyClass {
> public:
>     static const int value = 10;
> };
> ```
> 
> 但是，在某些情况下，例如用作数组的大小时，不能直接使用 `static const` 成员。因此，引入了 `enum hack`，通过 `enum` 来实现编译时常量：
> 
> ```cpp
> class MyClass {
> public:
>     enum { value = 10 };
> };
> ```
> 
> 这样做的原因是，枚举常量的值在编译时就已确定，因此可以在需要编译时常量的地方（如数组大小）使用。
> 
> ### 3. `enum hack` 使用场景：
> - **定义常量：** 在类中定义编译时整数常量，如上面例子所示。
>   
>   ```cpp
>   class MyClass {
>   public:
>       enum { MAX_SIZE = 100 };
>   };
>   
>   int arr[MyClass::MAX_SIZE];  // 使用枚举常量作为数组大小
>   ```
> 
> - **模板元编程：** 在模板编程中，可以利用 `enum hack` 进行**编译时**分支或递归调用。例如，定义递归计算阶乘：
> 
>   ```cpp
>   template <int N>
>   struct Factorial {
>       enum { value = N * Factorial<N - 1>::value };
>   };
> 
>   template <>
>   struct Factorial<0> {
>       enum { value = 1 };
>   };
> 
>   int fact5 = Factorial<5>::value;  // 计算5的阶乘
>   ```
> 
> ### 4. 为什么叫 "hack"？
> 
> 它被称为 "hack" 是因为这是一种为了解决 C++ 早期标准中的局限性而采取的特殊手段，实际上并不是一种优雅的解决方案。在现代 C++ 中（C++11及以后），我们有了 `constexpr` 和 `static_assert` 等工具，可以在编译期轻松处理常量和编译期检查。因此，这种技巧在现代 C++ 中已不再推荐使用，除非你必须编写与旧版 C++ 兼容的代码。
> 
> 例如，在 C++11 之后，可以使用 `constexpr` 更加优雅地实现：
> 
> ```cpp
> class MyClass {
> public:
>     static constexpr int value = 10;
> };
> ```
> 
> `constexpr` 是更好的选择，因为它可以用来处理任意类型的编译期常量，而不仅限于整数值。
> 
> ### 5. 结论：
> `enum hack` 是 C++98/03 中的一种技巧，用于定义编译时常量，尤其是在类成员中定义整数常量。在现代 C++ 中，它的作用已经被 `constexpr` 等特性所取代，但了解它仍然有助于理解早期 C++ 代码中的一些常见模式，以及如何解决在编译时计算中的一些问题。





> 在 C++98/03 中，`static const` 整型成员虽然是编译时常量，但在某些情况下（例如用作数组大小）不能直接使用。原因主要涉及到**编译期常量表达式**的限制，以及与 C++ 对象模型的细节有关。
> 
> ### 1. 编译期常量 vs. 链接时常量
> 
> 当你在 C++98/03 中定义一个 `static const` 成员变量时，它的声明通常是放在类的声明内部，而它的定义需要在类外提供。这导致它在某些情况下并不能被编译器立即当作编译期常量处理。
> 
> 例如：
> 
> ```cpp
> class MyClass {
> public:
>     static const int value = 10;
> };
> ```
> 
> 虽然 `value` 是 `static const`，但它属于类的静态成员，需要在类外部进行定义：
> 
> ```cpp
> const int MyClass::value;
> ```
> 
> 这个静态成员的定义在链接阶段才确定（在类文件中声明，在别的文件中定义，需要到链接阶段），并不是在编译阶段直接可用。这意味着在某些需要**编译期常量**的上下文中（如数组大小），编译器不能立即用它来替代常量表达式，因为它要等到链接时才能最终确定。
> 
> 因此，如果你尝试在数组大小中使用它：
> 
> ```cpp
> int arr[MyClass::value];  // 在 C++98/03 中，这会导致编译错误
> ```
> 
> 这时编译器会提示错误，因为数组的大小必须是一个编译时可确定的常量，而 `static const int` 可能在编译阶段并没有立即确定。
> 
> **但是在C++11之后就能通过编译了**
> 
> ### 2. 为什么 `enum hack` 可以解决这个问题？
> 
> `enum` 常量是完全编译时确定的，并且它们的值在类定义时就已经固定，不需要单独的定义或在链接时确定。因此，在数组大小等需要编译时常量的地方，`enum` 可以正常使用。
> 
> ```cpp
> class MyClass {
> public:
>     enum { value = 10 };
> };
> 
> int arr[MyClass::value];  // 这是可以的，因为枚举值在编译时已知
> ```
> 
> `enum` 的值是直接内嵌在编译期的，因此在使用时编译器可以直接将它替换为数值。
> 
> ### 3. 现代 C++ 中的改进
> 
> 在 C++11 及以后的标准中，引入了 `constexpr`，它明确表示一个变量或表达式可以在编译时求值。因此，`constexpr` 是解决这个问题的更优雅方法。
> 
> ```cpp
> class MyClass {
> public:
>     static constexpr int value = 10;
> };
> 
> int arr[MyClass::value];  // 在 C++11 中，这样是可以的
> ```
> 
> `constexpr` 保证了 `value` 是编译时常量，可以直接用于要求编译期常量的场景中，如数组大小。
> 
> ### 总结
> 
> 在 C++98/03 中，`static const` 成员变量虽然表示常量，但由于它是类的静态成员，需要在链接时定义，所以不能在所有需要编译期常量的地方使用（如数组大小）。`enum hack` 通过利用枚举的编译期常量特性，解决了这个问题。而在现代 C++ 中，引入了 `constexpr`，提供了一种更自然和简洁的方式来定义编译时常量。


> std::vector 使用**堆内存**来存储其元素，允许在运行时动态调整大小。
> 静态数组 **使用栈内存，大小在编译时确定**，内存自动分配和释放，但栈内存有限，不适合动态大小需求。



> 在 C++98/03 标准中，`static const int` 变量的值在编译时是已知的，但这并不总是足够使它成为数组大小的合法常量。以下是一些原因，解释了为什么 `static const int` 变量在某些情况下不能作为数组的大小：
> 
> ### 1. **编译期常量的要求**
> 
> C++ 标准规定，数组的大小必须是编译期常量（compile-time constant）。编译期常量是指在编译阶段就可以确定的常量。对于 `static const` 成员变量，这个要求可能无法满足，因为：
> 
> - **静态成员变量的定义**：在 C++98/03 中，`static const` 成员变量的值在类定义内部给定，**但在实际程序中**，可能需要在类外定义其值。编译器在编译数组大小时，可能不能完全确定这个 `static const` 变量的值，因为它可能需要等到链接阶段才能确定变量的实际值（例如，如果它涉及到其他文件或外部链接的内容）。
> 
> - **限制和兼容性**：C++98/03 对数组大小的要求更加严格。虽然 `static const int` 变量的值在编译时是常量，但编译器可能无法确定它是否满足所有的编译期常量要求，尤其是在涉及到链接时。如果在多个源文件中有多个定义或者某些编译器的实现对 `static const` 的处理方式不同，这可能导致数组大小的编译期常量判断失败。
> 
> ### 2. **`enum hack` 的优势**
> 
> `enum` 常量被视为**编译期常量**，因为它们的值在编译阶段就已完全确定，并且没有任何额外的定义步骤。这使得 `enum` 常量可以用作数组大小，而不会引发编译器错误：
> 
> ```cpp
> class MyClass {
> public:
>     enum { value = 10 };
> };
> 
> int arr[MyClass::value];  // 使用 `enum` 常量作为数组大小
> ```
> 
> `enum` 常量的值直接内嵌在编译期，因此它可以被直接用作数组大小等需要编译期常量的上下文。
> 
> ### 3. **现代 C++ 中的改进**
> 
> 在 C++11 及以后版本中，`constexpr` 关键字提供了一种更现代的方式来定义编译期常量。`constexpr` 变量可以直接用作数组大小，并且没有 `static const` 的额外限制：
> 
> ```cpp
> class MyClass {
> public:
>     static constexpr int value = 10;
> };
> 
> int arr[MyClass::value];  // 使用 `constexpr` 常量作为数组大小
> ```
> 
> `constexpr` 变量的值在编译时完全确定，这避免了 C++98/03 中 `static const` 变量可能遇到的限制。
> 
> ### 总结
> 
> 在 C++98/03 中，`static const int` 变量的值在编译时虽然是已知的，但由于编译器对编译期常量的严格要求和静态成员的定义限制，可能无法保证其作为数组大小的合法性。而 `enum` 常量和现代 C++ 中的 `constexpr` 提供了更可靠的解决方案，用于满足编译期常量的要求。




```cpp
class foo{
private:
    enum {NumTurns=5};
    int scores[NumTurns];
}
```

Another common misuse of the #define directive is using it to implement macro that look like functions.
```cpp
#define CALL_WITH_MAX(a,b) f((a)>(b)?(a):(b))
```

whenever you write this kind of macro, you have to remember to parenthesize all the arguments (call macro with an expression).

```cpp
int a=5,b=0;
CALL_WITH_MAX(++a,b); //finally, a is 7
CALL_WITH_MAX(++a,b+10) //finally, a is 6
```

you should use a template for an inline function

```cpp
template<typename T>
inline void callWithMax(const T& a,const T&b){
    f(a>b?a:b);
}

```

## Item3: Use const whenever possible

```cpp
char greeting[]="Hello";
char *p=greeting;       // non-const pointer, non-const data

const char *p=greeting; //const data, non-const pointer

char * const p=greeting; //const pointer, non-const data

const char * const p=greeting; //const pointer, const data

```

the difference is the side of const relative to *, there are same parameter type:

```cpp
void f1(const foo *pf);
void f2(foo const *pf);

```
STL iterator acts like T* pointer

```cpp
const vector<int>::iterator iter1; // T* const, const pointer, non-const data
*iter1=10 // ok
iter1++  //error
```

```cpp
vecto<int>::const_iterator iter2; //const T*, const data, non-const pointer
*iter2=10 //error
iter2++ // ok

```

Having a function return a constant value often makes it  possible to reduce the incidence of client error, such as:
```cpp
class Rational{
    //...
};
const Rational operator*(const Rational& lhs,const Rational& rhs);
Rational a,v,c;
if (a*b=c) //error
```
> 注意const对象不能赋值给非const对象，用非const对象接收返回值的话会报错

The purpose of `const` on `member function` is to identify which member function may be invoked on `const` objects.

```cpp
class foo{
public:
    const char& operator[](size_t position) const {return text[position];}
    char& operator[](size_t position){return text[position];}
}

private:
    string text;


foo tb("Hello");
cout << tb[0]; //non-const operator[]

const foo ctb("Hello");
cout << tb[0] //const operator[]
```

![const](3-const.png)


> 当 `const` 和非 `const` 的成员函数同时存在时，非 `const` 对象仍然可以调用 `const` 成员函数。在 C++ 中，编译器会根据对象的 `const` 性质和成员函数的 `const` 性质来决定调用哪个成员函数。如果一个非 `const` 对象调用成员函数并且该函数有一个 `const` 和一个非 `const` 版本，那么编译器会**优先**选择非 `const` 版本。如果只有 `const` 版本存在，那么编译器会选择调用 `const` 版本。
> 
> 举个例子：
> 
> ```cpp
> class MyClass {
> public:
>     void func() {
>         // 非 const 版本
>         std::cout << "Non-const function called" << std::endl;
>     }
> 
>     void func() const {
>         // const 版本
>         std::cout << "Const function called" << std::endl;
>     }
> };
> 
> int main() {
>     MyClass obj;
>     obj.func(); // 调用非 const 版本
>     return 0;
> }
> ```
> 
> 在这个例子中，`obj.func()` 会调用非 `const` 版本的 `func()` 函数，因为 `obj` 是一个非 `const` 对象。
> 
> 如果你想明确调用 `const` 版本的成员函数，可以将对象转为 `const` 对象：
> 
> ```cpp
> const MyClass& constObj = obj;
> constObj.func(); // 调用 const 版本
> ```
> 
> 通过将 `obj` 转换为 `const` 引用，我们可以明确调用 `const` 版本的 `func()` 函数。

**Member function differing only in their constness can be overloaded**

const member function **might modify some of the bits in the object**, even though compilers enforce bitwise constness.

```cpp
#include <bits/stdc++.h>
using namespace::std;

class foo{
public:
    foo(char* c):ptext(c){};
    char& operator[](size_t position) const {return ptext[position];}

private:
    char* ptext;
};

int main(){
    char* p="Hello";
    
    const foo cctb(p);
    char* pc=&cctb[0]; //call the const operator[]
    *pc='J'; //modify data with pointer
    
}

```
> 即使函数本身是const，仍然有可能修改成员变量，但返回值是const的话永远不可能修改返回值


`mutable` free data member from constraints of bitwise constness

```cpp
#include <iostream>

class Logger {
public:
    Logger() : logCount(0) {}

    void log(const std::string& message) const {
        ++logCount; // logCount是mutable的，所以可以在const成员函数中修改
        std::cout << "Log[" << logCount << "]: " << message << std::endl;
    }

private:
    mutable int logCount;
};

int main() {
    Logger logger;
    logger.log("First message");
    logger.log("Second message");
    return 0;
}


```
To avoid duplication in `const` and `non-const` member functions, you can use casting

```cpp
class foo{
private:
    string text;
public:
    const char& operator[](size_t position) const {return text[position];}
    char& operator[](size_t position){
        return const_cast<char&>(
            static_cast<const foo&>(*this)[position];
        )
    }
};

```
> `static_cast` 是 C++ 中用于进行类型转换的运算符。它提供了一种安全的类型转换方式，可以在编译时进行类型检查。其主要作用包括：
> 
> 1. **基本类型转换**：可以将一个基本数据类型转换为另一个基本数据类型，例如 `int` 转换为 `float`。
>    
>    ```cpp
>    int i = 10;
>    float f = static_cast<float>(i);  // int 转 float
>    ```
> 
> 2. **指针和引用类型转换**：可以将一个指针或引用转换为不同类型的指针或引用。这主要用于类层次结构中的类型转换，如基类指针转换为派生类指针，但只限于在有明确关系的情况下使用。
> 
>    ```cpp
>    class Base {};
>    class Derived : public Base {};
> 
>    Base* basePtr = new Derived();
>    Derived* derivedPtr = static_cast<Derived*>(basePtr);  // Base* 转 Derived*
>    ```
> 
> 3. **避免不安全的转换**：`static_cast` 是比 C 风格的强制转换（如 `(Type)value`）更安全的选择，因为它会进行类型检查，如果类型转换是不安全的，编译器会报错。
> 
> 4. **明确转换意图**：使用 `static_cast` 可以清晰地表明你的转换意图，增强代码的可读性和可维护性。
> 
> 需要注意的是，`static_cast` 不能用于在没有明确继承关系的类之间进行转换，也不能处理复杂的类型转换（如 `dynamic_cast`、`reinterpret_cast` 和 `const_cast`），这些操作需要使用其他类型转换运算符。
> 
> 
> `const_cast` 是 C++ 中用于修改对象的常量性（constness）或去除对象的常量性。它允许你在不改变对象的实际数据类型的情况下，改变对象是否为常量（`const`）或非常量（`non-const`）。主要用途如下：
> 
> 1. **去除常量性**：如果你有一个 `const` 对象指针或引用，而你确信它实际上不是 `const` 的（通常是因为它指向的对象是非常量的，但通过某些方式被声明为 `const`），可以使用 `const_cast` 去除 `const` 限定符。
> 
>    ```cpp
>    void modifyValue(int* ptr) {
>        *ptr = 10;  // 修改值
>    }
> 
>    int main() {
>        const int x = 5;
>        modifyValue(const_cast<int*>(&x));  // 去除 const 限定符，虽然实际操作可能导致未定义行为
>        return 0;
>    }
>    ```
> 
>    **警告**：去除 `const` 限定符并修改对象是危险的，因为如果原始对象是 `const` 的，修改它会导致未定义行为。
> 
> 2. **添加常量性**：可以使用 `const_cast` 将一个非常量对象转换为 `const` 对象，这在某些接口要求 `const` 参数时很有用。
> 
>    ```cpp
>    void process(const int& value) {
>        // 只读操作
>    }
> 
>    int main() {
>        int x = 10;
>        process(const_cast<const int&>(x));  // 将非常量引用转换为 const 引用
>        return 0;
>    }
>    ```
> 
>    这种用法通常不会改变对象本身的常量性，只是确保接口在处理过程中不会修改对象。
> 
> 
> - **不应滥用 `const_cast`**：在使用 `const_cast` 去除 `const` 时，必须确保该对象实际上不是 `const` 的，否则会导致未定义行为。你应当谨慎使用它，并理解它的潜在风险。
>   
> - **安全使用**：`const_cast` 主要用于需要兼容旧代码或处理 API 接口的场景。对于大多数场景，避免不必要地更改对象的常量性是最佳实践。
> 
> ## Item4: Make sure that objects are initialized before they're used
> 
> The rules of  C++ stipulate that data member of an object are **initialized before the body of a ctor** is entered. The best way to write ctor is to use member initialization list instead of assignment.

```cpp
class MyClass {
private:
    int a;
    string b;
    // build-in type like a has no difference in cost in two situation, undefined situation

public:
    // member initialization list is more effective
    //call b's copy ctor to initialization
    MyClass(int x, string y) : a(x), b(y) {}

    // assignment
    //call b's default ctor, then assignment
    MyClass(int x, string y) {
        a = x;
        b = y;
    }
};

```
Sometimes the initialization must be used, such as `const` and `reference`

```cpp
class MyClass {
private:
    const int a;
    int &b;
public:
    
    MyClass(int x, int &y) : a(x), b(y) {}

    // MyClass(int x, int &y) {
    //     a = x; // 编译错误，const 成员不能赋值
    //     b = y; // 编译错误，引用成员不能赋值
    // }
};


```

The order of member initialization depends on its **declaration order**, not order on initialization list.

translation unit is important

> 在C++中，翻译单元（translation unit）是指编译器处理的基本单位。具体来说，翻译单元是由一个源文件及其包含的头文件（通过#include指令）组合而成的。编译器会将这个组合视为一个单独的实体来进行编译。假设有以下文件结构：
```cpp
// file1.cpp
#include "common.h"
void function1() {
    // Some code
}

// file2.cpp
#include "common.h"
void function2() {
    // Some code
}

// common.h
#ifndef COMMON_H
#define COMMON_H

void sharedFunction();

#endif
```
> 编译时会生成两个翻译单元：
> - file1.cpp和common.h的组合。
> - file2.cpp和common.h的组合。
> 每个翻译单元分别编译生成目标文件，然后链接成最终的可执行文件或库。

The order of initialization of non-local static object defined in different translation units may cause trouble, because the relative order of initialization of non-local static objects defined in different translation units is undefined.

![static解释](static.png)




> 静态对象包括在全局对象，在命名空间中定义的对象，类中声明为static的对象，函数中声明为static的对象，某个文件中声明为static的对象。局部static对象特指函数内定义的static变量。其他的静态对象都是 non-local object，本节要声明的就是要把所有静态对象都尽量做成局部静态对象 local static，即静态对象都放在函数内部，c++保证在第一次调用函数的时候会把它内部的静态对象都初始化
> 
> 在C++中，非局部(static)对象包括两类主要类型的对象：
> 
> 1. **全局静态对象 (global static objects)**：这些对象在全局作用域中声明，并使用`static`关键字修饰。全局静态对象在程序开始时初始化，并在程序结束时销毁。它们的生命周期覆盖整个程序的运行过程，但作用域仅限于定义它们的文件。
> 
>     ```cpp
>     // 文件 file1.cpp
>     static int globalStaticVar = 10; // 全局静态变量
>     ```
> 
> 2. **命名空间内的静态对象 (namespace static objects)**：这些对象在命名空间作用域中声明，并使用`static`关键字修饰。它们的生命周期与全局静态对象类似，但其作用域仅限于定义它们的命名空间。
> 
>     ```cpp
>     namespace MyNamespace {
>         static int namespaceStaticVar = 20; // 命名空间内的静态变量
>     }
>     ```
> 
> 需要注意的是，C++中的**局部静态对象** (local static objects) 是在**函数或代码块**中声明并使用`static`关键字修饰的对象。这些对象在第一次使用时初始化，并在程序结束时销毁。它们的生命周期覆盖整个程序的运行过程，但作用域仅限于定义它们的函数或代码块。这类对象不属于非局部静态对象。
> 
> ```cpp
> void myFunction() {
>     static int localStaticVar = 30; // 局部静态变量
> }
> ```
> 
> 总结起来，非局部静态对象包括全局静态对象和命名空间内的静态对象，它们在程序的整个运行期间存在，但作用域限制在其声明的文件或命名空间中。

Client use functions return reference to objects instead of using the objects themselves, then every objects can be initialize correctly. **In other words, non-local static object are replaced with local static object.**

> 在C++中，局部静态对象（local static object）是在其所属的函数或代码块首次执行到该对象的声明时进行初始化的。这意味着它们的初始化只会发生一次，无论该函数或代码块被调用多少次。
> 
> 具体来说：
> 
> - 局部静态对象的初始化发生在程序控制首次到达其声明的地方，也就是第一次调用函数的时候。
> - 如果函数从未被调用，那么局部静态对象也不会被初始化。
> - 一旦局部静态对象被初始化，它在程序的整个生命周期内保持存在，即使该函数多次调用，局部静态对象也不会再次被初始化。
> 
> - 例子
> 
> ```cpp
> #include <iostream>
> 
> void foo() {
>     static int x = 0; // 局部静态对象x
>     x++;
>     std::cout << "x = " << x << std::endl;
> }
> 
> int main() {
>     foo(); // 第一次调用，x被初始化为0，然后加1，输出x = 1
>     foo(); // 第二次调用，x不会重新初始化，只会加1，输出x = 2
>     foo(); // 第三次调用，x再次加1，输出x = 3
>     return 0;
> }
> ```
> 
> 在上述代码中，`static int x = 0;`仅在第一次调用`foo`时初始化。后续调用时，`x`的值将基于上一次调用的结果递增。
> 
> - 多线程环境下的初始化
> 
> 在C++11及以后，局部静态对象的初始化是线程安全的。这意味着如果多个线程同时首次到达该对象的声明，只有一个线程会执行初始化代码，其它线程会等待初始化完成后继续执行。

```cpp

//file1
class foo1{...};

foo1& getFoo1(){
    static foo1 f1;
    return f1;
}

//file2
class foo2{...};
foo2::foo2(params){
    something=getFoo1().method();
}

foo2& getFoo2(){
    static foo2 f2(params);
    return f2;
}


```

# 2.Constructors, Destructors, and Assignment Operators

## Item5: Know what functions C++ silently writes and calls

Compilers may implicitly generate a class's default ctor, copy ctor, copy op=, and destructor.

## Item6: Explicitly disallow the use of compiler generated function you do not want

By declaring a member function explicitly and not implementing them, you prevent compilers from generating thier own version,, and by making the function `private`, you keep people from calling it. It is used to prevent copying in several classes in C++'s iostream library.

> 在C++中，如果你试图调用一个没有定义的方法，会在连接（linking）时产生错误。这是因为在编译阶段，编译器只会检查方法的声明是否正确，并不会检查方法是否有定义。而连接器在链接阶段则会试图找到所有方法的定义，如果找不到相应的方法定义，就会导致链接错误。
> 例如，假设你有以下代码：
> ```cpp
> // MyClass.h
> class MyClass {
> public:
>     void myMethod();
>};
>
> // MyClass.cpp
> #include "MyClass.h"
> 
> // 注意：这里没有给出myMethod的定义
> 
> // main.cpp
> #include "MyClass.h"
> 
> int main() {
>    MyClass obj;
>    obj.myMethod(); // 尝试调用myMethod
>    return 0;
> }
> ```

To move link-time error up to compile time by declare copy ctor and op= in base class.

```cpp
class Uncopyable {
protected:
    // 构造函数和析构函数可以是protected的，以允许派生类实例化
    Uncopyable() {}
    ~Uncopyable() {}

private:
    // 拷贝构造函数和拷贝赋值运算符声明为private，禁止拷贝
    Uncopyable(const Uncopyable&); // 不需要参数名
    Uncopyable& operator=(const Uncopyable&);
};

class Derived : public Uncopyable {
public:
    Derived() {}
    // 其他成员函数和数据
};

int main() {
    Derived obj1;
    Derived obj2 = obj1; // 错误：拷贝构造函数不可用
    Derived obj3;
    obj3 = obj1; // 错误：拷贝赋值运算符不可用
    return 0;
}

```
> 如果你使用C++11及其之后的标准，可以更方便地禁用这些函数，通过将它们声明为删除的（delete）

> 如果父类的无参数构造函数被声明为 private，而子类没有显式实现构造函数，那么在尝试创建子类对象时会导致**编译错误**。这是因为编译器生成的子类默认构造函数将尝试调用基类的默认构造函数，但由于基类的默认构造函数是私有的，子类无法访问它。

> 这里是一个具体的例子来说明这种情况：
> class Base {
> private:
>     Base() {
>         // 基类无参数构造函数为 private
>     }
> 
> public:
>     Base(int x) {
>         // 基类带参数的构造函数
>     }
> };
> 
> class Derived : public Base {
>     // 没有定义构造函数
> };
> 
> int main() {
>     Derived d;  // 编译错误
>     return 0;
> }


## Item7: Declare destructors virtual in polymorphic base classes

> 非虚函数的调用是在编译时决定的，而不是在运行时。对于非虚的析构函数，编译器在编译时已经决定了通过父类指针调用父类的析构函数，而不是在运行时根据指针指向的对象类型来决定调用哪个析构函数。
> 
> 具体来说，当你使用一个指向基类的指针删除一个派生类对象时，如果基类的析构函数不是虚函数，编译器会将该删除操作解析为调用基类的析构函数，而不会涉及派生类的析构函数。这是因为编译器无法在编译时确定指针实际上指向的是哪个类的对象。
> 
> 下面是更详细的解释：
> 
> ```cpp
> #include <iostream>
> 
> class Base {
> public:
>     ~Base() {
>         std::cout << "Base destructor" << std::endl;
>     }
> };
> 
> class Derived : public Base {
> public:
>     ~Derived() {
>         std::cout << "Derived destructor" << std::endl;
>     }
> };
> 
> int main() {
>     Base* ptr = new Derived();
>     delete ptr; // 这是未定义行为
>     return 0;
> }
> ```
> 
> 在上述代码中，编译器看到的是一个指向`Base`类的指针`ptr`。当执行`delete ptr`时，编译器调用的是`Base`类的析构函数，因为`Base`的析构函数不是虚函数。由于这个调用是在编译时决定的，编译器不会在运行时检查`ptr`实际指向的是`Derived`类的对象。因此，只调用了`Base`的析构函数，而`Derived`的析构函数则被忽略了。
> 
> 为了让析构函数具有多态性，使得在删除通过基类指针指向的派生类对象时能够正确调用派生类的析构函数，必须将基类的析构函数声明为虚函数。这样编译器在生成代码时会插入必要的运行时检查，以确保在运行时根据对象的实际类型来调用正确的析构函数：
> 
> ```cpp
> #include <iostream>
> 
> class Base {
> public:
>     virtual ~Base() {
>         std::cout << "Base destructor" << std::endl;
>     }
> };
> 
> class Derived : public Base {
> public:
>     ~Derived() {
>         std::cout << "Derived destructor" << std::endl;
>     }
> };
> 
> int main() {
>     Base* ptr = new Derived();
>     delete ptr; // 现在是定义行为，会先调用Derived析构函数，再调用Base析构函数
>     return 0;
> }
> ```
> 
> 在这个代码中，由于`Base`的析构函数被声明为虚函数，当通过基类指针删除派生类对象时，运行时系统会检查实际的对象类型，并首先调用`Derived`类的析构函数，然后再调用`Base`类的析构函数。输出结果将是：
> 
> ```
> Derived destructor
> Base destructor
> ```
> 
> 这样确保了所有对象的析构函数都能被正确调用，避免资源泄漏和其他未定义行为。

![virtual function](7vf.png)

> 在C++中，虚函数表（Virtual Function Table，VTable）和虚指针（Virtual Pointer，VPTR）是用来实现多态性（Polymorphism）的关键概念。
> ### 虚函数表（VTable）
> 虚函数表是用来实现动态多态性的一种机制。在包含虚函数的类中，每个对象都有一个指向虚函数表的指针。这个表存储了类的虚函数的地址。当调用一个虚函数时，实际执行的函数是根据对象指向的虚函数表中的地址来确定的，而不是根据对象的类型或者指针的类型。
> 具体来说：
> - 每个类（含有虚函数的类）有一个对应的虚函数表。
> - 虚函数表是一个数组，每个元素是一个指向虚函数的指针。
> - 对象的内存布局中，通常会有一个虚指针，指向该对象所属类的虚函数表。
> - 当通过基类的指针或引用调用虚函数时，实际执行的是派生类中的对应虚函数，这是通过虚函数表和虚指针实现的。
> ### 虚指针（VPTR）
> 虚指针是一个指向虚函数表的指针，它位于每个对象的内存布局中，用来指示该对象的实际类型。具体来说
> - 对于含有虚函数的类的每个对象，都会包含一个虚指针。
> - 虚指针指向该对象所属类的虚函数表的起始地址。
> - 通过虚指针，可以在运行时确定调用哪个类的虚函数表，从而实现多态性。
>综上所述，虚函数表和虚指针是C++实现多态性的关键机制。它们使得在运行时能够动态地确定调用哪个类的虚函数，从而实现了面向对象编程中的多态性特性。

Declare a virtual destructor in a class if and only if that class contains at least one virtual function. Do not add useless memory usage of vptr.

**If a class has any virtual functions, it should have a virtual destructor.**

## Item8: Prevent exceptions from leaving destructor

> 在C++的析构函数中抛出异常是不被鼓励的。这是因为在C++中，如果一个析构函数在堆栈展开期间抛出异常，会导致程序调用`std::terminate`，从而终止程序。堆栈展开期间通常发生在另一个异常被抛出时，因此如果析构函数在这种情况下再抛出一个异常，就会有两个同时存在的未处理异常，C++标准规定在这种情况下必须终止程序。
> 
> 这是一个简化的示例：
> 
> ```cpp
> #include <iostream>
> 
> class Example {
> public:
>     ~Example() {
>         throw std::runtime_error("Destructor exception");
>     }
> };
> 
> int main() {
>     try {
>         Example e;
>         throw std::runtime_error("Main exception");
>     } catch (const std::exception& e) {
>         std::cout << "Caught exception: " << e.what() << std::endl;
>     }
>     return 0;
> }
> ```
> 
> 在这个例子中，析构函数中抛出的异常与`main`函数中抛出的异常同时存在，程序会调用`std::terminate`，**导致程序崩溃而不是捕获并处理异常**。
> 
> 为了避免这种情况，应该在析构函数中处理任何可能抛出的异常，或者在析构函数中不抛出异常，以保证异常永远不会离开析构函数。以下是一个处理异常的示例：
> 
> ```cpp
> #include <iostream>
> 
> class Example {
> public:
>     ~Example() {
>         try {
>             // Code that might throw an exception
>         } catch (...) {
>             // Handle exception or log it
>         }
>     }
> };
> 
> int main() {
>     try {
>         Example e;
>         throw std::runtime_error("Main exception");
>     } catch (const std::exception& e) {
>         std::cout << "Caught exception: " << e.what() << std::endl;
>     }
>     return 0;
> }
> ```
> 
> 在这个示例中，析构函数捕获并处理了任何可能抛出的异常，确保不会在堆栈展开期间抛出异常，从而避免程序终止。

> 堆栈展开（Stack Unwinding）是指在C++程序中，当一个异常被抛出并传播时，为了找到合适的异常处理器（catch块），程序会按调用栈的顺序依次退出函数，并在此过程中调用已经构造的对象的析构函数。这一过程确保了资源的正确释放和对象的正确销毁。
> 
> 堆栈展开的过程如下：
> 
> 1. **异常抛出**：当一个异常被抛出时，程序会开始寻找处理该异常的catch块。
> 
> 2. **寻找catch块**：程序从当前抛出异常的位置开始，沿着调用栈向上查找，直到找到合适的catch块来处理该异常。
> 
> 3. **调用析构函数**：在寻找catch块的过程中，程序会依次退出各个函数，并为在这些函数中构造的对象调用析构函数，以释放资源和进行清理。
> 
> 4. **处理异常**：一旦找到合适的catch块，程序就会跳转到该catch块，执行其中的代码来处理异常。
> 
> 如果在堆栈展开的过程中，一个析构函数抛出异常，而此时已经有一个未处理的异常存在，则会导致程序中出现两个未处理的异常。C++标准规定，在这种情况下，程序会调用`std::terminate`，从而终止程序。
> 
> 下面是一个示例代码来说明堆栈展开：
> 
> ```cpp
> #include <iostream>
> #include <stdexcept>
> 
> class Example {
> public:
>     Example() {
>         std::cout << "Example constructed\n";
>     }
>     ~Example() {
>         std::cout << "Example destructed\n";
>     }
> };
> 
> void funcB() {
>     Example e;
>     std::cout << "In funcB\n";
>     throw std::runtime_error("Exception in funcB");
> }
> 
> void funcA() {
>     Example e;
>     std::cout << "In funcA\n";
>     funcB();
> }
> 
> int main() {
>     try {
>         funcA();
>     } catch (const std::exception& e) {
>         std::cout << "Caught exception: " << e.what() << std::endl;
>     }
>     return 0;
> }
> ```
> 
> 输出：
> 
> ```
> Example constructed
> In funcA
> Example constructed
> In funcB
> Example destructed
> Example destructed
> Caught exception: Exception in funcB
> ```
> 
> 在这个示例中，当`funcB`中抛出异常时，堆栈展开过程开始。首先，`funcB`中的局部对象`e`的析构函数被调用，然后程序退出`funcB`，接着`funcA`中的局部对象`e`的析构函数被调用，最后异常被`main`中的catch块捕获并处理。这展示了堆栈展开过程中如何调用析构函数来清理资源。
```cpp
class DBConn{
  private:
      DBConnection db; 
      bool closed;
  public:
      //db.close() may throw some exceptions
      void close(){
          db.close();
          closed=true;
      }   
      ~DBConn(){
          if (!closed){
              try{ //must handle to prevent from terminating
                  db.close();
              }   
              catch(//...){
                  //...
              }
          }
      }   
  
  }
```

Moving the responsibility for calling `close` from DBConn's destructor to DBConn's client is a better strategy. **If an operation may fail by throwing an exception and there may be a need to handle that exception, the exception has to come from some non-destructor function.**

## Item9: Never call virtual functions during construction or destruction

> 在 C++ 中，当父类的构造函数调用虚函数时，调用的实际上是父类自身的虚函数实现，而不是子类的实现。这是因为在父类构造函数执行期间，子类部分还没有被初始化，因此虚函数表（vtable）还没有被更新到子类的版本。 
> 具体来说，在 C++ 中，对象的构造是从基类到派生类逐步进行的。父类的构造函数先于子类的构造函数执行，在父类构造函数执行期间，**对象实际上还是父类类型的**（运行时类型都会认为是父类，比如影响 dynamic_cast 和 typeid 等），因此调用的虚函数是父类的版本。对于析构函数也有同样的道理，开始析构的时候，运行时类型先是子类，之后类型会变成父类，也会影响到调用的虚函数版本
> 
> ```cpp
> #include <iostream>
> 
> class Base {
> public:
>     Base() {
>         // 这里调用的是Base类中的foo，而不是Derived类中的foo
>         foo();
>     }
> 
>     virtual void foo() {
>         std::cout << "Base::foo()" << std::endl;
>     }
> };
> 
> class Derived : public Base {
> public:
>     Derived() : Base() {
>         // 这里调用的是Derived类中的foo
>         foo();
>     }
> 
>     void foo() override {
>         std::cout << "Derived::foo()" << std::endl;
>     }
> };
> 
> int main() {
>     Derived d;
>     return 0;
> }
> ```
> 
> 输出结果为：
> ```
> Base::foo()
> Derived::foo()
> ```
> 
> 从输出结果可以看出，在父类构造函数中调用的是父类的 `foo()` 方法，而在子类构造函数中调用的是子类的 `foo()` 方法。

Don't call virtual functions during construction or destruction, because such calls will never go to a more derived class than that of the currently executing ctor or dtor.

## item 10: Have assignment operators return a reference to *this

> 在C++中，赋值操作通常返回左参数的引用。这是因为返回左参数的引用允许对赋值操作进行链式调用（chaining）。例如，你可以这样写：
> 
> ```cpp
> int a, b, c;
> a = b = c = 5;
> ```
> 
> 在这个例子中，赋值操作会从右到左进行，首先将5赋值给`c`，然后将`c`的值赋值给`b`，最后将`b`的值赋值给`a`。这是通过赋值操作返回左参数的引用实现的。
> 
> 这是一个简单的类示例，说明赋值操作符如何返回左参数的引用：
> 
> ```cpp
> #include <iostream>
> 
> class MyClass {
> public:
>     MyClass& operator=(const MyClass& other) {
>         if (this != &other) {
>             // 执行赋值操作
>             // 例如：复制成员变量
>         }
>         return *this; // 返回当前对象的引用
>     }
> };
> 
> int main() {
>     MyClass obj1, obj2, obj3;
>     obj1 = obj2 = obj3; // 链式赋值操作
>     return 0;
> }
> ```
> 
> 在这个示例中，`operator=` 函数返回当前对象（`*this`）的引用（`&`） ，这允许链式赋值操作。

This convention applies to all assignment op, such as operator+=


## Item 11: Handle assignment to self in op=


> 在C++中，aliasing（别名或别名化）指的是多个变量或引用指向同一个内存位置的现象。这意味着通过这些变量或引用可以访问和修改相同的内存内容。aliasing 可能会对程序的行为和性能产生影响，因为编译器在优化代码时需要考虑这种情况。
> 
> 以下是一个简单的例子，说明 aliasing 的概念：
> 
> ```cpp
> #include <iostream>
> 
> void modify(int &a, int &b) {
>     a = 10;
>     b = 20;
> }
> 
> int main() {
>     int x = 5;
>     int &y = x;  // y 是 x 的引用，即 y 和 x 指向同一个内存位置
> 
>     modify(x, y); // 调用 modify 函数时，a 和 b 都是对 x 的引用
> 
>     std::cout << "x = " << x << std::endl;  // 输出 20
>     std::cout << "y = " << y << std::endl;  // 输出 20
> 
>     return 0;
> }
> ```
> 
> 在这个例子中，`x` 和 `y` 指向同一个内存位置，因此它们是 aliasing 的。当 `modify` 函数改变 `a` 和 `b` 的值时，实际上是改变了同一个变量 `x` 的值。
> 
> aliasing 可能会对编译器优化产生影响。为了让编译器更好地优化代码，C++ 引入了 `restrict` 关键字（在 C++11 中引入，但在 C++ 标准库中并不直接使用，可以通过编译器特定的扩展如 `__restrict__` 来使用），这可以告诉编译器某个指针不会 alias 其他指针。例如：
> 
> ```cpp
> void modify(int * __restrict__ a, int * __restrict__ b) {
>     *a = 10;
>     *b = 20;
> }
> ```
> 
> 在这个函数中，`__restrict__` 关键字告诉编译器，指针 `a` 和 `b` 不会指向同一块内存区域，这样编译器可以进行更激进的优化。
> 
> 理解 aliasing 对编写高效代码非常重要，特别是在涉及指针和引用的复杂程序中。
> 
> 
> 在C++中，拷贝赋值操作符（即拷贝赋值函数）可能会遇到自我赋值的问题，这会导致程序出现不安全的行为或者错误。自我赋值发生在对象给自己赋值时，如`a = a`。为了避免这种情况，我们需要在实现拷贝赋值操作符时进行自我赋值检查。以下是一个示例说明如何正确实现拷贝赋值操作符并处理自我赋值的问题：
> 
> ```cpp
> #include <iostream>
> 
> class MyClass {
> public:
>     int* data;
> 
>     // 构造函数
>     MyClass(int value) {
>         data = new int(value);
>     }
> 
>     // 拷贝构造函数
>     MyClass(const MyClass& other) {
>         data = new int(*other.data);
>     }
> 
>     // 拷贝赋值操作符
>     MyClass& operator=(const MyClass& other) {
>         if (this == &other) { //添加额外的代码使得占用内存更大，而且引入了分支流，代码运行速度也会下降
>             // 如果是自我赋值，直接返回当前对象
>             return *this;
>         }
>       //上面如果不检查的话这里可能会先删除自己内部的东西，然后又引用已经被删除的内容 
>         // 释放旧的资源
>         delete data;
> 
>         // 分配新的资源并拷贝数据
>         data = new int(*other.data);
> 
>         // 返回当前对象的引用
>         return *this;
>     }
> 
>     // 析构函数
>     ~MyClass() {
>         delete data;
>     }
> 
>     // 打印数据
>     void printData() const {
>         std::cout << *data << std::endl;
>     }
> };
> 
> int main() {
>     MyClass obj1(42);
>     MyClass obj2(100);
> 
>     obj2 = obj1; // 调用拷贝赋值操作符
>     obj2.printData(); // 输出 42
> 
>     obj2 = obj2; // 自我赋值
>     obj2.printData(); // 仍然输出 42
> 
>     return 0;
> }
> ```
> 
> 在这个示例中，我们在拷贝赋值操作符中首先检查是否是自我赋值（`if (this == &other)`）。如果是自我赋值，我们直接返回当前对象的引用（`return *this`），这样可以避免不必要的操作和潜在的问题。如果不是自我赋值，我们会先释放当前对象的数据（`delete data`），然后分配新的内存并复制数据（`data = new int(*other.data)`）。最后，返回当前对象的引用。
> 
> 这种方法可以确保拷贝赋值操作符在处理自我赋值时是安全的，并且不会导致内存泄漏或未定义行为。
> 
>  **但是上面的函数仍然存在拷贝赋值的异常不安全**
> 
> 在C++中，拷贝赋值操作符如果抛出异常，可能会导致部分资源被释放，进而引发资源泄露或程序崩溃的问题。下面是一个简单的例子，展示了拷贝赋值操作符抛出异常时可能导致部分资源被删除的情况。
> 
> 假设我们有一个类 `ResourceHolder`，它管理两个资源。为了简化示例，这里使用指向整数的指针作为资源。
> 
> ```cpp
> #include <iostream>
> #include <stdexcept>
> 
> class ResourceHolder {
> public:
>     int* resource1;
>     int* resource2;
> 
>     // 构造函数
>     ResourceHolder(int val1, int val2) {
>         resource1 = new int(val1);
>         resource2 = new int(val2);
>     }
> 
>     // 拷贝构造函数
>     ResourceHolder(const ResourceHolder& other) {
>         resource1 = new int(*other.resource1);
>         resource2 = new int(*other.resource2);
>     }
> 
>     // 拷贝赋值操作符
>     ResourceHolder& operator=(const ResourceHolder& other) {
>         if (this == &other) {
>             return *this;
>         }
> 
>         // 释放当前资源
>         delete resource1;
>         delete resource2;
>       // 先delete后new可能会出问题 
>         // 分配新资源
>         resource1 = new int(*other.resource1);
> 
>         // 在这里故意抛出一个异常，模拟可能出现的错误情况
>         if (other.resource2 == nullptr) {
>             throw std::runtime_error("Error while copying resource2");
>         }
> 
>         resource2 = new int(*other.resource2);
> 
>         return *this;
>     }
> 
>     // 析构函数
>     ~ResourceHolder() {
>         delete resource1;
>         delete resource2;
>     }
> };
> 
> int main() {
>     try {
>         ResourceHolder holder1(10, 20);
>         ResourceHolder holder2(30, 40);
> 
>         // 这里进行拷贝赋值操作
>         holder2 = holder1;
>     } catch (const std::exception& e) {
>         std::cerr << "Exception caught: " << e.what() << std::endl;
>     }
> 
>     return 0;
> }
> ```
> 
> 在这个例子中，`ResourceHolder` 类管理两个整数资源 `resource1` 和 `resource2`。在拷贝赋值操作符中，当我们分配 `resource1` 之后，在分配 `resource2` 之前故意抛出了一个异常。这会导致 `resource1` 已经被分配了新值，而 `resource2` 还没有分配新值。此时，原本持有的 `resource2` 的资源已经被删除，造成了资源泄露或潜在的程序崩溃问题。
> 
> 为了解决这个问题，可以交换一下声明顺序，先new，后delete。防止delete之后new抛出异常
> 
> ```cpp
> #include <iostream>
> #include <stdexcept>
> #include <algorithm> // std::swap
> 
> class ResourceHolder {
> public:
>     int* resource1;
>     int* resource2;
> 
>     // 构造函数
>     ResourceHolder(int val1, int val2) {
>         resource1 = new int(val1);
>         resource2 = new int(val2);
>     }
> 
>     // 拷贝构造函数
>     ResourceHolder(const ResourceHolder& other) {
>         resource1 = new int(*other.resource1);
>         resource2 = new int(*other.resource2);
>     }
> 
>     // 移动构造函数
>     ResourceHolder(ResourceHolder&& other) noexcept {
>         resource1 = other.resource1;
>         resource2 = other.resource2;
>         other.resource1 = nullptr;
>         other.resource2 = nullptr;
>     }
> 
>     // 拷贝赋值操作符，使用拷贝并赋值
>     ResourceHolder& operator=(const ResourceHolder& other) {
>         // 把原本的内容先保存一份
>         int* ro1=resource1;
>         int* ro2=resource2;
>         //赋值
>         resource1=new int(*other.resource1);
>         resource2=new int(*other.resource2);
>         //精髓在于先new后delete，避免资源被意外删除
>         delete ro1;delete ro2;
>         return *this;
>     }
> 
>     // 析构函数
>     ~ResourceHolder() {
>         delete resource1;
>         delete resource2;
>     }
> };
> 
> int main() {
>     try {
>         ResourceHolder holder1(10, 20);
>         ResourceHolder holder2(30, 40);
> 
>         // 这里进行拷贝赋值操作
>         holder2 = holder1;
>     } catch (const std::exception& e) {
>         std::cerr << "Exception caught: " << e.what() << std::endl;
>     }
> 
>     return 0;
> }
> ```


> **最终似乎掌握这个就好了**："Copy and swap"技巧是一种用于实现C++类拷贝赋值函数的常用技术，主要目的是通过交换成员变量的方式来实现赋值操作，**同时处理自赋值和异常安全性**。
> 
> 下面是一个简单的示例，演示如何使用"copy and swap"技巧来实现拷贝赋值函数：
> 
> ```cpp
> #include <iostream>
> #include <cstring> // for std::swap
> class Example {
> private:
>     int* data;
> public:
>     // Constructor
>     Example(int d){
>         data=new int(d);
>         std::cout << "Constructor called " << *data << std::endl;
>     } 
>     // Destructor
>     ~Example() {
>         std::cout << "Destructor called " << *data << std::endl;
>         delete data;
>     }
>     // Copy constructor
>     Example(const Example& other){
>         data=new int(*other.data);
>         std::cout << "Copy constructor called\n";
>     }
>     // Assignment operator using copy-and-swap
>     Example& operator=(Example other) { //调用copy ctor
>         std::cout << "Assignment operator called\n";
>         std::swap(data, other.data);
>         return *this;
>     }
>     int getData(){
>         return *data;
>     }
> };
> 
> int main() {
>     Example obj1(5); // Constructor called
> 
>     Example obj2(10); // Constructor called
>     std::cout << obj1.getData() << " " << obj2.getData() << std::endl;
>  
>     obj2 = obj1; // Assignment operator called 
>     std::cout << obj1.getData() << " " << obj2.getData() << std::endl; 
>     return 0;
> }
> ```
> 
> 在上面的示例中，关键点是在拷贝赋值操作符重载函数中，参数`other`是按值传递的，这意味着在函数开始时会调用复制构造函数来创建`other`的副本(`copy`)。然后，通过调用`swap`函数来交换`*this`和`other`副本的内容，从而实现了赋值操作，同时这将不会修改other的内容，因为按值传递后swap交换的只是other的副本。在这个例子中，交换前是5，10，交换后是5，5，并会因为调用了swap而变成10，5
> 
> 这种技巧的优势在于它能够简化代码并确保异常安全性，因为在交换发生之前，`other`的副本已经被成功创建，即使交换过程中发生异常，也不会影响到原始对象。
> 如果op=的参数是`const Example& other`而使得参数是按引用传递，之前的swap版本就会修改赋值者和被赋值者，此时可以手动创建副本（`copy`），然后交换（`swap`）
> ```cpp
> Example tmp(other);
> std::swap(data,tmp.data)
> ```

## item 12: Copy all parts of an object

When you're writing a copying function, be sure to
- copy all local data member
- invoke the appropriate copying function in all base classes

Don't try to implement one of the copying functions in term of the other. Instead, put common functionality in a third function that both call.


# 3.Resource Management

## item13: Use objects to manage resources

To make sure that the resource is always released, we need to put that resource inside an object whose destructor will automatically release the resource when control leaves function.


> 在 C++ 程序结束时，对象的析构函数会被自动调用。这是 C++ 对象生命周期管理的一部分。具体来说：
> 
> 1. **自动变量**：在函数或代码块中定义的局部变量，当程序执行离开变量所在的作用域时（比如函数返回或代码块结束），这些变量的析构函数会被自动调用。
> 
>     ```cpp
>     void someFunction() {
>         MyClass obj; // obj 是一个自动变量
>         // do something
>     } // 这里 obj 的析构函数会被自动调用
>     ```
> 
> 2. **全局和静态变量**：全局变量和静态变量的析构函数在程序结束时会被自动调用。全局变量的析构函数在 `main` 函数返回之后调用，静态变量的析构函数在其作用域结束后调用。
> 
>     ```cpp
>     MyClass globalObj; // 全局变量
> 
>     void someFunction() {
>         static MyClass staticObj; // 静态局部变量
>         // do something
>     }
> 
>     int main() {
>         someFunction();
>         // do something
>     } // 这里 globalObj 和 staticObj 的析构函数会被自动调用
>     ```
> 
> 3. **动态分配的对象**：对于使用 `new` 分配的对象，必须手动使用 `delete` 来销毁对象并调用其析构函数。如果使用智能指针（如 `std::unique_ptr` 或 `std::shared_ptr`）来管理动态分配的对象，当智能指针超出作用域或被重置时，智能指针会自动删除对象并调用其析构函数。
> 
>     ```cpp
>     void someFunction() {
>         MyClass* obj = new MyClass; // 动态分配的对象
>         // do something
>         delete obj; // 必须手动调用 delete 来销毁对象并调用析构函数
>     }
> 
>     void someFunctionWithSmartPointer() {
>         std::unique_ptr<MyClass> obj = std::make_unique<MyClass>(); // 使用智能指针管理动态分配的对象
>         // do something
>     } // 这里 obj 超出作用域，析构函数会被自动调用
>     ```
> 
> 总之，C++ 对象的析构函数会在适当的时间点被自动调用，以确保对象资源的正确释放。

> `std::auto_ptr` 是 C++98 引入的一种智能指针，用于管理动态分配的对象。然而，它在 C++11 中被弃用，并在 C++17 中被完全移除。取而代之的是更安全、更高效的 `std::unique_ptr` 和 `std::shared_ptr`。尽管如此，为了了解 `std::auto_ptr` 的使用方法，以下是一个简单的示例：
> 
> ```cpp
> #include <iostream>
> #include <memory>
> 
> class MyClass {
> public:
>     MyClass() { std::cout << "Constructor called" << std::endl; }
>     ~MyClass() { std::cout << "Destructor called" << std::endl; }
>     void display() { std::cout << "Displaying MyClass object" << std::endl; }
> };
> 
> int main() {
>     std::auto_ptr<MyClass> ptr1(new MyClass());
>     ptr1->display();
>     
>     std::auto_ptr<MyClass> ptr2;
>     ptr2 = ptr1; // ptr1 转移所有权给 ptr2,任何赋值将会把原先的对象设置为空，因为不能有两个智能指针指向同一个对象，否则那个对象可能会被删除两次
>     if (!ptr1) {
>         std::cout << "ptr1 is now empty" << std::endl;
>     }
>     ptr2->display();
> 
>     return 0;
> }
> ```
> 
> 在上面的示例中，注意以下几点：
> 
> 1. **构造函数**：`std::auto_ptr` 可以通过一个指向动态分配对象的指针进行初始化。
> 2. **所有权转移**：当 `std::auto_ptr` 被赋值或复制时，所有权会从一个 `std::auto_ptr` 转移到另一个 `std::auto_ptr`。在赋值操作之后，原来的 `std::auto_ptr` 会变为空指针。
> 3. **自动释放**：当 `std::auto_ptr` 超出其作用域时，指向的对象会自动被删除。
> 
> 由于 `std::auto_ptr` 的所有权转移机制很容易导致未定义行为和其他问题，建议使用现代的智能指针，如 `std::unique_ptr` 和 `std::shared_ptr`。
> 
> 以下是使用 `std::unique_ptr` 实现相同功能的示例：
> 
> ```cpp
> #include <iostream>
> #include <memory>
> 
> class MyClass {
> public:
>     MyClass() { std::cout << "Constructor called" << std::endl; }
>     ~MyClass() { std::cout << "Destructor called" << std::endl; }
>     void display() { std::cout << "Displaying MyClass object" << std::endl; }
> };
> 
> int main() {
>     std::unique_ptr<MyClass> ptr1(new MyClass());
>     ptr1->display();
>     
>     std::unique_ptr<MyClass> ptr2 = std::move(ptr1); // ptr1 转移所有权给 ptr2
>     if (!ptr1) {
>         std::cout << "ptr1 is now empty" << std::endl;
>     }
>     ptr2->display();
> 
>     return 0;
> }
> ```
> 
> 使用 `std::unique_ptr` 可以避免 `std::auto_ptr` 的许多缺陷，同时提供更强的类型安全性和更好的性能。

> RAII（Resource Acquisition Is Initialization，资源获取即初始化）是 C++ 中的一种管理资源（如内存、文件句柄、网络连接等）的方法。这种方法确保资源在对象的生命周期内被正确管理，当对象不再需要时（超出作用域或被销毁），资源会自动被释放。
> 
> ### RAII 的核心思想
> 
> 1. **资源的获取**：资源在对象创建（初始化）时被获取。这通常是在构造函数中进行的。
> 2. **资源的释放**：资源在对象销毁（超出作用域或被显式删除）时被释放。这是在析构函数中进行的。
> 
> ### RAII 的优点
> 
> - **异常安全**：RAII 确保资源即使在异常情况下也能被正确释放，从而避免资源泄漏。
> - **简化代码**：RAII 减少了显式管理资源的代码量，简化了资源管理逻辑。
> 
> ### 示例
> 
> 以下是一个使用 RAII 管理动态内存的示例：
> 
> ```cpp
> #include <iostream>
> 
> class RAII {
> private:
>     int* data;
> public:
>     // 构造函数：分配资源
>     RAII(size_t size) : data(new int[size]) {
>         std::cout << "Resource acquired" << std::endl;
>     }
> 
>     // 析构函数：释放资源
>     ~RAII() {
>         delete[] data;
>         std::cout << "Resource released" << std::endl;
>     }
> 
>     // 其他成员函数
>     void doSomething() {
>         std::cout << "Doing something with the resource" << std::endl;
>     }
> };
> 
> int main() {
>     {
>         RAII raii(10); // 创建对象，分配资源
>         raii.doSomething();
>     } // 超出作用域，自动调用析构函数，释放资源
>     return 0;
> }
> ```
> 
> 在这个示例中，当 `RAII` 对象被创建时，构造函数分配动态内存；当对象超出作用域时，析构函数自动释放内存。即使在异常情况下，析构函数也会被调用，从而确保资源被正确释放。
> 
> ### 智能指针与 RAII
> 
> C++11 引入了智能指针（如 `std::unique_ptr` 和 `std::shared_ptr`），它们也是 RAII 的典型应用。以下是使用 `std::unique_ptr` 的示例：
> 
> ```cpp
> #include <iostream>
> #include <memory>
> 
> void useResource() {
>     std::unique_ptr<int[]> data(new int[10]); // 分配资源
>     std::cout << "Resource acquired" << std::endl;
>     // 自动释放资源，无需显式调用 delete
>     std::cout << "Resource will be released automatically" << std::endl;
> }
> 
> int main() {
>     useResource();
>     return 0;
> }
> ```
> 
> 在这个示例中，`std::unique_ptr` 自动管理动态分配的内存，当 `data` 超出作用域时，内存会被自动释放。智能指针通过 RAII 提供了异常安全和简化代码的优势。


> `shared_ptr` 是 C++ 标准库中的一种智能指针，属于引用计数型智能指针（Reference Counted Smart Pointer，RCSP）。`shared_ptr` 的主要功能是管理动态分配的对象，并确保在没有更多的 `shared_ptr` 指向该对象时自动删除对象，以防止内存泄漏。
> 
> 在 `shared_ptr` 中，每个 `shared_ptr` 实例都包含一个指向共享对象的指针和一个指向引用计数的指针。每当一个新的 `shared_ptr` 复制或者赋值时，引用计数会增加；每当一个 `shared_ptr` 被销毁或者重置时，引用计数会减少。当引用计数减少到零时，共享对象会被删除。
> 
> 下面是一个简单的示例代码，演示了 `shared_ptr` 的使用：
> 
> ```cpp
> #include <iostream>
> #include <memory>
> 
> class Test {
> public:
>     Test() { std::cout << "Test Constructor\n"; }
>     ~Test() { std::cout << "Test Destructor\n"; }
> };
> 
> int main() {
>     std::shared_ptr<Test> ptr1 = std::make_shared<Test>();
>     {
>         std::shared_ptr<Test> ptr2 = ptr1;
>         std::cout << "Inside scope: ptr2 use count = " << ptr2.use_count() << "\n";
>     }
>     std::cout << "Outside scope: ptr1 use count = " << ptr1.use_count() << "\n";
>     return 0;
> }
> ```
> 
> 在这个示例中，`Test` 类的构造函数和析构函数会在对象创建和销毁时打印消息。`ptr1` 和 `ptr2` 是两个 `shared_ptr`，它们都指向同一个 `Test` 对象。在内部作用域结束后，`ptr2` 被销毁，引用计数减少，最后在程序结束时 `ptr1` 被销毁，引用计数归零，`Test` 对象被删除。

智能指针的默认删除方式是 `delete`，而不是 `delete[]`。这对于指向单个对象的智能指针是合适的，但对于指向数组的智能指针来说，会导致未定义行为，因为数组需要使用 `delete[]` 来正确释放内存。

> `std::unique_ptr` 和 `std::shared_ptr` 的默认删除方式
> 
> - `std::unique_ptr`：从 C++11 开始，`std::unique_ptr` 通过使用 `std::default_delete` 模板类来提供默认删除器。对于指向单个对象的 `std::unique_ptr`，`std::default_delete` 使用 `delete`；对于指向数组的 `std::unique_ptr`，`std::default_delete` 使用 `delete[]`。
> 
>   示例：
>   ```cpp
>   #include <iostream>
>   #include <memory>
> 
>   int main() {
>       // 指向单个对象的 std::unique_ptr
>       std::unique_ptr<int> ptr1(new int(5));
> 
>       // 指向数组的 std::unique_ptr
>       std::unique_ptr<int[]> ptr2(new int[5]);
> 
>       // 正确使用数组
>       for (int i = 0; i < 5; ++i) {
>           ptr2[i] = i;
>           std::cout << ptr2[i] << " ";
>       }
> 
>       std::cout << std::endl;
> 
>       return 0;
>   }
>   ```
> 
> - `std::shared_ptr`：`std::shared_ptr` 默认使用 `delete` 作为删除器。因此，如果要使用 `std::shared_ptr` 来管理数组，需要显式提供一个删除器，如 `std::default_delete<int[]>`。
> 
>   示例：
>   ```cpp
>   #include <iostream>
>   #include <memory>
> 
>   int main() {
>       // 使用自定义删除器 std::default_delete<int[]>
>       std::shared_ptr<int> ptr(new int[5], std::default_delete<int[]>());
> 
>       // 正确使用数组
>       for (int i = 0; i < 5; ++i) {
>           ptr.get()[i] = i;
>           std::cout << ptr.get()[i] << " ";
>       }
> 
>       std::cout << std::endl;
> 
>       return 0;
>   }
>   ```
> 
> - 对于 `std::unique_ptr`，可以直接使用它来管理数组，因为它已经为数组提供了默认的 `delete[]` 删除器。
> - 对于 `std::shared_ptr`，需要显式指定一个合适的删除器来正确管理数组，否则会导致未定义行为。
> 
> 使用智能指针时，理解它们的默认删除方式并确保正确管理动态内存是非常重要的。


## item 14: Think carefully about copying behavior in resource-managing classes

> 在C++中，"synchronization primitives"（同步原语）是指一组用于管理和协调多线程程序中对共享资源访问的基本构件。它们确保在并发环境中，多个线程能够安全地访问和修改共享数据，而不会导致数据竞争或其它并发问题。
> 
> 常见的同步原语包括：
> 
> 1. **Mutex（互斥锁）**：
>    - 用于保证同一时刻只有一个线程可以访问共享资源。C++标准库提供了`std::mutex`类。
> 
> 2. **Recursive Mutex（递归互斥锁）**：
>    - 允许同一个线程多次获得锁，而不会导致死锁。C++标准库提供了`std::recursive_mutex`类。
> 
> 3. **Timed Mutex（定时互斥锁）**：
>    - 允许线程在尝试获取锁时设置一个超时时间。如果在指定时间内无法获取锁，线程将停止尝试。C++标准库提供了`std::timed_mutex`类。
> 
> 4. **Shared Mutex（共享互斥锁）**：
>    - 允许多个线程同时读取共享资源，但写入操作必须独占。C++17引入了`std::shared_mutex`类。
> 
> 5. **Condition Variable（条件变量）**：
>    - 用于线程间的通信，允许一个线程等待某个条件发生，并在条件满足时被通知。C++标准库提供了`std::condition_variable`类。
> 
> 6. **Latch和Barrier**：
>    - 用于协调多个线程在某个点上进行同步。`std::latch`和`std::barrier`在C++20中引入。
> 
> 7. **Atomic Operations（原子操作）**：
>    - 提供对基本数据类型的原子操作，确保这些操作在多线程环境中是安全的。C++标准库提供了`std::atomic`模板类。
> 
> 这些同步原语帮助开发者确保多线程程序中的数据一致性和正确性。通过合理使用这些工具，可以有效避免数据竞争、死锁等常见的并发编程问题。


What should happen when an RAII object is copied? Most of the time, you'll want to choose one  of the following possibilities:
- prohibit copying like item6 does
- reference-count the underlying resource by containing a shared_ptr data member, and initialize shared_ptr with specification of a "deleter"
- copying the underlying resource
- transfer ownership of the underlying resource

## Item 15: Provide access to raw resources in resource-managing class

> 在 C++ 中，智能指针是一种自动管理内存的工具，主要通过 RAII（Resource Acquisition Is Initialization）机制来管理资源的生命周期。智能指针不仅提供了基础指针的功能，还可以在离开作用域时自动释放资源，从而避免内存泄漏。
> 
> C++11 引入了标准库中的几种常用智能指针：`std::unique_ptr`、`std::shared_ptr` 和 `std::weak_ptr`。这些智能指针实现了 `get()` 方法和解引用操作。
> 
> ### `get()` 方法
> 
> - **`std::unique_ptr<T>::get()`**：返回原生指针，但不转移所有权。
> - **`std::shared_ptr<T>::get()`**：返回原生指针，但不影响引用计数。
> 
> ### 解引用操作符
> 
> - **`operator*()`**：返回所指对象的引用。
> - **`operator->()`**：返回所指对象的指针。
> 
> ### 代码示例
> 
> 以下是 `std::unique_ptr` 和 `std::shared_ptr` 的使用示例：
> 
> ```cpp
> #include <iostream>
> #include <memory>
> 
> class MyClass {
> public:
>     void display() const {
>         std::cout << "Hello from MyClass!" << std::endl;
>     }
> };
> 
> int main() {
>     // 使用 unique_ptr
>     std::unique_ptr<MyClass> uniquePtr = std::make_unique<MyClass>();
>     uniquePtr->display(); // 解引用操作
>     MyClass* rawPtr1 = uniquePtr.get(); // 获取原生指针
>     rawPtr1->display();
> 
>     // 使用 shared_ptr
>     std::shared_ptr<MyClass> sharedPtr = std::make_shared<MyClass>();
>     sharedPtr->display(); // 解引用操作
>     MyClass* rawPtr2 = sharedPtr.get(); // 获取原生指针
>     rawPtr2->display();
> 
>     return 0;
> }
> ```
> 
> ### 智能指针的优点
> 
> - **自动释放资源**：智能指针在离开作用域时自动释放资源，无需手动调用 `delete`。
> - **防止内存泄漏**：智能指针确保所有权管理，防止内存泄漏。
> - **线程安全**：`std::shared_ptr` 是线程安全的，可以在多个线程中安全使用。
> 
> ### 注意事项
> 
> - **循环引用**：使用 `std::shared_ptr` 时要注意循环引用问题，可以通过 `std::weak_ptr` 解决。
> - **性能开销**：智能指针在管理资源时会有一定的性能开销，特别是在多线程环境中使用 `std::shared_ptr` 时。
> 
> 总的来说，智能指针是现代 C++ 编程中的重要工具，有助于更安全和高效地管理动态内存。

> 是的，C++ 标准库中的智能指针提供了对原生指针类型的隐式类型转换。这使得智能指针在需要使用原生指针的地方（例如，与 C API 交互）可以无缝地转换为原生指针。下面是 `std::unique_ptr` 和 `std::shared_ptr` 的隐式类型转换的详细说明：
> 
> ### `std::unique_ptr`
> 
> `std::unique_ptr` 提供了一个隐式转换操作符，可以将 `std::unique_ptr` 对象转换为原生指针类型。
> 
> ```cpp
> #include <iostream>
> #include <memory>
> 
> class MyClass {
> public:
>     void display() const {
>         std::cout << "Hello from MyClass!" << std::endl;
>     }
> };
> 
> void useRawPointer(MyClass* ptr) {
>     if (ptr) {
>         ptr->display();
>     }
> }
> 
> int main() {
>     std::unique_ptr<MyClass> uniquePtr = std::make_unique<MyClass>();
>     useRawPointer(uniquePtr.get()); // 使用 get 方法
>     useRawPointer(uniquePtr); // 隐式转换
>     return 0;
> }
> ```
> 
> ### `std::shared_ptr`
> 
> 同样地，`std::shared_ptr` 也提供了隐式转换操作符，可以将 `std::shared_ptr` 对象转换为原生指针类型。
> 
> ```cpp
> #include <iostream>
> #include <memory>
> 
> class MyClass {
> public:
>     void display() const {
>         std::cout << "Hello from MyClass!" << std::endl;
>     }
> };
> 
> void useRawPointer(MyClass* ptr) {
>     if (ptr) {
>         ptr->display();
>     }
> }
> 
> int main() {
>     std::shared_ptr<MyClass> sharedPtr = std::make_shared<MyClass>();
>     useRawPointer(sharedPtr.get()); // 使用 get 方法
>     useRawPointer(sharedPtr); // 隐式转换
>     return 0;
> }
> ```
> 
> ### 注意事项
> 
> 虽然智能指针提供了隐式转换，但在使用时需要注意以下几点：
> 
> 1. **生命周期管理**：隐式转换获得的原生指针不管理对象的生命周期，因此需要确保在使用原生指针时，智能指针仍然有效。
> 2. **循环引用**：特别是在使用 `std::shared_ptr` 时，要注意循环引用的问题，避免导致内存泄漏。
> 
> 通过隐式转换，智能指针可以更方便地与需要原生指针的代码进行交互，同时仍然享有智能指针带来的安全性和资源管理的优势。

Like most well-designed classes, it hides what clients don't need to see, but it makes available those things that clients honestly need to access.

## item 16: Use the same form in corresponding uses of new and delete


![搭配使用](16-corr.png)
![new](16-new.png)
![delete](16-delete.png)


To avoid confusion, abstain from `typedef` for array types, use vector.

## item 17: Store newed object in smart pointers in standalone statements

> 在 C++ 中，使用 `std::shared_ptr` 来管理动态分配的对象通常可以有效地防止内存泄漏。然而，在特定情况下，可能会引起微妙的内存泄漏问题。
> 
> ```cpp
> func(shared_ptr<foo>(new foo), priority());
> ```
> 
> 这个调用可能引发内存泄漏的原因是，如果在构造 `std::shared_ptr<foo>` 对象时抛出异常或者 priority() 中抛出异常，当编译器的执行顺序是
> 
> - new foo
> - priority()
> - ctor of shared_ptr
> 
> ，可能会导致 `new foo` 分配的内存未被释放。具体步骤如下：
> 
> 1. `new foo` 创建一个 `foo` 对象，并返回一个原始指针。
> 2. 这个指针被传递给 `std::shared_ptr<foo>` 的构造函数。
> 3. 如果在 `std::shared_ptr<foo>` 构造过程中抛出异常（例如，分配 `shared_ptr` 管理块的内存失败），则 `new foo` 分配的内存不会被释放，因为没有 `shared_ptr` 成功创建来管理它。
> 
> 为了防止这种情况，可以使用 `std::make_shared`，并且声明在单独的一行中，不与priority()在一起，它会在单一步骤中进行对象创建和 `shared_ptr` 的管理内存分配，从而避免上述问题：
> ```cpp
> std::shared_ptr<foo> p = std::make_shared<foo>();
> func(p, priority());
> ```
> 
> 使用 `std::make_shared` 会更安全和高效，因为它确保了对象和 `shared_ptr` 的控制块同时分配和构造，从而避免了潜在的异常安全问题。
"


# 4.Designs and Declarations

## Item 18: Make interfaces easy to use correctly and hard to use incorrectly

Developing interfaces that are easy to use correctly and hard to use incorrectly requires that you consider the kinds of mistakes that clients might make.

The type system is your primary ally in preventing undesirable code from compiling, such as introducing simple wrapper types.

> 在 C++ 中，cross-DLL 问题（跨 DLL 问题）通常指的是在不同的动态链接库（DLL）之间共享和管理对象和函数时所遇到的一些问题。主要的问题包括：
> 
> 1. **内存管理**：如果一个 DLL 分配了内存（例如通过 `new`），另一个 DLL 可能无法正确释放这块内存，因为它们可能使用不同的内存分配器。这会导致内存泄漏或崩溃。
> 
> 2. **对象的生命周期和类型信息**：如果你在一个 DLL 中创建了一个对象，而在另一个 DLL 中使用它，这可能导致类型信息和对象生命周期的问题。例如，如果对象的虚函数表（vtable）不一致，调用虚函数可能会导致崩溃。
> 
> 3. **符号导出和导入**：不同的 DLL 可能会有不同的符号导出和导入规则，导致链接问题。如果函数和类在 DLL 之间共享，确保它们的符号导出正确非常重要。
> 
> 4. **ABI（应用二进制接口）兼容性**：如果 DLL 使用了不同的编译器或编译器设置，它们的 ABI 可能不兼容，导致函数调用失败或数据结构不匹配。
> 
> 5. **全局变量和静态数据**：如果 DLL 之间共享全局变量或静态数据，可能会遇到数据不一致或状态不同步的问题。
> 
> 要解决这些问题，可以采取一些策略：
> 
> - **使用接口和抽象类**：在 DLL 之间传递对象时，使用纯虚类或接口来隐藏实现细节，这样可以减少对具体实现的依赖。
> - **共享内存管理代码**：如果需要跨 DLL 共享内存，确保所有 DLL 使用相同的内存管理代码或库。
> - **确保 ABI 兼容性**：尽量使用相同的编译器和编译器设置来确保 ABI 兼容。
> - **明确导出符号**：使用 `__declspec(dllexport)` 和 `__declspec(dllimport)` 指令来明确导出和导入符号。
> 
> 这些措施可以帮助减少或避免 cross-DLL 问题，提高 DLL 之间的互操作性。

shared_ptr is an easy way to eliminate some client error.

## item 19: Treat class design as type design

The answers to the following questions often lead to constraints on your design:
- How should objects of your new type be created and destroyed
- How should object initialization differ from object assignment
- What does it mean for objects of your new type to be passed by value
> 在C++中，拷贝构造函数在以下几种情况中会被调用，其中包括按值传递对象的情况：
> 
> 1. **按值传递对象**：
>    当一个对象以值传递的方式传递给函数时，函数的参数会对该对象进行拷贝，这时会调用拷贝构造函数。例如：
>    ```cpp
>    void func(MyClass obj) {
>        // 在这里拷贝构造函数已经被调用
>    }
>    ```
> 
> 2. **按值返回对象**：
>    当一个函数按值返回一个对象时，会调用拷贝构造函数来创建返回值。例如：
>    ```cpp
>    MyClass createObject() {
>        MyClass obj;
>        return obj; // 这里会调用拷贝构造函数
>    }
>    ```
> 
> 3. **用一个对象初始化另一个对象**：
>    当使用一个对象来初始化另一个对象时，会调用拷贝构造函数。例如：
>    ```cpp
>    MyClass obj1;
>    MyClass obj2 = obj1; // 这里会调用拷贝构造函数
>    ```
> 
> 4. **通过对象的拷贝初始化容器中的元素**：
>    当将对象存储在STL容器中时，容器有时会对对象进行拷贝，从而调用拷贝构造函数。例如：
>    ```cpp
>    std::vector<MyClass> vec;
>    MyClass obj;
>    vec.push_back(obj); // 这里会调用拷贝构造函数
>    ```
> 
> 下面是一个简单的示例，展示了拷贝构造函数在按值传递时的调用：
> 
> ```cpp
> #include <iostream>
> 
> class MyClass {
> public:
>     MyClass() {
>         std::cout << "Default constructor called" << std::endl;
>     }
> 
>     MyClass(const MyClass& other) {
>         std::cout << "Copy constructor called" << std::endl;
>     }
> };
> 
> void func(MyClass obj) {
>     std::cout << "Function body" << std::endl;
> }
> 
> int main() {
>     MyClass obj;
>     func(obj); // 调用拷贝构造函数
>     return 0;
> }
> ```
> 
> 在上述代码中，当`func`函数以值传递的方式接收`MyClass`对象时，会调用拷贝构造函数。运行结果如下：
> 
> ```
> Default constructor called
> Copy constructor called
> Function body
> ```
> 
> 由此可见，拷贝构造函数在按值传递对象时会被调用。
 
- What are the restrictions on legal values for your new type
- Does your new type fit into an inheritance graph
> 在C++中，当你有一个多态（polymorphic）基类时，基类的析构函数最好声明为虚函数。这是为了确保当你通过基类指针删除派生类对象时，会正确调用派生类的析构函数，从而避免资源泄漏或未定义行为。

> 在C++中，子类的析构函数会自动调用父类的析构函数，这是正确的。然而，子类的构造函数不会自动调用父类的构造函数。需要通过显式调用父类构造函数来实现。具体解释如下：
> 
> 1. **构造函数**：
>     - 当创建子类对象时，子类的构造函数首先被调用。
>     - 然后，子类构造函数必须显式地调用父类构造函数。这通常在子类构造函数的初始化列表中完成。
>     - 如果不显式调用父类构造函数，C++编译器会自动调用父类的默认构造函数（即没有参数的构造函数）。如果父类没有默认构造函数，且没有在子类构造函数中显式调用其他构造函数，则会导致编译错误。
> 
>     例如：
>     ```cpp
>     class Base {
>     public:
>         Base(int value) {
>             // 构造函数实现
>         }
>     };
> 
>     class Derived : public Base {
>     public:
>         Derived(int value) : Base(value) {
>             // Derived 的构造函数实现
>         }
>     };
>     ```
> 
> 2. **析构函数**：
>     - 当销毁子类对象时，子类的析构函数首先被调用。
>     - 在子类的析构函数完成后，父类的析构函数会自动被调用。
>     - 这确保了对象销毁时，资源可以被正确释放，避免内存泄漏等问题。
> 
>     例如：
>     ```cpp
>     class Base {
>     public:
>         virtual ~Base() {
>             // 析构函数实现
>         }
>     };
> 
>     class Derived : public Base {
>     public:
>         ~Derived() override {
>             // Derived 的析构函数实现
>         }
>     };
>     ```
> 
> 总结：子类的构造函数不会自动调用父类构造函数，需要显式调用；而子类的析构函数会自动调用父类的析构函数。

- What kind of type conversions are  allowed for your new type
- What operators and functions make sense for the new type
- What standard functions should be disallowed such as declaring private
- Whow should have access to the members of your new type
- what is the "undeclared interface" of your new type
- How general is your new type such as class template
- Is a new type really what you need

## Item 20: Prefer pass-by-reference-to-const to pass-by-value

> 在C++中，将子类对象传递给函数的父类形参可能会导致切片（object slicing）问题。切片问题发生在将子类对象作为父类对象传递时，子类特有的成员和行为被“切掉”，只保留了父类部分。
> 
> - 切片问题示例
> ```cpp
> #include <iostream>
> using namespace std;
> 
> class Base {
> public:
>     int baseValue;
>     virtual void display() const {
>         cout << "Base value: " << baseValue << endl;
>     }
> };
> 
> class Derived : public Base {
> public:
>     int derivedValue;
>     void display() const override {
>         cout << "Base value: " << baseValue << ", Derived value: " << derivedValue << endl;
>     }
> };
> 
> void printBase(Base b) {
>     b.display(); // 这里只会调用Base::display()，即使传递的是Derived对象
> }
> 
> int main() {
>     Derived d;
>     d.baseValue = 10;
>     d.derivedValue = 20;
>     
>     printBase(d); // 发生切片问题，只保留了Base部分
>     return 0;
> }
> ```
> 在这个示例中，`printBase`函数接收一个`Base`对象。当我们将`Derived`对象`d`传递给`printBase`时，由于参数按值传递，实际传递的是一个新的`Base`对象拷贝，这个拷贝对象没有`Derived`类中的`derivedValue`成员。
> 
> - 解决方法
> 要避免切片问题，可以使用以下几种方法：
> 
> 1. **使用指针或引用传递**：
>    通过传递指针或引用，可以保持对象的完整性，并且保留多态性。
>    ```cpp
>    void printBase(const Base& b) {
>        b.display(); // 这里会调用Derived::display()，如果传递的是Derived对象
>    }
>    
>    int main() {
>        Derived d;
>        d.baseValue = 10;
>        d.derivedValue = 20;
>        
>        printBase(d); // 不会发生切片问题
>        return 0;
>    }
>    ```
> 
> 2. **使用智能指针**：
>    智能指针不仅可以避免切片问题，还能管理对象的生命周期，防止内存泄漏。
>    ```cpp
>    #include <memory>
>    
>    void printBase(const std::shared_ptr<Base>& b) {
>        b->display(); // 这里会调用Derived::display()，如果传递的是Derived对象
>    }
>    
>    int main() {
>        std::shared_ptr<Derived> d = std::make_shared<Derived>();
>        d->baseValue = 10;
>        d->derivedValue = 20;
>        
>        printBase(d); // 不会发生切片问题
>        return 0;
>    }
>    ```
> 
> 通过上述方法，可以有效避免在C++中因将子类对象传递给函数的父类形参而导致的切片问题。


In general, the only types for  which you can reasonably assume that pass-by-value is inexpensive are **build-in types and STL iterator and function object**.

> 在C++中，按值传递对象有时是合理的，但这取决于具体的情况。以下是一些通常适合按值传递的对象：
> 
> 1. **内建类型**：如int, float, char等基本数据类型，按值传递效率高。 
> 2. **函数对象（functor）**：很多情况下，传递函数对象（如lambda表达式）按值传递是高效且方便的。
> 
> - 示例
> 
> 以下是一些适合按值传递的示例：
> 
> ```cpp
> void processPoint(Point p) {
>     // 按值传递小型POD类型
> }
> 
> void processPair(std::pair<int, int> p) {
>     // 按值传递轻量级对象
> }
> 
> void processFunctor(std::function<void()> f) {
>     // 按值传递函数对象
> }
> ```
> 
> 需要注意的是，按值传递对象时会进行对象的拷贝，这在对象较大或拷贝成本较高时可能会带来性能问题。在这种情况下，通常推荐按引用传递（包括常引用`const&`）或使用移动语义（C++11及以上）来优化性能。
> 
> - 何时避免按值传递
> 
> 1. **大型对象**：包含大量数据成员的类或结构体，按值传递会导致高昂的拷贝成本。
> 2. **动态分配内存的对象**：如包含指针成员或管理资源的类，按值传递可能引起资源的非必要拷贝。
> 3. **需要保持对象状态一致的情况**：按值传递会创建对象的副本，可能导致状态不同步。
> 
> 综合考虑对象的大小、拷贝成本及其使用场景来决定是否按值传递，是一个重要的优化技巧。

## Item 21: Don't try to return a reference when you must return an object

Never return a pointer or reference to a local stack object, a reference to heap-allocated object (noone can delete pointer otherwise), or a pointer or reference to a local static object if there is a chance that more than one such object will be needed.

## Item 22: Declare data members private

use private data members:
- syntactic consistency for access anything
- more pricise control over the accessibility of data member
```cpp

#include <iostream>

class AccessLevels {
private:
    // 不可访问数据
    int inaccessibleData;

    // 只读数据（通过const成员函数访问）
    int readOnlyData;
    
    // 只写数据，不能访问它现在的值
    int writeOnlyData;

    // 可读可写数据
    int readWriteData;
public:
    
    //可读可写
    int getReadWriteData() const {
        return readWriteData;
    }
    
    void setReadWriteData(int value){
        readWriteData=value;
    }
    
    //只能读readOnlyData
    int getReadOnlyData() const {
        return readOnlyData;
    }

    // 只写数据（通过成员函数写入）
    void setWriteOnlyData(int value) {
        writeOnlyData = value;
    }


```
- Encapsulation. Hiding data member behind fuctional interface can offer all kinds of implementation flexibility.

if you don't hide data members, you'll soon find that even if you own the source code to a class, your ability to change anything public is extremely restricted, because too much client code will be broken. **Unencapsulated means unchangeable**, especially for classes that are widely used.

**Something's encapsulation is inversely proportional to the amount of code that might be broken if that something changes.** For public data members, all client code that uses it. For protected data member, all derived class that use it.

Protected data members are unencapsulated as public ones. From an encapsulation point of view, there are really **only two access levels**: private (which offer encapsulation) and everything else (which doesn't).

## Item 23: Prefer non-member non-friend functions to member function

non-member non-friend functions offer more encapsulation.

the more functions can access data, the less encapsulated the data.

> 在 C++ 中，`friend` 关键字用于授予一个类、函数或另一个类对该类的私有（private）和保护（protected）成员的访问权限。`friend` 主要用于以下两种情况：
> 
> 1. **友元函数（Friend Function）**：允许一个非成员函数访问类的私有和保护成员。
> 2. **友元类（Friend Class）**：允许一个类访问另一个类的私有和保护成员。
> 
> ### 友元函数
> 
> 友元函数是一个不是类成员的函数，但它可以访问类的私有和保护成员。通过在类中声明一个函数为友元函数，类的私有和保护成员将对这个函数可见。
> 
> ```cpp
> #include <iostream>
> 
> class Box {
> private:
>     double width;
> 
> public:
>     Box() : width(0.0) {}
> 
>     // 声明 friend 函数
>     friend void printWidth(Box box);
> };
> 
> // 定义 friend 函数
> void printWidth(Box box) {
>     std::cout << "Width of box: " << box.width << std::endl;
> }
> 
> int main() {
>     Box box;
>     printWidth(box); // 调用友元函数
>     return 0;
> }
> ```
> 
> ### 友元类
> 
> 友元类是一个可以访问另一个类的私有和保护成员的类。通过在一个类中声明另一个类为友元类，前者的私有和保护成员将对后者可见。
> 
> ```cpp
> #include <iostream>
> 
> class B;
> 
> class A {
> private:
>     int a;
> 
> public:
>     A() : a(0) {}
>     friend class B; // 声明 B 为友元类
> };
> 
> class B {
> public:
>     void setA(A& obj, int value) {
>         obj.a = value; // 访问 A 的私有成员
>     }
> 
>     void printA(A& obj) {
>         std::cout << "Value of a: " << obj.a << std::endl;
>     }
> };
> 
> int main() {
>     A objA;
>     B objB;
>     objB.setA(objA, 10);
>     objB.printA(objA);
>     return 0;
> }
> ```
> 
> ### 使用友元的注意事项
> 
> 1. **破坏封装性**：友元函数和友元类破坏了类的封装性，因为它们可以访问类的私有和保护成员。使用友元时需要谨慎，尽量保持封装性。
> 
> 2. **维护复杂性**：友元函数和友元类增加了类之间的耦合度，可能使代码的维护变得更加复杂。
> 
> 3. **使用场景**：友元通常在需要直接访问类的内部状态或需要两个类紧密协作的场景中使用。
> 
> 总结来说，`friend` 关键字提供了一种灵活的方式来控制类的访问权限，但也应谨慎使用，以避免不必要的复杂性和耦合。
> 
> ```cpp
> namespace foo{
>     class foo{
>         //...
>     }
>     void processFoo(Foo& foo);
> }
> ```
 
> 在 C++ 中，可以将同一个类的 convenience functions（便利函数）分散到不同的头文件中，并用命名空间进行封装。这种做法可以提高代码的组织性和可维护性，当修改某一部分的代码时可以不影响其他部分的编译情况，对用户更友好。以下是一个示例：
> 
> ### 示例类定义
> ```cpp
> // MyClass.h
> //声明所有用户都需要用到的核心函数，所有封装都需要用这个头文件
> #ifndef MYCLASS_H
> #define MYCLASS_H
> 
> class MyClass {
> public:
>     MyClass(int value);
>     int getValue() const;
> 
> private:
>     int value;
> };
> 
> #endif // MYCLASS_H
> ```
> 
> ### 头文件1：Utility1.h
> ```cpp
> // Utility1.h
> #ifndef UTILITY1_H
> #define UTILITY1_H
> 
> #include "MyClass.h"
> 
> namespace Utility {
>     void printValue(const MyClass& obj);
> }
> 
> #endif // UTILITY1_H
> ```
> 
> ### 头文件2：Utility2.h
> ```cpp
> // Utility2.h
> #ifndef UTILITY2_H
> #define UTILITY2_H
> 
> #include "MyClass.h"
> 
> namespace Utility {
>     MyClass doubleValue(const MyClass& obj);
> }
> 
> #endif // UTILITY2_H
> ```
> 
> ### 便利函数实现
> 
> #### Utility1.cpp
> ```cpp
> // Utility1.cpp
> #include "Utility1.h"
> #include <iostream>
> 
> namespace Utility {
>     void printValue(const MyClass& obj) {
>         std::cout << "Value: " << obj.getValue() << std::endl;
>     }
> }
> ```
> 
> #### Utility2.cpp
> ```cpp
> // Utility2.cpp
> #include "Utility2.h"
> 
> namespace Utility {
>     MyClass doubleValue(const MyClass& obj) {
>         return MyClass(obj.getValue() * 2);
>     }
> }
> ```
> 
> ### 主程序
> ```cpp
> // main.cpp
> #include "MyClass.h"
> #include "Utility1.h"
> #include "Utility2.h"
> 
> int main() {
>     MyClass obj(10);
>     Utility::printValue(obj);
> 
>     MyClass doubledObj = Utility::doubleValue(obj);
>     Utility::printValue(doubledObj);
> 
>     return 0;
> }
> ```
> 
 
> ### 说明
> 1. **类定义（MyClass）**：类的基本定义放在一个头文件中，以确保类的声明可以被其他文件引用。
> 2. **便利函数**：将不同功能的便利函数放在不同的头文件和源文件中，并使用**同一个命名空间**进行封装，提高了封装性
> 3. **主程序**：在主程序中根据需要包含相关的头文件，并使用命名空间调用便利函数。
> 
> 这种结构可以使代码更加模块化，便于管理和维护。如果类的便利函数越来越多，可以继续添加新的头文件和命名空间，以保持代码的清晰和有序。
> 实际上标准c++库就是这么组织的，不同的头文件（<vector>，<algorithm>等）在同一个命名空间 std 中声明不同的函数，使用 vector 的人不需要 #include <algorithm>，这样使得编译不会互相依赖，提升用户体验，也能提高封装性；同时用户也能够方便地扩展命名空间中的函数


## Item 24: Declare non-member functions when type conversions should apply to all parameter


```cpp
class Rational{
//...
    const Rational operator*(const Rational& rhs) const;
};

Rational oneHalf(1,2);

Rational res=oneHalf*2; //fine, implicit type conversion

Rational res=2*oneHalf //error

//fine if 2 => Rational, must explicit type conversion

//parameters are eligible for implicit type conversion only if they are listed in the parameter list

//Rational res=Rational(2).operator*(oneHalf);

//also fine if declare non-member function and type conversions, perform type conversion on all arguments in the parameter list
```

## Item 25: Consider support for a non-throwing swap

> 在C++中，模板特化（Template Specialization）是一种允许为特定类型或特定情况提供专门实现的机制。模板特化有两种主要形式：**完全特化**和**部分特化**。以下是这两种特化的详细解释和示例：
> 
> ### 1. 完全特化（Full Specialization）
> 完全特化指为特定的类型提供一个完全独立于模板参数的实现。
> 
> ```cpp
> #include <iostream>
> 
> // 通用模板
> template <typename T>
> class MyClass {
> public:
>     void display() {
>         std::cout << "Generic template" << std::endl;
>     }
> };
> 
> // 特化版本（针对 int 类型）
> template <>
> class MyClass<int> {
> public:
>     void display() {
>         std::cout << "Specialized template for int" << std::endl;
>     }
> };
> 
> int main() {
>     MyClass<double> obj1;
>     obj1.display(); // 输出：Generic template
> 
>     MyClass<int> obj2;
>     obj2.display(); // 输出：Specialized template for int
> 
>     return 0;
> }
> ```
> 
> ### 2. 部分特化（Partial Specialization）
> 部分特化允许为模板参数的一部分提供特化实现。部分特化通常用于类模板，不能用于函数模板。
> 
> ```cpp
> #include <iostream>
> 
> // 通用模板
> template <typename T1, typename T2>
> class MyClass {
> public:
>     void display() {
>         std::cout << "Generic template" << std::endl;
>     }
> };
> 
> // 部分特化版本（针对第二个参数是 int 类型）
> template <typename T1>
> class MyClass<T1, int> {
> public:
>     void display() {
>         std::cout << "Partial specialization where second type is int" << std::endl;
>     }
> };
> 
> int main() {
>     MyClass<double, double> obj1;
>     obj1.display(); // 输出：Generic template
> 
>     MyClass<double, int> obj2;
>     obj2.display(); // 输出：Partial specialization where second type is int
> 
>     return 0;
> }
> ```
> 
> ### 函数模板特化
> 函数模板不支持部分特化，但可以进行完全特化。一般情况下，当需要函数偏特化的时候只需要进行函数重载即可
> 
> ```cpp
> #include <iostream>
> 
> // 通用函数模板
> template <typename T>
> void func(T t) {
>     std::cout << "Generic function template" << std::endl;
> }
> 
> // 特化版本（针对 int 类型）
> template <>
> void func<int>(int t) {
>     std::cout << "Specialized function template for int" << std::endl;
> }
> 
> int main() {
>     func(10.5); // 输出：Generic function template
>     func(10);   // 输出：Specialized function template for int
> 
>     return 0;
> }
> ```
> 
> 模板特化是C++中强大而灵活的特性，允许程序员为特定的类型或情况提供优化和定制的实现。实际应用中不允许修改标准命名空间中定义的内容，但允许进行标准模板的特化，例如为自己定义的类完全特化 swap 

All of STL containers provide both public swap member function and version of std::swap that call these member functions.

> Argument-Dependent Lookup (ADL)，也称为Koenig Lookup，是C++中的一种名称解析机制。它允许在调用函数时，根据参数的类型和命名空间来查找函数，而不仅仅依赖于当前作用域。这种机制在处理模板和运算符重载时特别有用。
> 
> 具体来说，ADL的工作原理如下：
> 
> 1. **函数调用的参数类型**：当你调用一个函数时，编译器会查看参数的类型。
> 2. **命名空间查找**：编译器会在这些参数类型所属的命名空间中查找合适的函数。
> 
> 举个例子：
> 
> ```cpp
> #include <iostream>
> 
> namespace MyNamespace {
>     class MyClass {};
> 
>     void myFunction(MyClass) {
>         std::cout << "Function in MyNamespace" << std::endl;
>     }
> }
> 
> int main() {
>     MyNamespace::MyClass obj;
>     myFunction(obj); // 这里的函数调用将通过ADL机制查找到MyNamespace::myFunction
>     return 0;
> }
> ```
> 
> 在这个例子中，`myFunction`在`main`函数中被调用，但没有显式地指定命名空间。然而，由于参数`obj`的类型`MyNamespace::MyClass`属于`MyNamespace`命名空间，编译器通过ADL机制找到并调用了`MyNamespace::myFunction`。
> 
> 这种机制的好处在于，它使代码更加简洁，并且在模板编程和运算符重载时能够正确地解析函数调用。例如，标准库中的运算符重载（如`operator<<`）经常利用ADL来查找适当的函数。
> 
> 需要注意的是，ADL仅适用于非成员函数和友元函数，对成员函数不适用。
>
> 在C++中，命名空间用于组织代码并避免命名冲突。假设在命名空间中声明了一个类和该类的特化模板函数，并且在全局命名空间中也有一个同名的模板函数，那么在另一个文件中调用该类的模板函数时，是否会调用命名空间中的模板函数取决于几个因素，如命名空间的使用和调用方式。
> 
> 考虑以下代码示例：
> 
> ### 文件1：namespace_example.h
> 
> ```cpp
> namespace MyNamespace {
>     class MyClass {
>     public:
>         void doSomething() {
>             // ...
>         }
>     };
> 
>     template<typename T>
>     void myFunction(T t) {
>     }
> 
>     template<>
>     void myFunction(MyClass t) {
>         t.doSomething();
>     }
> }
> 
> // 全局命名空间中的模板函数
> template<typename T>
> void myFunction(T t) {
>     // ...
> }
> ```
> 
> ### 文件2：main.cpp
> 
> ```cpp
> #include "namespace_example.h"
> 
> int main() {
>     MyClass obj;
>     myFunction(obj); // 调用哪个 myFunction?
>     return 0;
> }
> ```
> 
> ### 分析：
> 
> 1. **命名空间与全局命名空间的区分**：在 `MyNamespace` 命名空间中，有一个特化的 `myFunction` 函数，而在全局命名空间中，有一个通用的 `myFunction` 函数。
> 
> 2. **未使用命名空间限定符**：在 `main.cpp` 中调用 `myFunction(obj)` 时，没有使用命名空间限定符。（**这点很重要，这样允许编译器寻找最适合的函数，但是有些情况下使用了限定命名空间的std::myFunction就会导致没有合适的函数，因此要求尽量在std中为自己的类全特化所需函数**）
> 
> 3. **ADL（Argument-Dependent Lookup）**：C++ 中的 ADL 规则会根据函数参数的类型来决定从哪个命名空间查找函数。在这个例子中，`obj` 是 `MyClass` 类型的实例，而 `MyClass` 是在 `MyNamespace` 命名空间中定义的。因此，编译器会在 `MyNamespace` 中查找 `myFunction`。
> 
> 因此，在不使用命名空间限定符的情况下调用 `myFunction(obj)` 时，编译器会根据 ADL 规则在 `MyNamespace` 中查找 `myFunction`，并调用特化的模板函数。
> 
> ### 结论：
> 
> 在另一个文件中，不声明命名空间，调用类的模板函数时会调用命名空间中的模板函数，而不是全局命名空间中的同名函数。这是因为 ADL 规则会根据参数类型查找适当的函数定义。


- Provide a **swap member function** when std::swap would be inefficient of your type. **Make sure your swap doesn't throw exceptions.**

在C++中实现swap函数时可能会抛出异常，这对使用copy-and-swap技术有一定的影响。让我们详细讨论一下这个问题。

> ### copy-and-swap技术简介
> copy-and-swap是一种常用的C++编程技术，主要用于实现异常安全的赋值运算符。其基本思想是：
> 
> 1. 先创建一个临时副本。
> 2. 使用这个副本进行所需的操作（如赋值）。
> 3. 交换临时副本和当前对象的资源。
> 
> 这样做的好处是，如果任何操作（如复制构造、赋值、交换）抛出异常，当前对象保持不变，不会处于部分修改的状态。
> 
> ### swap函数的异常抛出
> 通常，swap函数的实现应该保证不抛出异常（`noexcept`），因为一旦swap函数抛出异常，整个copy-and-swap技术就无法保证其异常安全性。例如，标准库中的`std::swap`函数是被标记为`noexcept`的。
> 
> ```cpp
> namespace std {
>     template <class T>
>     void swap(T& a, T& b) noexcept(/* see below */);
> }
> ```
> 
> 但是，如果你自己实现的swap函数可能会抛出异常，那么在copy-and-swap技术中使用它就会导致问题：
> 
> 1. **异常传播**：如果swap函数在交换过程中抛出异常，那么就会导致原始对象和临时对象处于未定义的状态。
> 2. **异常安全性**：copy-and-swap技术的一个核心是通过交换来保证异常安全性，如果swap函数抛出异常，异常安全性将无法得到保证。
> 
> ### 如何处理
> 为了确保copy-and-swap技术的安全性，你需要确保你的swap函数是`noexcept`的。你可以使用`noexcept`关键字来显式声明你的swap函数不抛出异常：
> 
> ```cpp
> void swap(MyClass& lhs, MyClass& rhs) noexcept {
>     // 交换成员
>     std::swap(lhs.member1, rhs.member1); // 假设这些成员的swap操作也是noexcept的
>     std::swap(lhs.member2, rhs.member2);
>     // ...
> }
> ```
> 
> 同时，你可以在类的赋值运算符中利用这个`noexcept`的swap函数：
> 
> ```cpp
> class MyClass {
> public:
>     MyClass& operator=(MyClass rhs) {
>         swap(*this, rhs); // 使用noexcept的swap函数
>         return *this;
>     }
> 
>     friend void swap(MyClass& lhs, MyClass& rhs) noexcept;
> };
> ```
> 
> 通过确保swap函数的`noexcept`，你可以保证copy-and-swap技术的异常安全性，从而提高程序的稳定性和健壮性。


- if you offer a member swap, also offer a **non-member swap** that calls the member. For classes (not templates), specialize **std::swap** too. 
- When calling swap, employ a using declaration for std::swap, then call swap without namespace qualification

> 如果有命名空间量化的话，就要把std::swap的特化也实现，因为一般量化命名空间都用的是std的，这样 ADL 就会只在 std 中找合适的swap，特化自定义类的 swap 使得调用 std::swap 更高效。如果不量化命名空间的话，ADL 会自动寻找自定义类命名空间中的特化swap,不会盲目调用std::swap。能特化 std::swap 的时候最好特化。当自定义类是模板类的时候因为不能特化 std::swap，如果要使用自定义类命名空间中的高效 swap，不能加 std:: 限定命名空间，让 ADL 寻找最合适的 swap。

> 总结：能实现特化尽量全实现特化，不能实现 std 特化（用的是类模板的情况）的时候，调用 swap 的时候不要指定 std:: 限定命名空间，ADL 会去自定义类所在的命名空间找非成员函数的swap

- It's fine to totally specialize std templates for user-defined types, but never try to add something compeletely new to std

![swap](swap1.png)
![swap](swap2.png)

# 5. Implementations

## Item 26: Postpone variable definition as long as possible

> You should postpone the definition until you must use it and you have initialization arguments for it. By doing so, you avoid constructing and destructing unneeded objects, and you avoid unnecessary default ctor. 
> 
> 
> 在C++中，循环中需要使用的变量定义在循环外面和定义在循环内部，两种方法在耗时上的差异通常可以忽略不计，但在某些特定情况下，可能会有一些性能差异。
> 
> 1. **定义在循环外面**：
>    ```cpp
>    Widget w;
>    for (int i = 0; i < 100; ++i) {
>        // 使用变量w
>        w=value;
>    }
>    ```
>    在这种情况下，耗时是一次 ctor 和一次 dtor，n 次 赋值
> 
> 2. **定义在循环内部**：
>    ```cpp
>    for (int i = 0; i < 100; ++i) {
>        Widget w(value);
>    }
>    ```
>    在这种情况下，耗时是 n 次 ctor 和 n 次  dotr，通常将这种重新声明将优化为在循环开始时分配内存，因此在大多数情况下性能差异不大。
> 
> ### 性能差异的详细说明
> 
> - **编译器优化**：现代C++编译器（如GCC, Clang, MSVC等）都会对代码进行优化。在开启优化选项（如 `-O2` 或 `-O3`）时，编译器通常会将循环内部定义的变量优化为在循环外部定义的变量，因此两种方法在大多数情况下性能差异很小。
> 
> - **变量作用域**：如果变量只在循环内部使用，将其定义在循环内部可以减少其作用域，增加代码的可读性和可维护性。对于性能影响来说，这种局部变量定义方式在大多数现代编译器的优化下，不会带来明显的性能劣化。
> 
> - **微小开销**：理论上，在每次迭代中重新定义变量可能会带来一点点开销，但由于编译器的优化，这种开销通常是微乎其微的。只有在极其苛刻的性能需求下，才可能需要考虑这种微小的差异。
> 
> ### 总结
> 在大多数情况下，两种方法的性能差异是可以忽略的。更重要的是代码的可读性和维护性。**建议在实际编写代码时，更倾向于使用作用域最小的变量定义方式，即在循环内部定义只在循环中使用的变量，只在循环内部使用的变量，尽量在循环内部定义，这样使得代码更加清晰，也更加容易维护**


## Item 27: Minimize casting
- When casting is necessary, try to hide it inside a function. Clients can then call the function instead of putting casts in  thier own code.
- Prefer C++-style casts to old-style casts. They are easier to see, and they are more specific about what they do.
The rules of C++ are designed to guarantee that type errors are impossible, but casts subvert the type system.

C-style casts, old style:

```cpp
(T) expression

T(expression)

```

C++-style casts, new style:

```cpp

const_cast<T>(expression)
dynamic_cast<T>(expression)
reinterpret_cast<T>(expression)
static_cast<T>(expression)
```
> 一个好的c++程序应该尽量少用cast
> C++中有四种类型的类型转换（cast），分别是：`static_cast`、`dynamic_cast`、`const_cast`和`reinterpret_cast`。每种类型转换都有其特定的用途和适用场景。以下是每种类型转换的详细介绍：
> 
> ### 1. `static_cast`
> 
> `static_cast`用于在相关类型之间进行转换，主要用于编译时类型检查。这种转换是安全的，但它不会进行运行时类型检查。**会生成临时对象存储数据，注意可能会导致函数调用的时候是对临时对象作用的，而不是原本待转换类型的对象**
> 
> - 用途：
>   - 基类和派生类之间的转换（需要类型安全）。
>   - 基本数据类型之间的转换（如整数到浮点数）。
>   - void*和其他指针类型之间的转换。
> 
> - 示例：
>   ```cpp
>   class Base {};
>   class Derived : public Base {};
> 
>   Base* basePtr = new Derived;
>   Derived* derivedPtr = static_cast<Derived*>(basePtr);
>   ```
> 
> ### 2. `dynamic_cast`
> 
> `dynamic_cast`主要用于多态类型之间的转换。它执行运行时类型检查，确保转换的安全性。如果转换失败，将返回`nullptr`（对于指针）或抛出`std::bad_cast`异常（对于引用）。此cast非常耗时，例如至少要做的就是类名称的比对，如果涉及到n层继承，就需要比对n次类名的string，非常耗时，能用其他方式完成所需目标，尽量别用这个cast，非常耗时；另外如果继承关系发生改变或新增，也会重新产生更多臃肿的代码
> 
> - 用途：
>   - 用于在继承层次中向下转换（downcast），确保转换对象是目标类型的实例。
>   - 需要在类中至少有一个虚函数（通常是虚析构函数）。
> 
> - 示例：
>   ```cpp
>   class Base {
>     virtual ~Base() {}
>   };
>   class Derived : public Base {};
> 
>   Base* basePtr = new Derived;
>   Derived* derivedPtr = dynamic_cast<Derived*>(basePtr);
>   if (derivedPtr) {
>     // 转换成功
>   } else {
>     // 转换失败
>   }
>   ```
> 
> ### 3. `const_cast`
> 
> `const_cast`用于添加或移除`const`或`volatile`属性。它不用于类型之间的转换，只用于修饰符的调整。
> 
> - 用途：
>   - 移除`const`属性以修改原本为常量的变量。
>   - 添加`const`属性以将变量设为只读。
> 
> - 示例：
>   ```cpp
>   void func(const int* p) {
>     int* modifiable = const_cast<int*>(p);
>     *modifiable = 10;
>   }
>   ```
> 
> ### 4. `reinterpret_cast`
> 
> `reinterpret_cast`用于将指针或引用转换为其他类型的指针或引用，几乎不进行类型检查。它用于低级别的、平台相关的代码中。
> 
> - 用途：
>   - 将一个指针类型转换为不同的指针类型。
>   - 在指针和整数之间进行转换。
>   - 进行位模式重新解释。
> 
> - 示例：
>   ```cpp
>   int i = 42;
>   void* ptr = &i;
>   int* intPtr = reinterpret_cast<int*>(ptr);
>   ```
> 
> ### 总结
> 
> 这四种类型转换各自有特定的应用场景和使用方法。正确使用这些类型转换可以提高代码的可读性和安全性，同时避免不必要的类型错误和运行时错误。
 
> C++中的四种类型转换（`static_cast`、`dynamic_cast`、`const_cast`和`reinterpret_cast`）在编译时的行为各不相同，有些会生成额外的代码，有些则不会。以下是每种类型转换在编译时可能的行为：
> 
> ### 1. `static_cast`
> **实际上生成了一个拷贝对象**
> `static_cast`主要在编译时进行类型检查和转换。它告诉编译器将一个对象看作另一种类型，但不会执行额外的运行时检查。对于基本数据类型之间的转换，编译器可能会生成必要的代码以实现转换（例如从整数到浮点数，因为他们的底层表示并不一样，不能直接转换视角看待）。对于指针和引用之间的转换，通常不会生成额外的代码。
> 
> - **示例**：
>   ```cpp
>   float f = 3.14;
>   int i = static_cast<int>(f);  // 可能生成代码以实现浮点数到整数的转换，生成了另一个暂时的拷贝int对象，存储数值之后赋值给i
>   ```
> 
> ### 2. `dynamic_cast`
> 
> `dynamic_cast`会在运行时执行类型检查，确保转换的安全性。它通常用于多态类型之间的转换，需要运行时类型信息（RTTI）。因此，使用`dynamic_cast`时，编译器会生成额外的代码来执行这些检查。
> 
> - **示例**：
>   ```cpp
>   Base* basePtr = new Derived;
>   Derived* derivedPtr = dynamic_cast<Derived*>(basePtr);  // 生成运行时类型检查代码
>   ```
> 
> ### 3. `const_cast`
> 
> `const_cast`用于添加或移除`const`或`volatile`属性，不会在运行时生成额外的代码。它仅仅是告诉编译器忽略这些修饰符，因此不会对生成的机器代码产生影响。
> 
> - **示例**：
>   ```cpp
>   const int* p = &i;
>   int* modifiable = const_cast<int*>(p);  // 不生成额外的代码
>   ```
> 
> ### 4. `reinterpret_cast`
> 
> `reinterpret_cast`用于在不同的指针或引用类型之间进行转换。它通常不会生成额外的代码，只是改变了编译器对数据的解释方式。对于指针和整数之间的转换，编译器可能会生成一些代码，但这取决于具体的转换和平台。
> 
> - **示例**：
>   ```cpp
>   int i = 42;
>   void* ptr = &i;
>   int* intPtr = reinterpret_cast<int*>(ptr);  // 不生成额外的代码
>   ```
> 
> ### 总结
> 
> - `static_cast`：编译时类型检查和转换，可能生成代码（特别是基本类型之间的转换）。
> - `dynamic_cast`：运行时类型检查，通常会生成额外的代码。
> - `const_cast`：仅在编译时生效，不生成额外的代码。
> - `reinterpret_cast`：改变编译器对数据的解释方式，通常不生成额外的代码。
> 
> 每种类型转换的主要目的是在编译时或运行时确保类型安全和正确性，根据具体使用场景选择合适的类型转换非常重要。


## Item 28: Avoid returning "handles" to object internals

Avoid returning handles (references, pointers, or iterators) to object internals. Not returning handles increases encapsulation, helps const member functions act const, and Minimizes the creation of dangling handles.

## Item 29: Strive for exception-safe code

When an exception is thrown,  exception-safe functions:
- Leak no resource
- Don't allow data structures to become corrupted

> 在C++中，exception-safe函数提供三种类型的保证，分别是：
> 
> 1. **基本保证（Basic Guarantee）**：
>    - 当异常发生时，程序的所有不变性（invariants）依然保持，并且没有资源泄漏。
>    - 尽管程序的状态可能不确定，但它依然是有效的，可以继续执行。
>    - 这是异常安全性的最低要求。
> 
> 2. **强烈保证（Strong Guarantee）**：
>    - 当异常发生时，程序的状态会回滚到调用函数之前的状态，就好像函数调用从未发生过。
>    - 这意味着要么函数成功并正常执行，要么异常发生且程序状态没有变化。
>    - 强烈保证通常通过事务性操作（transactional operation）来实现，类似于数据库中的回滚机制。
> 
> 3. **无异常保证（No-Throw Guarantee 或 Nothrow Guarantee）**：
>    - 函数承诺不会抛出任何异常。
>    - 这是最强的保证，通常用于析构函数、内存管理函数等关键函数。
>    - 要实现这种保证，需要确保函数内部的所有操作都是不抛出异常的。
> 
> 这三种保证有助于在C++中编写健壮和可靠的代码，特别是在处理资源管理和异常处理时。
 
> 在 C++ 中，函数声明中的异常规范（exception specification）用于指明函数可能抛出的异常类型。
> 
> 在 C++11 之前，有一种称为“空异常规范”（empty exception specification）的语法形式，用于声明函数不抛出任何异常。具体语法是：
> 
> ```cpp
> void foo() throw();
> ```
> 
> 这意味着 `foo` 函数不应该抛出任何异常。如果这个函数确实抛出了异常，程序会调用 `std::unexpected` 函数，这通常会导致程序终止。这种情况并不是指函数一定不抛出异常，而是指如果抛出异常，那将是非常严重的错误，并不是指实现上一定不会抛出异常
> 
> 从 C++11 开始，标准引入了 `noexcept` 关键字来取代这种语法。 `noexcept` 关键字可以用于指示一个函数是否会抛出异常：
> 
> ```cpp
> void foo() noexcept;
> ```
> 
> 与 `throw()` 类似，`noexcept` 也表示函数不会抛出任何异常。使用 `noexcept` 的好处在于它不仅更简洁，还可以参与到更多的优化中，使编译器能更好地优化代码。
> 
> 总之，“空异常规范”指的是在函数声明中显式指明该函数不会抛出任何异常。在 C++11 之前使用 `throw()`，而在 C++11 及之后建议使用 `noexcept`。


> 在 C++ 中，使用 "copy and swap" 技术是一种常见的方法来提供强异常保证（strong exception guarantee）。强异常保证意味着：即使在出现异常时，对象的状态也会保持不变，make all-or-nothin。实现这一技术通常包括以下步骤：
> 
> - 复制构造函数：创建一个将要修改对象的临时拷贝对象，所有的修改都针对于这个临时拷贝对象，而不是原对象
> - 交换操作：用临时对象与当前对象交换数据。
> - 提供异常安全：通过交换操作，如果在操作过程中发生异常，当前对象的状态依然保持不变，因为只有当所有操作成功后，交换才会进行。
> 
> 实现这种方法一般会用到 pimpl idiom 技术
> 
> Pimpl（Pointer to IMPLementation）idiom 是一种用于 C++ 编程的设计模式。它通过将类的实现细节隐藏在实现文件（.cpp 文件）中，只在头文件（.h 文件）中保留接口声明，从而达到减少编译依赖、加快编译速度和隐藏实现细节的目的，**主要通过的就是将用户要使用的类与这个类的成员类分离，修改成员类不会导致用户使用的类重新编译（因为只有一个指针成员，指针的存储空间大小是固定的，编译器知道如何分配空间，不需要访问实现文件），只需要实现文件重新编译，而用户用的接口文件不用重新编译**。以下是对 Pimpl idiom 的详细解释：
> 
> ### 原理
> Pimpl idiom 的核心思想是将类的具体实现封装在一个独立的实现类中，通过一个指向实现类的指针来访问实现类的成员。这样可以将实现类的定义从头文件中移到实现文件中，从而隐藏实现细节。
> 
> ### 实现步骤
> 1. **在头文件中声明接口类**：
>     - 声明一个包含指向实现类指针的类。
>     - 声明接口函数，但不提供实现。
> 
> 2. **在实现文件中定义实现类**：
>     - 定义实现类，并在接口类的构造函数中实例化它。
> 
> 3. **在接口函数中调用实现类的相应函数**：
>     - 在接口函数中，通过指向实现类的指针调用相应的实现函数。
> 
> ### 示例代码
> 
> #### 头文件 (MyClass.h)
> ```cpp
> // 头文件
> #ifndef MYCLASS_H
> #define MYCLASS_H
> 
> #include <memory> // 用于 std::unique_ptr
> 
> class MyClass {
> public:
>     MyClass(); // 构造函数
>     ~MyClass(); // 析构函数
> 
>     void doSomething(); // 接口函数
> 
> private:
>     class Impl; // 前向声明实现类
>     std::unique_ptr<Impl> pImpl; // 指向实现类的指针
> };
> 
> #endif // MYCLASS_H
> ```
> 
> #### 实现文件 (MyClass.cpp)
> ```cpp
> // 实现文件
> #include "MyClass.h"
> #include <iostream>
> 
> class MyClass::Impl {
> public:
>     void doSomethingImpl() {
>         std::cout << "Doing something!" << std::endl;
>     }
> };
> 
> MyClass::MyClass() : pImpl(std::make_unique<Impl>()) {}
> 
> MyClass::~MyClass() = default;
> 
> void MyClass::doSomething() {
>     pImpl->doSomethingImpl();
> }
> ```
> 
> ### 优点
> 1. **减少编译依赖**：头文件中的更改不会导致依赖该头文件的其他文件重新编译，因为实现细节在实现文件中。
> 2. **隐藏实现细节**：实现类的定义和实现完全隐藏在实现文件中，用户无法访问。
> 3. **稳定接口**：接口类的接口相对稳定，即使实现发生改变，接口也不变。
> 
> ### 缺点
> 1. **引入间接层**：通过指针访问实现类的成员会引入一定的运行时开销。
> 2. **复杂性增加**：代码结构变得更加复杂，增加了维护成本。
> 
> Pimpl idiom 是一种有效的设计模式，特别适用于需要频繁修改实现细节但希望保持接口稳定的场景。

A fuction can usually offer a guarantee no stronger than the weakest guarantee of the fuctions it calls.

## Item 30: Understand the ins and outs of inlining

> 在C++编译器处理中，内联函数（inline functions）的优化主要体现在以下几个方面，某些优化仅针对内联函数才会实现，其他函数不会有这样的优化，但需要注意的是内联函数很少支持debug，这可能会给代码编写带来麻烦：
> 
> 1. **内联展开（Inline Expansion）**：
>    - 编译器会将内联函数的代码直接插入到每个调用点，而不是生成函数调用。这消除了函数调用的开销，包括参数传递和返回值处理的开销。
> 
> 2. **减少函数调用开销**：
>    - 函数调用通常涉及栈操作（如保存返回地址、传递参数、保存和恢复寄存器等），内联函数通过直接插入代码消除了这些开销，提高了性能。
> 
> 3. **常量传播（Constant Propagation）**：
>    - 如果内联函数中使用了常量参数，编译器可以在展开内联函数时进行常量传播，从而优化内联函数内部的代码。
> 
> 4. **消除代码冗余（Code Elimination）**：
>    - 通过内联展开，编译器可以更好地进行死代码消除和冗余代码删除等优化。这是因为内联展开后，编译器可以更全面地分析代码上下文。
> 
> 5. **跨函数优化（Inter-Procedural Optimization）**：
>    - 内联函数的展开使得跨函数优化变得更容易进行，例如循环展开、指令重排等优化可以在更大范围内进行。
> 
> 6. **减少函数调用延迟**：
>    - 对于小函数，内联展开可以显著减少由于函数调用引起的延迟，特别是在性能关键的代码路径上。
> 
> 需要注意的是，内联函数并不总是带来性能提升。对于较大的函数，内联展开可能导致代码膨胀（code bloat），从而对性能产生负面影响。此外，编译器并不一定会内联标记为`inline`的函数，尤其是在函数较大或者递归调用的情况下，编译器可能会忽略内联建议以避免代码膨胀和递归展开带来的问题。最终是否进行内联展开，通常取决于编译器的优化策略和启用的优化选项。


> 在C++中，内联函数确实通常声明和定义在头文件中。这是因为内联函数需要在编译时内联展开（即替换函数调用为函数体），而编译器在编译每个源文件时需要知道内联函数的定义。
> 
> 以下是一些原因和详细解释：
> 
> 1. **内联展开**：内联函数的目的是在编译时将函数调用展开为函数体，以减少函数调用的开销。为了做到这一点，编译器在每个调用内联函数的源文件中都需要看到内联函数的定义。
> 
> 2. **多重定义**：如果内联函数的定义只在一个源文件中，而在其他源文件中只进行声明，编译器无法在其他源文件中进行内联展开。将内联函数的定义放在头文件中，可以确保所有包含该头文件的源文件都能看到函数定义，从而实现内联展开。
> 
> 3. **链接器优化**：内联函数的定义放在头文件中，也有助于链接器进行优化。链接器可以更好地优化和消除不必要的函数调用，从而生成更高效的代码。
> 
> 例如：
> 
> ```cpp
> // my_inline_functions.h
> inline int add(int a, int b) {
>     return a + b;
> }
> ```
> 
> ```cpp
> // main.cpp
> #include "my_inline_functions.h"
> 
> int main() {
>     int result = add(3, 4);
>     return 0;
> }
> ```
> 
> 在上面的例子中，`add` 函数是一个内联函数，其定义放在头文件 `my_inline_functions.h` 中。这样，任何包含该头文件的源文件都可以对 `add` 函数进行内联展开。
> 
> 不过，值得注意的是，虽然通常情况下内联函数定义在头文件中，但有时也可以在源文件中定义内联函数，只要确保在所有使用该函数的源文件中包含相应的头文件声明。例如，可以使用 `static` 或 `extern inline` 来控制内联函数的链接行为。

> 内联函数的函数体**必须声明在头文件**中，是因为编译器需要在每个使用该函数的翻译单元中都能看到函数的定义，以便在调用点展开内联代码。
> 
> 具体来说，内联函数的定义和调用的关系如下：
> 
> - 内联展开（Inline Expansion）：内联函数是一种优化手段，编译器会在调用点直接插入函数体，而不是生成函数调用指令。这样可以减少函数调用的开销，尤其是对于小函数。这需要在编译时就能看到函数的定义。
> 
> - 多个翻译单元：C++程序通常由多个翻译单元（源文件）组成，每个翻译单元独立编译。在每个翻译单元中都可能调用内联函数。为了使编译器能够在每个翻译单元中展开内联函数，函数的定义必须对所有使用它的翻译单元可见。
> 
> - 头文件的作用：头文件通常包含函数声明和内联函数定义，并被多个源文件包含。这样，所有包含这个头文件的源文件（翻译单元）都能看到内联函数的定义，从而允许编译器在这些源文件中展开内联函数。
> 
> 如果将内联函数的定义放在源文件中，则只有包含该源文件的翻译单元才能看到函数定义，其他翻译单元将无法内联该函数，这违反了内联函数的初衷。


> 是的，当构造函数抛出异常时，任何已经成功构造的成员对象或基类子对象的析构函数会被自动调用。C++ 的这种行为确保了在异常情况下，不会有资源泄漏，维持了对象的整体一致性。
> 
> 具体来说：
> 
> 1. **成员对象**：如果一个类的构造函数在构造过程中抛出异常，所有已经成功构造的成员对象的析构函数都会被调用。未成功构造的成员对象的析构函数不会被调用，因为它们根本就没有构造成功。
> 
> 2. **基类对象**：如果派生类的构造函数抛出异常，任何已经成功构造的基类子对象的析构函数也会被调用。
> 
> 例如：
> 
> ```cpp
> #include <iostream>
> #include <stdexcept>
> 
> class Member {
> public:
>     Member() {
>         std::cout << "Member constructed\n";
>     }
>     ~Member() {
>         std::cout << "Member destructed\n";
>     }
> };
> 
> class MyClass {
>     Member m;
> public:
>     MyClass() {
>         std::cout << "MyClass constructor\n";
>         throw std::runtime_error("Constructor exception");
>     }
>     ~MyClass() {
>         std::cout << "MyClass destructor\n";
>     }
> };
> 
> int main() {
>     try {
>         MyClass obj;
>     } catch (const std::exception& e) {
>         std::cout << "Caught exception: " << e.what() << "\n";
>     }
>     return 0;
> }
> ```
> 
> 在这个例子中，当 `MyClass` 的构造函数抛出异常时，已经构造的 `Member` 对象的析构函数会被调用。
> 
> 运行结果是：
> 
> ```
> Member constructed
> MyClass constructor
> Member destructed
> Caught exception: Constructor exception
> ```
> 
> 从输出可以看出，即使 `MyClass` 的构造函数抛出异常，已经构造的 `Member` 对象的析构函数仍然被正确调用。这展示了 C++ 语言在异常情况下自动进行清理的机制。

## Item 31: Minimize compilation dependencies between files

> 在C++中，**前向声明（forward declaration）** 是一种在实际定义之前声明类、函数或变量的方法。前向声明通常用于解决循环依赖问题或者减少编译时间。它告诉编译器某个名字是存在的，但暂时不提供完整的定义。这种声明方式非常有用，特别是在涉及到多个头文件和类之间互相引用的情况下。
> 
> ### 类的前向声明
> 假设你有两个类 `A` 和 `B`，它们互相引用对方：
> 
> ```cpp
> // Forward declaration of class B
> class B;
> 
> class A {
>     B* b;  // Pointer to an instance of class B
>     // Other members and methods
> };
> 
> class B {
>     A* a;  // Pointer to an instance of class A
>     // Other members and methods
> };
> ```
> 
> 在这个例子中，`class B;` 是类 `B` 的前向声明，使得 `A` 可以声明一个指向 `B` 类型的指针。在实际使用 `B` 类型的成员或方法时，必须有 `B` 的完整定义。
> 
> ### 函数的前向声明
> 函数前向声明允许在使用函数之前声明它，以便编译器知道它的存在。例如：
> 
> ```cpp
> // Forward declaration of function foo
> void foo(int, double);
> 
> int main() {
>     foo(42, 3.14);  // OK: foo has been forward declared
>     return 0;
> }
> 
> void foo(int x, double y) {
>     // Function definition
> }
> ```
> 
> 在这个例子中，`void foo(int, double);` 是函数 `foo` 的前向声明，使得在 `main` 函数中可以调用 `foo`，即使 `foo` 的定义在后面。
> 
> ### 前向声明的限制
> 1. **类的前向声明**：只能声明指针或引用，不能声明实际对象或调用未定义的方法。
> 2. **函数的前向声明**：参数类型和返回类型必须匹配，不能使用默认参数。
> 
> ### 使用前向声明的好处
> 1. **解决循环依赖**：当两个类互相引用时，通过前向声明可以避免无限递归包含头文件的问题。
> 2. **减少编译时间**：前向声明减少了头文件的包含次数，从而减少编译时间。
> 
> 总的来说，前向声明是C++编程中一个重要的技巧，可以帮助你管理复杂的代码依赖关系并优化编译过程。

> 在C++中，声明、定义和实现是编程中常用的概念，它们有不同的作用和使用场景。下面详细说明每个概念及其区别：
> 
> ### 声明（Declaration）
> 
> 声明用于告诉编译器某个名字（如变量、函数或类）存在，并指定其类型或签名，但不提供完整的信息或存储空间。例如：
> 
> - **变量声明：**
> 
>   ```cpp
>   extern int a; // 声明变量 a，但不定义
>   ```
> 
>   `extern` 关键字用于声明变量而不定义它，这意味着变量的存储空间在其他地方分配。
> 
> - **函数声明：**
> 
>   ```cpp
>   void foo(int, double); // 声明函数 foo，但不定义
>   ```
> 
>   这告诉编译器存在一个名为 `foo` 的函数，接受一个 `int` 和一个 `double` 参数，并返回 `void`。
> 
> - **类声明（前向声明）：**
> 
>   ```cpp
>   class MyClass; // 前向声明类 MyClass
>   ```
> 
>   前向声明用于告诉编译器类的存在，而不提供类的完整定义。
> 
> ### 定义（Definition）
> 
> 定义是为声明的实体（变量、函数或类）提供实际的存储空间或完整的信息。在定义中，编译器为变量分配存储空间，为函数提供实际的实现代码，为类提供其成员和方法的具体信息。
> 
> - **变量定义：**
> 
>   ```cpp
>   int a = 10; // 定义并初始化变量 a
>   ```
> 
>   这不仅声明了变量 `a`，还为其分配存储空间并初始化为 `10`。
> 
> - **函数定义：**
> 
>   ```cpp
>   void foo(int x, double y) {
>       // 函数实现代码
>   }
>   ```
> 
>   这不仅声明了函数 `foo`，还提供了函数体的实现。
> 
> - **类定义：**
> 
>   ```cpp
>   class MyClass {
>   public:
>       void myMethod();
>   private:
>       int myData;
>   };
>   ```
> 
>   这不仅声明了类 `MyClass`，还定义了其成员变量和成员函数。
> 
> ### 实现（Implementation）
> 
> 实现通常指函数或方法的具体代码，也可以理解为类的成员函数在类定义外的定义。实现通常在源文件（`.cpp` 文件）中进行，以便与声明分离，提供更好的代码组织和编译效率。
> 
> - **函数实现：**
> 
>   ```cpp
>   void foo(int x, double y) {
>       // 函数实现代码
>   }
>   ```
> 
> - **类成员函数实现：**
> 
>   在头文件（`.h` 或 `.hpp` 文件）中声明类和其成员函数：
> 
>   ```cpp
>   class MyClass {
>   public:
>       void myMethod();
>   private:
>       int myData;
>   };
>   ```
> 
>   在源文件（`.cpp` 文件）中实现成员函数：
> 
>   ```cpp
>   #include "MyClass.h"
> 
>   void MyClass::myMethod() {
>       // 方法实现代码
>   }
>   ```
> 
> ### 总结
> 
> - **声明**：告诉编译器名字的存在及其类型或签名。
> - **定义**：为声明的实体提供存储空间或完整信息，此时编译器分配存储空间。
> - **实现**：提供函数或方法的具体代码。


> 减小编译依赖的关键技术就是 pimpl idiom，用户使用的是接口类(Handle class)，这个接口类只有一个指向实现类的指针，这样接口文件就不需要依赖其他复杂的成员类。修改成员类的实现的时候就不会导致用户使用的接口文件的重新编译，只需要重新编译实现文件。除此之外也能用纯虚的抽象基类(interface class)和派生类作为实现的方法，只需要这个作为接口的抽象基类不改变，用户就不需要重新编译


Make your header files self-sufficient whenever it's practical, and when it's not, **depend on declarations in other files, not definitions**. Hence:

- Avoid using objects when object references and pointers will do.
> 定义指针的时候只需要类型的声明，而定义对象的时候则需要对象完整的定义，因为定义会让编译器分配空间，指针的存储空间是确定的；而要定义对象就必须让编译器知道对象的存储空间大小

- Depend on class declarations instead of class definitions whenever you can.
```cpp
class Date; //forward-declaration, practically, client include declaration file, not definition file

//fine - no definition of Date
Date today();
void clear(Date d);

```

By moving the onus of providing class definitions from your header file of function declarations to client's files containing function calls, you eliminate artificial client dependencies on type definitions they don't really need.

- Provide separate header files for declarations and definitions


> 在C++中，工厂方法是一种设计模式，它提供了一种创建对象的接口，但由子类决定实例化哪一个类。这使得在不改变代码结构的情况下，客户端代码可以灵活地创建不同类型的对象。工厂方法模式通常用于解耦对象的创建和使用。
> 
> 下面是一个简单的例子，展示了如何在C++中实现工厂方法模式。
> 
> 首先，定义一个产品的接口或抽象基类：
> 
> ```cpp
> class Product {
> public:
>     virtual void use() = 0;
>     virtual ~Product() {}
> };
> ```
> 
> 然后，定义具体的产品类：
> 
> ```cpp
> class ConcreteProductA : public Product {
> public:
>     void use() override {
>         std::cout << "Using Product A" << std::endl;
>     }
> };
> 
> class ConcreteProductB : public Product {
> public:
>     void use() override {
>         std::cout << "Using Product B" << std::endl;
>     }
> };
> ```
> 
> 接下来，定义工厂方法的接口或抽象基类：
> 
> ```cpp
> class Creator {
> public:
>     virtual Product* factoryMethod() = 0;
> 
>     void doSomething() {
>         Product* product = factoryMethod();
>         product->use();
>         delete product;
>     }
> 
>     virtual ~Creator() {}
> };
> ```
> 
> 最后，实现具体的工厂类：
> 
> ```cpp
> class ConcreteCreatorA : public Creator {
> public:
>     Product* factoryMethod() override {
>         return new ConcreteProductA();
>     }
> };
> 
> class ConcreteCreatorB : public Creator {
> public:
>     Product* factoryMethod() override {
>         return new ConcreteProductB();
>     }
> };
> ```
> 
> 在客户端代码中，可以这样使用工厂方法模式：
> 
> ```cpp
> int main() {
>     Creator* creatorA = new ConcreteCreatorA();
>     Creator* creatorB = new ConcreteCreatorB();
> 
>     creatorA->doSomething(); // Using Product A
>     creatorB->doSomething(); // Using Product B
> 
>     delete creatorA;
>     delete creatorB;
> 
>     return 0;
> }
> ```
> 
> 在这个例子中，`Creator` 类提供了 `doSomething()` 方法，它依赖于 `factoryMethod()` 来创建具体的产品。具体的工厂类 `ConcreteCreatorA` 和 `ConcreteCreatorB` 实现了 `factoryMethod()` 来创建不同的产品对象。这样，通过使用工厂方法模式，客户端代码可以在不修改现有代码的情况下轻松地添加新产品。也可以在抽象基类中将 create 作为静态方法（抽象类不能实例化，必须是静态方法）生成对象，而不用具体的工厂类

The general idea behind minimizing compilation dependencies is to depend on declarations instead of definitions. Two approaches based on this idea are Handle classes and Interface classes.

# 6.Inheritance and Object-Oriented Design

## Item 32: Make sure public inheritance models "is-a"

"Is-a" is true only for public inheritance. Private inheritance means something entirely different (see Item 39)

## Item 33: Avoid hiding inherited names

> C++编译器在解析标识符（名字）时，会按照一定的作用域规则逐层查找，这个过程被称为“名字查找”或“名称解析”，这个标识符可以是变量，也可以是函数，成员函数等。发生在成员函数层面时，可能会隐藏从基类中继承的函数名。具体的查找顺序如下：
> 
> 1. **局部作用域**：编译器首先会在当前的局部作用域中查找标识符。如果找到了，直接使用。
> 
> 2. **更外层的作用域**：如果在当前作用域中找不到标识符，编译器会继续向更外层的作用域查找，直到找到匹配的标识符为止。
> 
> 3. **全局作用域**：如果在所有嵌套的局部作用域中都找不到，编译器会最终查找全局作用域中的标识符。
> 
> 4. **隐藏效应（Shadowing）**：当某个名字在局部作用域中被重新定义时，它会“隐藏”外层或全局作用域中同名的标识符。也就是说，局部作用域中的名字会覆盖同名的全局变量或外层作用域的名字。
> 
> 例如：
> 
> ```cpp
> #include <iostream>
> 
> int x = 10;  // 全局作用域
> 
> void foo() {
>     double x = 20;  // 局部作用域，隐藏了全局作用域的 x
>     std::cout << "Local x: " << x << std::endl;
> }
> 
> int main() {
>     std::cout << "Global x: " << x << std::endl;
>     foo();
>     return 0;
> }
> ```
> 
> 在这个例子中：
> 
> - 在 `main()` 函数中，输出的是全局作用域的 `x`，所以输出 `Global x: 10`。
> - 在 `foo()` 函数中，局部定义的 `x` 隐藏了全局作用域的 `x`，所以输出 `Local x: 20`。
> 
> 这种隐藏效应是C++语言中作用域规则的一个关键特点。在实际编程中，如果不小心处理可能会引起混淆或错误，所以要注意作用域的使用和命名的一致性。
> 当发生在函数名的解析上时，即使函数的参数列表不同，只要名字相同，全局的函数名就会被局部的函数隐藏，有时可能会导致基类的函数被继承子类声明的同名函数（**即使参数列表不同**）隐藏，就像局部的 double x也能覆盖全局的int x一样，名字是关键，类型并不重要

> 在C++中，“inherit the overloads”指的是子类继承了基类中所有重载的函数。这意味着基类中定义的所有重载函数在子类中依然可用，除非子类对某些重载进行了覆盖或隐藏。
> 
> ### 举个例子
> 假设我们有一个基类 `Base`，其中有多个重载的函数 `func`：
> 
> ```cpp
> class Base {
> public:
>     void func(int x) {
>         std::cout << "Base func(int): " << x << std::endl;
>     }
> 
>     void func(double x) {
>         std::cout << "Base func(double): " << x << std::endl;
>     }
> };
> ```
> 
> 现在，我们有一个从 `Base` 继承的子类 `Derived`，它可能会定义一个新的重载的 `func` 函数：
> 
> ```cpp
> class Derived : public Base {
> public:
>     void func(std::string x) {
>         std::cout << "Derived func(std::string): " << x << std::endl;
>     }
> };
> ```
> 
> 在 `Derived` 中，如果我们不做任何额外处理，`Base` 类中的 `func(int)` 和 `func(double)` 将被隐藏，只有 `Derived` 中定义的 `func(std::string)` 可见：
> 
> ```cpp
> int main() {
>     Derived d;
>     d.func("Hello"); // 调用 Derived::func(std::string)
>     d.func(10);      // 错误！Base::func(int) 被隐藏
> }
> ```
> 
> ### 如何继承重载函数
> 
> 为了在子类中也能使用基类的所有重载版本，可以使用 `using` 语句将基类的重载函数引入子类的作用域：
> 
> ```cpp
> class Derived : public Base {
> public:
>     using Base::func; // 引入 Base 类中的所有重载版本的 func
> 
>     void func(std::string x) {
>         std::cout << "Derived func(std::string): " << x << std::endl;
>     }
> };
> ```
> 
> 现在，你可以在 `Derived` 中使用所有的重载函数：
> 
> ```cpp
> int main() {
>     Derived d;
>     d.func("Hello"); // 调用 Derived::func(std::string)
>     d.func(10);      // 调用 Base::func(int)
>     d.func(3.14);    // 调用 Base::func(double)
> }
> ```
> 
> ### 总结
> 
> “Inherit the overloads” 的意思是子类可以继承基类中所有重载的函数。通过在子类中使用 `using` 关键字，可以确保基类中的所有重载版本在子类中依然可用，从而避免隐藏效应。


> “Inline forwarding functions” 是 C++ 中的一种设计模式，通常用于将一个函数的调用转发给另一个函数。这种方法可以在编写通用代码、优化性能和增强代码可维护性方面发挥重要作用。
> 
> ### 什么是 Forwarding Functions？
> 
> “Forwarding function”（转发函数）指的是一个函数，它的主要任务是将其参数转发给另一个函数。这个转发的过程可以是简单的直接调用，也可以包括一些额外的逻辑，比如参数转换、验证等。
> 
> ### 什么是 Inline Forwarding Functions？
> 
> “Inline forwarding functions” 就是以 `inline` 方式定义的转发函数，通常用于避免函数调用的开销。`inline` 关键字提示编译器将函数的调用直接展开为内联代码，减少函数调用的额外成本。
> 
> ### 举个例子
> 
> ```cpp
> #include <iostream>
> 
> class Base {
> public:
>     void process(int x) {
>         std::cout << "Processing int: " << x << std::endl;
>     }
> 
>     void process(double x) {
>         std::cout << "Processing double: " << x << std::endl;
>     }
> };
> 
> class Derived : public Base {
> public:
>     // Inline forwarding function
>     inline void process(int x) {
>         Base::process(x);  // Forward the int to Base::process(int)
>     }
> 
>     inline void process(double x) {
>         Base::process(x);  // Forward the double to Base::process(double)
>     }
> };
> ```
> 
> 在这个例子中，`Derived` 类定义了两个 `process` 函数，它们都是 `inline` 的，并且它们的唯一功能是将调用转发给基类 `Base` 中的相应函数。
> 
> ### 为什么使用 Inline Forwarding Functions？
> 
> 1. **性能优化**：通过将函数内联展开，可以减少函数调用的开销，尤其是在性能关键的代码中。
> 
> 2. **代码复用**：可以在派生类中直接调用基类的函数，而不必重复实现相同的功能。
> 
> 3. **类型安全**：可以确保转发函数的参数类型与被转发函数一致，避免不必要的类型转换。
> 
> 4. **增强可读性**：转发函数有助于保持接口的一致性，提供更直观的代码结构。
> 
> ### 举个更复杂的例子
> 
> 在模板和泛型编程中，`std::forward` 通常与转发函数一起使用，以保留参数的左值/右值属性：
> 
> ```cpp
> #include <iostream>
> #include <utility>
> 
> template<typename T>
> void process(T&& x) {
>     std::cout << "Processing: " << x << std::endl;
> }
> 
> template<typename T>
> inline void forward_to_process(T&& x) {
>     process(std::forward<T>(x));  // Forwarding the argument while preserving its value category
> }
> 
> int main() {
>     int a = 10;
>     forward_to_process(a);  // Lvalue is forwarded as Lvalue
>     forward_to_process(20); // Rvalue is forwarded as Rvalue
> }
> ```
> 
> 在这个例子中，`forward_to_process` 是一个内联转发函数，它使用 `std::forward` 将参数转发给 `process` 函数，同时保留了参数的左右值属性。
> 
> ### 总结
> 
> Inline forwarding functions 是一种内联的转发函数，它可以通过直接调用另一个函数来减少函数调用的开销。这种技术在模板编程和泛型编程中非常有用，特别是在需要保持参数类型和左右值属性的情况下。

## Item 34: Differentiate between inheritance of interface and inheritance of implementation
> 
> 在C++中，为纯虚函数（也称为抽象函数）提供定义实现确实有实际应用，尽管这种做法不常见，但在某些情况下是非常有用的。以下是几个实际应用的场景：
> 
> ### 1. **提供默认实现**
>    纯虚函数通常被用作接口，用来强制派生类提供自己的实现。然而，基类可以为纯虚函数提供一个默认实现，供派生类在特定情况下调用。派生类可以选择使用基类的实现，而不是重新定义自己的版本。当有新类继承基类的时候必须实现纯虚函数，如果有自己独特实现可以自己定义，否则可以直接调用父类的纯虚函数。**实际上有实现的纯虚函数已经分裂成了两部分：函数接口和函数默认实现。函数接口一定可以被继承，而默认实现来说，当子类需要的时候可以也继承，但必须自己亲手调用，而不是必定继承**。但是非纯的虚函数，实现和接口都是必定继承的
>    ```cpp
>    class Base {
>    public:
>        virtual void foo() = 0; // 纯虚函数
>    };
> 
>    void Base::foo() {
>        // 默认实现
>        std::cout << "Default implementation in Base class." << std::endl;
>    }
> 
>    class Derived : public Base {
>    public:
>        void foo() override {
>            Base::foo(); // 调用基类中的实现
>        }
>    };
>    ```
> 
> ### 2. **实现部分逻辑，并强制派生类完成剩余部分**
>    基类可以实现纯虚函数中的部分逻辑，而将剩余部分留给派生类实现。这样可以确保派生类遵循某种框架或约定，同时仍然强制它们提供具体的实现。
> 
>    ```cpp
>    class Base {
>    public:
>        virtual void process() = 0; // 纯虚函数
>    protected:
>        void commonTask() {
>            std::cout << "Common task done by Base." << std::endl;
>        }
>    };
> 
>    void Base::process() {
>        commonTask(); // 调用基类中的通用任务
>        // 留给派生类的具体实现
>    }
> 
>    class Derived : public Base {
>    public:
>        void process() override {
>            Base::process(); // 调用基类中实现的部分逻辑
>            std::cout << "Specific task done by Derived." << std::endl;
>        }
>    };
>    ```
> 
> ### 3. **避免重复代码**
>    如果多个派生类中存在公共的操作步骤，这些步骤可以在基类的纯虚函数中实现，而让派生类负责处理与自身相关的独特操作。
> 
> ### 4. **实现基类析构函数**
>    纯虚析构函数是一个特殊情况。通常，我们定义一个纯虚析构函数以确保基类是抽象类，同时为其提供一个实现以确保正确的资源管理。
> 
>    ```cpp
>    class Base {
>    public:
>        virtual ~Base() = 0; // 纯虚析构函数
>    };
> 
>    Base::~Base() {
>        // 析构函数实现
>        std::cout << "Base destructor called." << std::endl;
>    }
>    ```
> 
> 在上述这些场景中，基类中的纯虚函数既可以提供共性功能的实现，也可以在一定程度上约束派生类的实现方式，因此在某些设计模式中会用到这种技术。

- Pure virtual functions specify inheritance of interface only.
- Impure virtual functions specify inheritance of inheritance plus inheritance of a default implementation.
- Non-virtual functions specify inheritance of inheritance plus inheritance of a mandatory implementation.

> mandatory implementation 指的是必须是这个函数实现，命令性质的，任何派生类都不应该修改

## Item 35: Consider alternatives to  virtual functions

There are different ways to virtual function.

### The Template Method Pattern via the Non-Virtual Interface Idiom

> **这里书中提到了子类重新定义父类的私有函数，不对吧**。
> 在 C++ 中，`Non-Virtual Interface` (NVI) 是一种设计惯用法，用于创建公共接口，同时将具体实现细节隐藏在基类中。NVI 模式的关键思想是将公共方法定义为非虚函数（`non-virtual`），而实际的实现细节则通过私有或受保护的虚函数实现。这样可以提高代码的健壮性和可维护性，实际使用的是模板方法，基类创建一个模板调用子类才能实现的虚函数。
> 
> ### NVI 模式的基本结构
> 1. **公共非虚接口** (`Public Non-Virtual Interface`)：
>     - 这是提供给外部使用的公共方法。它通常是一个非虚函数，定义在基类中，负责调用虚函数来实现具体功能。
>     - 这种非虚函数通常负责执行一些通用操作，比如参数检查、资源管理或日志记录。
> 
> 2. ** 受保护的虚函数** (`Private/Protected Virtual Function`)：
>     - 这些是具体实现的方法，通常在派生类中进行重写。基类中的公共非虚接口调用这些虚函数来实现特定的行为。
> 
> ### 示例代码
> ```cpp
> class Base {
> public:
>     void interfaceMethod() { // 公共非虚接口
>         // 一些通用操作
>         commonOperation();
> 
>         // 调用虚函数实现具体功能
>         specificOperation();
>     }
> 
> protected:
>     virtual void specificOperation() = 0; // 纯虚函数，派生类需实现
> 
> private:
>     void commonOperation() {
>         // 通用操作代码
>         std::cout << "Common operation in Base" << std::endl;
>     }
> };
> 
> class Derived : public Base {
> protected:
>     void specificOperation() override { // 实现基类中的虚函数
>         std::cout << "Specific operation in Derived" << std::endl;
>     }
> };
> 
> int main() {
>     Derived d;
>     d.interfaceMethod(); // 调用非虚接口，实际执行 Derived 中的 specificOperation()
>     return 0;
> }
> ```
> 
> ### NVI 模式的优点
> 1. **统一接口**：通过非虚函数提供统一的接口，确保所有派生类遵循相同的调用方式。
> 2. **增强控制**：在基类中可以进行通用操作的预处理和后处理（如参数验证、日志记录等）。
> 3. **隐藏实现细节**：派生类只需要实现特定功能，而不需要考虑公共接口的实现细节。
> 4. **提高稳定性**：公共接口的非虚函数不会被派生类重写，减少了错误的可能性。
> 
> NVI 模式在需要严格控制接口行为、避免派生类修改公共接口行为的情况下非常有用。

### The Strategy Pattern via Function Pointers

> C++ 中的函数指针是一个指向函数的指针，这意味着它可以存储一个函数的地址，并通过这个指针调用该函数。函数指针允许在运行时动态地调用函数，使代码更加灵活和可扩展。
> 
> ### 基本概念
> 
> 1. **函数指针的声明**: 
>    函数指针的声明方式与普通指针类似，只不过指针的类型是函数的返回类型加上参数列表。例如：
> 
>    ```cpp
>    // 声明一个指向返回类型为 int，参数类型为 (int, int) 的函数的指针
>    int (*funcPtr)(int, int);
>    ```
> 
> 2. **赋值给函数指针**:
>    可以将与函数指针签名（返回类型和参数类型）相匹配的函数的地址赋给函数指针。例如：
> 
>    ```cpp
>    int add(int a, int b) {
>        return a + b;
>    }
> 
>    int (*funcPtr)(int, int) = &add;  // 将函数 add 的地址赋给函数指针
>    ```
> 
>    在 C++ 中，函数名本身就表示函数的地址，所以可以省略 `&`，即：
> 
>    ```cpp
>    int (*funcPtr)(int, int) = add; 
>    ```
> 
> 3. **通过函数指针调用函数**:
>    通过函数指针调用函数的方式与直接调用函数类似，只不过需要在指针前面加上 `*` 或省略它：
> 
>    ```cpp
>    int result = (*funcPtr)(2, 3);  // 调用函数 add，结果为 5
>    // 或者
>    int result = funcPtr(2, 3);  // 同样调用函数 add，结果为 5
>    ```
> 
> ### 函数指针的应用场景
> 
> 1. **回调函数**: 在某些 API 或库函数中，用户可以将自定义的函数作为参数传递给库函数。库函数在特定情况下调用用户提供的函数。这种机制常用于事件驱动编程和信号处理。
> 
> 2. **实现策略模式**: 可以用函数指针来实现不同策略之间的切换，如排序算法中选择不同的比较函数。
> 
> 3. **函数表**: 可以创建一个函数指针数组，根据索引动态调用不同的函数。
> 
> ### 例子
> 
> 以下是一个简单的例子，演示如何使用函数指针来选择不同的运算：
> 
> ```cpp
> #include <iostream>
> 
> // 定义两个函数，分别用于加法和乘法
> int add(int a, int b) {
>     return a + b;
> }
> 
> int multiply(int a, int b) {
>     return a * b;
> }
> 
> int main() {
>     // 定义一个函数指针
>     int (*operation)(int, int);
> 
>     // 根据用户输入选择不同的运算
>     char op;
>     std::cout << "Enter operation (+ or *): ";
>     std::cin >> op;
> 
>     if (op == '+') {
>         operation = add;
>     } else if (op == '*') {
>         operation = multiply;
>     }
> 
>     // 使用函数指针调用函数
>     int result = operation(5, 3);
>     std::cout << "Result: " << result << std::endl;
> 
>     return 0;
> }
> ```
> 
> 在这个例子中，根据用户的输入选择不同的运算，并通过函数指针调用相应的函数。

### The Strategy Pattern via tr1::function

> 在C++中，`tr1::function` 是 C++ 标准库中的一种模板类，位于 `<tr1/functional>` 头文件中。它是 C++11 引入的 `std::function` 的前身，属于 TR1（Technical Report 1）库的一部分，TR1 是 C++ 标准库的一个扩展。
> 
> `tr1::function` 类模板用于存储、复制和调用任何可调用对象，包括普通函数、函数对象、lambda 表达式，以及成员函数指针。它提供了一种通用的方法来处理这些不同类型的可调用对象。
> 
> ### 用法示例
> 
> ```cpp
> #include <iostream>
> #include <tr1/functional>
> 
> void my_function(int x) {
>     std::cout << "Function called with value: " << x << std::endl;
> }
> 
> int main() {
>     std::tr1::function<void(int)> func = my_function;
>     func(10);  // 输出: Function called with value: 10
>     return 0;
> }
> ```
> 
> 在上面的代码中，`func` 是一个 `tr1::function` 对象，它可以存储并调用一个接受 `int` 参数并返回 `void` 的函数。
> 
> ### 特点
> 
> - **多态性**：`tr1::function` 可以存储不同类型的可调用对象，只要它们的签名匹配。
> - **安全性**：`tr1::function` 对象在调用被包装的可调用对象时，自动处理对象的生命周期，并且可以检测和处理空的 `function` 对象（即没有绑定到任何可调用对象的情况）。
> - **灵活性**：支持各种类型的可调用对象，如函数指针、成员函数指针、函数对象和 lambda 表达式。
> 
> ### 迁移到 `std::function`
> 
> 虽然 `tr1::function` 是一个有用的工具，但随着 C++11 的引入，它被 `std::function` 所取代。`std::function` 提供了更强大的功能，建议在现代 C++ 代码中使用 `std::function` 代替 `tr1::function`。
> 
> ```cpp
> #include <iostream>
> #include <functional>  // 使用std::function
> 
> void my_function(int x) {
>     std::cout << "Function called with value: " << x << std::endl;
> }
> 
> int main() {
>     std::function<void(int)> func = my_function;
>     func(10);  // 输出: Function called with value: 10
>     return 0;
> }
> ```
> 
> 如果你在学习或维护遗留代码，理解 `tr1::function` 会有帮助，但在新的项目中，应该优先使用 `std::function`。

>  `tr1::bind` 是 C++ 标准库的一部分，用于创建函数对象（function object）或函数适配器（function adapter），通过将参数绑定到函数或成员函数上。`tr1::bind` 是 `std::bind` 的前身，在 C++11 引入 `std::bind` 之前用于相同的目的。
>  
>  ### 作用
>  
>  `tr1::bind` 可以将特定的参数绑定到一个函数或成员函数上，从而创建一个新的函数对象，该对象可以稍后调用，而不需要提供那些已经绑定的参数。
>  
>  ### 语法
>  
>  ```cpp
>  #include <functional>
>  #include <tr1/functional>
>  
>  auto boundFunction = std::tr1::bind(function, arg1, arg2, ...);
>  ```
>  
>  - `function` 是你要绑定的函数或成员函数。
>  - `arg1, arg2, ...` 是你想绑定的参数。
>  
>  ### 示例
>  
>  假设我们有一个简单的函数 `add`，它接受两个整数并返回它们的和：
>  
>  ```cpp
>  #include <iostream>
>  #include <tr1/functional>
>  
>  int add(int x, int y) {
>      return x + y;
>  }
>  
>  int main() {
>      // 将函数 `add` 绑定一个参数
>      auto addFive = std::tr1::bind(add, 5, std::tr1::placeholders::_1);
>  
>      // 调用绑定后的函数对象，只需提供一个参数
>      std::cout << addFive(10) << std::endl; // 输出 15
>  
>      return 0;
>  }
>  ```
>  
>  在这个示例中，`std::tr1::bind(add, 5, std::tr1::placeholders::_1)` 绑定了函数 `add` 的第一个参数为 `5`，并创建了一个新的函数对象 `addFive`，它接受一个参数并将其加上 `5`。
>  
>  ### 升级到 C++11
>  
>  如果你在使用 C++11 或更高版本，建议使用 `std::bind` 而不是 `tr1::bind`，因为 `tr1` 已被废弃。
>  
>  ### `std::bind` 示例
>  
>  ```cpp
>  #include <iostream>
>  #include <functional>
>  
>  int add(int x, int y) {
>      return x + y;
>  }
>  
>  int main() {
>      // 使用 std::bind
>      auto addFive = std::bind(add, 5, std::placeholders::_1);
>  
>      std::cout << addFive(10) << std::endl; // 输出 15
>  
>      return 0;
>  }
>  ```
>  
>  `std::bind` 的使用方式与 `tr1::bind` 非常相似，只不过你需要将 `std::tr1` 替换为 `std`。

### The "Class" Strategy Pattern

It's more flexible and familiar to someone.

![summary](35.png)

## Item 36: Never redefine an inherited non-virtual function

```cpp
class B{
public:
    void mf();
};

class D: public B{
public:
    void mf();
};

D x;

B* pb=&x;
D* pd=&x;

pb->mf();
pd->mf();

```
pb and pd behave differently, even though they are pointing to the same object;

> 在C++中，如果子类重写了父类的非虚函数，调用该函数时的行为取决于对象的静态类型（即声明类型）而不是对象的动态类型（即实际类型）。这意味着，当用不同类型的指针或引用（如父类指针或子类指针）指向同一个子类对象时，调用的非虚函数是根据指针或引用的静态类型决定的。非虚函数是静态绑定的，在编译时就已经确定了调用哪个函数。只有虚函数才会将具体函数的调用推迟到运行时
> 
> 举个例子：
> 
> ```cpp
> #include <iostream>
> 
> class Base {
> public:
>     void nonVirtualFunction() {
>         std::cout << "Base's non-virtual function" << std::endl;
>     }
> };
> 
> class Derived : public Base {
> public:
>     void nonVirtualFunction() {
>         std::cout << "Derived's non-virtual function" << std::endl;
>     }
> };
> 
> int main() {
>     Derived d;
>     Base* basePtr = &d;
>     Derived* derivedPtr = &d;
> 
>     basePtr->nonVirtualFunction();   // 调用 Base::nonVirtualFunction()
>     derivedPtr->nonVirtualFunction(); // 调用 Derived::nonVirtualFunction()
> 
>     return 0;
> }
> ```
> 
> 在这个例子中，虽然 `basePtr` 和 `derivedPtr` 都指向同一个 `Derived` 对象，但由于 `nonVirtualFunction` 不是虚函数，实际调用的函数取决于指针的静态类型：
> 
> - `basePtr->nonVirtualFunction()` 会调用 `Base` 类中的 `nonVirtualFunction`。
> - `derivedPtr->nonVirtualFunction()` 会调用 `Derived` 类中的 `nonVirtualFunction`。
> 
> 这正是由于非虚函数的静态绑定所导致的。在编译时，编译器会根据指针或引用的类型决定要调用哪个函数，因此即使两个指针指向的是同一个对象，调用的函数也可能不同。因此必须避免重写父类的非虚函数，不然会导致混乱

## Item 37:  Never redefine a function's inherited default parameter value

Virtual functions are dynamic bound, but default parameter values are statically bound.

> 在C++中，如果你在派生类中调用基类的虚函数，并且该虚函数有默认参数值，默认参数值是静态绑定的。这意味着默认参数值是在编译时确定的，并且基类版本的默认参数会在调用时使用，而不会考虑派生类中是否重新定义了该虚函数或提供了不同的默认参数值。
> 
> 具体来说，如果基类的虚函数有默认参数，而你在派生类中没有覆盖该函数，并且你在派生类的对象上调用该函数，使用的是基类的默认参数值，而不是派生类的。即使派生类覆盖了该函数，调用派生类版本的函数时，仍然会使用基类中定义的默认参数值。
> 
> 这是因为默认参数值是在函数声明时静态决定的，并不是虚函数表（vtable）的一部分。虚函数表只负责在运行时动态绑定具体调用哪个函数版本，而不负责动态绑定默认参数。
> 
> 以下是一个例子来说明这一点：
> 
> ```cpp
> #include <iostream>
> 
> class Base {
> public:
>     virtual void func(int x = 10) {
>         std::cout << "Base: " << x << std::endl;
>     }
> };
> 
> class Derived : public Base {
> public:
>     void func(int x = 20) override {
>         std::cout << "Derived: " << x << std::endl;
>     }
> };
> 
> int main() {
>     Derived d;
>     Base* basePtr = &d;
> 
>     basePtr->func(); // 调用的是 Derived::func，但使用的是 Base 中的默认参数
> 
>     return 0;
> }
> ```
> 
> 在这个例子中，`basePtr->func();` 调用了 `Derived::func()`，但是使用的是 `Base` 中的默认参数 `10`，输出为：
> 
> ```
> Derived: 10
> ```
> 
> 而如果你在 `Derived` 类的对象上直接调用 `func()`，那么你需要显式传递参数，或者手动覆盖默认参数，因为默认参数是在基类中定义的。

## Item 38: Model "has-a" or "is-implemented-in-terms-of" through composition

> 在C++中，"is-implemented-in-terms-of" 通过组合（composition）来实现，通常意味着一个类使用另一个类的实例来实现其功能，而不是通过继承。换句话说，类A使用类B的一个或多个对象作为其成员变量，从而借助这些对象来完成某些工作。这样的设计强调类之间的**"has-a"**关系，而非**"is-a"**关系。
> 
> 例如：
> 
> ```cpp
> class Engine {
> public:
>     void start() {
>         // 启动引擎的代码
>     }
> };
> 
> class Car {
> private:
>     Engine engine;  // Car类通过组合包含一个Engine对象
> 
> public:
>     void start() {
>         engine.start();  // Car类利用Engine对象的功能来实现其启动功能
>     }
> };
> ```
> 
> 在这个例子中，`Car`类通过组合包含了一个`Engine`对象，并通过`Engine`对象来实现汽车启动的功能。这就是通过组合实现的“is-implemented-in-terms-of”的一种表现形式。
> 
> 这种设计的好处在于，它鼓励模块化和封装，使得代码更加易于维护和扩展。同时，相比继承，组合使类之间的耦合性更低。

## Item 39: Use private inheritance judiciously

> 在 C++ 中，`private` 继承是一种访问控制形式，它影响了从基类继承的成员在派生类中的可访问性和可见性。以下是 `private` 继承的一些主要特性：
> 
> 1. **基类的公有和保护成员**：
>    - 当使用 `private` 继承时，基类的所有 `public` 和 `protected` 成员在派生类中都变为 `private`。这意味着这些成员只能在派生类的内部访问，不能通过派生类的对象或从派生类派生的类访问。
> 
> 2. **基类的私有成员**：
>    - 基类的 `private` 成员仍然是私有的，派生类无法直接访问这些成员，但可以通过基类的公有或受保护的成员函数间接访问。
> 
> 3. **不改变基类的访问性**：
>    - `private` 继承不会改变基类的访问权限，也就是说，基类的 `private` 成员仍然是私有的，无法被派生类直接访问。
> 
> 4. **继承类型不影响派生类的访问级别**：
>    - 无论是 `private` 继承还是 `public` 继承，派生类自身的成员访问级别不会受到影响，只是继承自基类的成员访问级别会根据继承类型而改变。
> 
> 5. **`is-a` 关系的破坏**：
>    - `private` 继承不提供一个 `is-a` 关系。这意味着派生类对象不能被看作是基类对象，因为基类的接口对外部是不可见的。因此，不能使用基类的指针或引用来指向派生类对象。
> 
> 6. **用作实现工具**：
>    - `private` 继承通常用于实现细节的封装，以便派生类能够使用基类的功能，而不让这些功能暴露给派生类的用户。
> 
> 以下是一个示例代码：
> 
> ```cpp
> class Base {
> public:
>     void publicMethod() {}
> protected:
>     void protectedMethod() {}
> private:
>     void privateMethod() {}
> };
> 
> class Derived : private Base {
> public:
>     void derivedMethod() {
>         publicMethod();       // 可以访问，但对 Derived 的用户是不可见的
>         protectedMethod();    // 可以访问，但对 Derived 的用户是不可见的
>         // privateMethod();   // 错误，无法访问
>     }
> };
> 
> int main() {
>     Derived d;
>     // d.publicMethod();    // 错误，无法访问
>     // d.protectedMethod(); // 错误，无法访问
>     d.derivedMethod();       // 可以访问
>     return 0;
> }
> ```
> 
> 在这个例子中，`Base` 类的 `publicMethod` 和 `protectedMethod` 在 `Derived` 类中都变成了私有的，所以它们不能通过 `Derived` 类的对象在 `main` 函数中访问。


Private inheritance means is-implement-in-terms-of, there is not any conceptual relation. You doprivate inheritance because you are interested in taking advantage of some of the features available in base class.

> is-implement-in-terms-of 在item38上面有解释

**Use composition whenever you can, and use private inheritance whenever you must. When must you? Primarily when protect members or virtual functions enter the picture.**

> 是的，在C++中，即使一个类是空的，它也会占用一定的内存空间。具体来说，空类通常会占用一个字节的空间。
> 
> 原因是C++中，每个对象都需要一个唯一的地址，以便在内存中能够区分不同的对象。如果空类不占用任何空间，那么多个空类对象将无法有不同的地址。因此，编译器通常为每个空类对象分配一个字节的空间，以保证每个对象有唯一的地址。
> 
> ```cpp
> class Empty {};
> 
> int main() {
>     Empty e1, e2;
>     std::cout << sizeof(e1) << std::endl; // 输出通常为1
>     std::cout << &e1 << std::endl;        // 输出e1的地址
>     std::cout << &e2 << std::endl;        // 输出e2的地址，不同于e1的地址
>     return 0;
> }
> ```
> 
> 在这个示例中，`sizeof(e1)`的值通常为1，表明空类对象占用了一个字节的空间。这是为了确保不同对象有不同的地址。

> Empty Base Optimization (EBO) 是C++中的一种编译器优化技术，用于减少内存占用。当一个类继承自一个空基类时，通过EBO，编译器可以消除该空基类的内存开销，从而不额外占用空间。只有私有继承能做到EBO，composition不能实现EBO。实际上STL中有很多空类，他们只是有一些typedef，而没有实际内容。EBO的存在使得STL这样的实现节省了空间
> 
> ### 背景
> 如你之前提到的，空类通常占用1个字节的空间，以确保对象有唯一的地址。然而，当一个类继承自一个或多个空基类时，这些空基类的内存开销可能被优化掉，以便节省内存。这种优化主要用于减少内存的浪费，尤其是在设计复杂的继承体系或使用模板编程时。
> 
> ### EBO 的工作原理
> 当一个类继承自一个空基类时，如果编译器能够确定该空基类在内存布局中不需要占用空间，它就会将派生类和基类的数据成员“合并”，使得空基类不额外占用内存。这通常是通过让空基类与第一个非空的数据成员共享相同的地址来实现的。
> 
> ### 示例
> ```cpp
> #include <iostream>
> 
> class Empty {};
> 
> class Derived : public Empty {
>     int x;
> };
> 
> int main() {
>     std::cout << "Size of Empty: " << sizeof(Empty) << std::endl;       // 通常为1
>     std::cout << "Size of Derived: " << sizeof(Derived) << std::endl;   // 通常为4
>     return 0;
> }
> ```
> 
> 在这个示例中，如果没有EBO，`Derived`类的大小将是`sizeof(int) + sizeof(Empty)`，即4 + 1 = 5个字节。但在大多数实现了EBO的编译器中，`Derived`类的大小将只是4个字节，因为空基类`Empty`不会占用额外的空间。
> 
> ### 何时使用
> EBO主要在模板编程、标准库容器以及其他需要继承空类的场景中使用，特别是当多个空类继承时，可以显著减少内存开销。标准库中的`std::pair`和`std::tuple`类模板就是常见的应用场景，它们经常使用EBO来优化内存布局。
> 
> ### 注意事项
> 虽然EBO在大多数现代C++编译器中是支持的，但它并不是C++标准的一部分，也就是说编译器并不强制要求实现EBO。这意味着在某些特定编译器或编译设置下，EBO可能不会生效。

## Item 40: Use multiple inheritance judiciously

> 在C++中，当一个类通过多重继承继承自多个基类，而这些基类中包含同名的成员函数时，即使这些成员函数中只有一个是`public`，在派生类中调用时仍然可能会产生歧义，编译器无法确定应该调用哪个基类的函数。
> 因为c++中是先找到最佳匹配的函数调用，再看有没有调用权，而没有找最佳匹配时已经把调用权考虑进去。下面的代码会报错
> 
> 假设你有以下代码：
> 
> ```cpp
> class Base1 {
> public:
>     void func() {
>         std::cout << "Base1::func" << std::endl;
>     }
> };
> 
> class Base2 {
> private:
>     void func() {
>         std::cout << "Base2::func" << std::endl;
>     }
> };
> 
> class Derived : public Base1, public Base2 {
>     // Derived does not override func()
> };
> ```
> 
> 在这个例子中，`Derived`类从`Base1`和`Base2`继承，而两个基类都具有名为`func`的成员函数。即使`Base2`中的`func`是`private`，它仍然在`Derived`类中存在（只是不能直接调用）。因此，当你在`Derived`类的对象中尝试调用`func`时，编译器会遇到歧义。
> 
> 解决这一问题的常见方法是显式地指定要调用的基类的成员函数，或者在派生类中重写该函数。例如：
> 
> ```cpp
> int main() {
>     Derived d;
>     d.Base1::func();  // 显式调用 Base1 中的 func
>     // d.func();  // 这样会引发歧义错误
>     return 0;
> }
> ```
> 
> 如果你想避免在派生类中手动指定调用哪个基类的函数，可以在派生类中定义自己的`func`函数，这样就能覆盖基类的同名函数。
> 
> ```cpp
> class Derived : public Base1, public Base2 {
> public:
>     using Base1::func;  // 使用 Base1 的 func
> };
> ```
> 
> 这样，`Derived`中的`func`会明确使用`Base1`中的实现。

> C++ 中的“致命多重继承菱形问题”（Deadly Multiple Inheritance Diamond Problem）是指在多重继承中，如果一个派生类通过两个不同的路径继承了同一个基类，就可能会出现歧义和数据重复的问题。
> 
> ### 问题的起因
> 
> 考虑以下示例代码：
> 
> ```cpp
> class A {
> public:
>     void func() {
>         std::cout << "A::func" << std::endl;
>     }
> };
> 
> class B : public A {
>     // B inherits A
> };
> 
> class C : public A {
>     // C also inherits A
> };
> 
> class D : public B, public C {
>     // D inherits both B and C, thus has two A base parts
> };
> ```
> 
> 在上述代码中，类 `D` 通过类 `B` 和类 `C` 继承了类 `A`。这导致 `D` 中实际上有两个 `A` 类的副本（`B` 中的一个，`C` 中的一个）。这种结构被称为“菱形继承”，因为类继承关系形成了一个菱形形状。
> 
> #### 菱形继承的问题：
> 
> 1. **歧义问题：** 如果你在 `D` 类的对象上调用 `A` 类的成员函数，例如 `func()`，编译器会报错，因为它无法确定应该调用哪个 `A` 类的 `func()` 函数——`B` 中的 `A` 还是 `C` 中的 `A`。
> 
> 2. **数据冗余：** 如果 `A` 类包含数据成员，那么 `D` 类将拥有两份 `A` 类的数据副本，可能会导致不必要的内存开销和数据不一致。
> 
> ### 解决方案：虚拟继承
> 
> 为了解决这些问题，可以使用虚拟继承。虚拟继承使得所有继承 `A` 的派生类只保留一个 `A` 类的实例，而不会创建多个副本。
> 
> ```cpp
> class A {
> public:
>     void func() {
>         std::cout << "A::func" << std::endl;
>     }
> };
> 
> class B : public virtual A {
>     // B virtually inherits A
> };
> 
> class C : public virtual A {
>     // C also virtually inherits A
> };
> 
> class D : public B, public C {
>     // D now has only one A part due to virtual inheritance
> };
> ```
> 
> 使用虚拟继承后，即使 `D` 类通过 `B` 和 `C` 间接继承了 `A`，它仍然只会保留一个 `A` 的实例。因此，调用 `D` 的 `func()` 时，编译器不会报歧义错误，并且不会有数据冗余问题。
> 
> ```cpp
> int main() {
>     D d;
>     d.func();  // 直接调用 A 的 func，没有歧义
>     return 0;
> }
> ```
> 
> 这样，编译器就能够正确地解析并调用 `A::func()`，避免了菱形继承中的典型问题。标准C++库中存在这样的虚拟继承情况，就是basic_ios, basic_istream, basic_ostrean, basic_iostream，他们是菱形继承的类模板

> 虚拟继承是C++中的一种特殊的继承方式，用于解决多重继承中可能出现的菱形继承问题（diamond problem），即多个派生类通过不同路径继承同一个基类，导致基类的成员在最终派生类中出现多份副本的问题。虚拟继承确保无论通过多少路径继承同一个基类，在最终派生类中该基类的成员只有一份。
> 
> ### 虚拟继承的工作原理
> 
> 虚拟继承的关键是确保在复杂的继承结构中，只有一个共享的基类子对象。当使用虚拟继承时，所有派生类共享同一个基类子对象，而不是每个派生类都拥有自己独立的基类子对象。
> 
> ### 虚拟继承的特点
> 
> 1. **共享基类子对象：** 在虚拟继承中，所有派生类共享同一个基类子对象，而不是每个派生类都独立拥有自己的基类子对象。
> 
> 2. **构造函数调用顺序：** 虚拟基类的构造函数在最底层派生类（最终派生类）的构造函数中被调用。在虚拟继承的情况下，最终派生类负责初始化虚拟基类，而不是中间派生类。
> 
>    例如：
> 
>    ```cpp
>    class A {
>    public:
>        A() { std::cout << "A's constructor" << std::endl; }
>    };
> 
>    class B : public virtual A {
>    public:
>        B() { std::cout << "B's constructor" << std::endl; }
>    };
> 
>    class C : public virtual A {
>    public:
>        C() { std::cout << "C's constructor" << std::endl; }
>    };
> 
>    class D : public B, public C {
>    public:
>        D() { std::cout << "D's constructor" << std::endl; }
>    };
> 
>    int main() {
>        D d;
>        return 0;
>    }
>    ```
> 
>    输出顺序为：
> 
>    ```
>    A's constructor
>    B's constructor
>    C's constructor
>    D's constructor
>    ```
> 
>    这是因为 `A` 的构造函数只会在 `D` 中被调用一次，避免了多次构造的问题。
> 
> 3. **内存布局：** 虚拟继承会稍微增加内存布局的复杂性。编译器需要为虚拟基类生成一个虚表（VTable）来管理共享的基类子对象的指针。因此，虚拟继承的类通常比非虚拟继承的类占用更多的内存，并且在运行时可能有一定的性能开销。
> 
> ### 何时使用虚拟继承
> 
> 虚拟继承并不是在所有多重继承的场景下都需要使用，通常只有在可能出现菱形继承时才使用虚拟继承。使用虚拟继承的典型场景包括：
> 
> - **菱形继承**：如果一个类通过多个派生类间接继承自同一个基类，使用虚拟继承可以避免数据成员的冗余和函数调用的歧义。
> 
> - **接口继承**：当一个类继承自多个接口（没有数据成员，只有纯虚函数），而这些接口有相同的基类时，虚拟继承可以确保最终派生类中只存在一个基类的子对象。
> 
> 总的来说，虚拟继承是C++中处理复杂多重继承结构的一种强大工具，但它增加了内存布局和调用的复杂性，所以在使用时要权衡利弊。当使用虚拟继承的时候尽量避免在基类中声明数据

It's more practical when virtual base classes have no data.

# 7.Templates and Generic Programming

## Item 41: Understand implicit interfaces and compile-time Polymorphism

> C++ 的模板确实提供了隐式接口和编译时多态的功能，这是模板元编程的核心概念之一。以下是对这两个概念的简要解释：
> 
> ### 1. 隐式接口 (Implicit Interface)
> 在 C++ 模板中，模板类或模板函数的接口是隐式的。也就是说，当你定义一个模板时，并不需要显式地声明模板参数必须具备的接口特性。相反，当你在实例化模板时，编译器会根据模板参数的使用情况，隐式地要求这些参数满足某些接口条件。
> 
> 例如，考虑以下模板函数：
> 
> ```cpp
> template <typename T>
> void print(const T& value) {
>     std::cout << value << std::endl;
> }
> ```
> 
> 这个 `print` 函数模板并没有明确要求 `T` 类型具备任何特定的接口（如 `<<` 操作符），但在实际使用时，编译器会检查 `T` 是否可以使用 `<<` 操作符与 `std::ostream` 进行交互。如果 `T` 不支持这种操作，编译时就会报错。这就是模板的隐式接口。
> 
> ### 2. 编译时多态 (Compile-time Polymorphism)
> C++ 模板通过编译时多态来实现灵活的代码复用。与传统的运行时多态（如虚函数机制）不同，编译时多态是在编译期间确定的，它不依赖于对象的类型，而是依赖于模板参数的类型。
> 
> 编译时多态的核心在于，模板代码会在编译时根据不同的模板参数实例化为不同的代码。每个实例化的模板实际上就是生成了特定于该参数类型的代码版本。由于这个过程发生在编译期，因此可以避免运行时的开销。
> 
> 例如，下面是一个简单的模板类：
> 
> ```cpp
> template <typename T>
> class Box {
> public:
>     void set(const T& value) { data = value; }
>     T get() const { return data; }
> 
> private:
>     T data;
> };
> ```
> 
> 当你使用 `Box<int>` 和 `Box<double>` 时，编译器会分别为 `int` 和 `double` 类型生成不同的 `Box` 类代码，从而实现编译时多态。
> 
> 综上所述，C++ 模板提供了强大的隐式接口和编译时多态机制，使得代码更加灵活和高效，同时也赋予了程序员极大的表达能力。
 
## Item 42: Understand the two meanings of typename

> 在C++模板编程中，理解 `dependent names` 和 `non-dependent names` 是非常重要的，它们决定了模板的解析方式。以下是对这两个概念的解释：
> 
> ### 1. **Dependent Names（依赖名）**
> `Dependent names` 是指那些在模板中依赖于模板参数的名称。它们的解析要等到模板被实例化的时候（即模板参数的具体类型被确定的时候）才进行。这种依赖性可以分为以下几类，嵌套依赖名默认被认为是变量而不是类型，除非声明typename：
> 
> - **Type-dependent names**：依赖于模板参数的类型。例如，如果你有一个类型参数 `T`，那么 `T::type` 或 `T::value_type` 就是 `dependent names`，因为它们取决于 `T` 的具体类型。
>   
> - **Value-dependent names**：依赖于模板参数的值。例如，如果你有一个非类型模板参数 `N`，那么表达式 `N+1` 或 `array[N]` 就是 `dependent names`，因为它们取决于 `N` 的值。
> 
> **示例：**
> ```cpp
> template <typename T>
> void foo(T t) {
>     typename T::type x; // T::type 是一个 dependent name
>     x = t.getValue();   // getValue() 是一个 dependent name，因为它依赖于 T 的类型
> }
> ```
> 
> ### 2. **Non-dependent Names（非依赖名）**
> `Non-dependent names` 是那些不依赖于模板参数的名称。它们在模板定义时立即被解析，不管模板参数是什么。
> 
> **示例：**
> ```cpp
> template <typename T>
> void foo(T t) {
>     int x = 10;    // x 是 non-dependent name
>     std::cout << x << std::endl; // std::cout 和 std::endl 是 non-dependent names
> }
> ```
> 
> ### 3. **相关问题：解析顺序**
> 由于 `dependent names` 的解析被推迟到模板实例化阶段，C++编译器在模板定义时无法确定这些名称的实际含义。这就导致了在某些情况下，需要使用 `typename` 关键字来显式地指明某个名称是一个类型。
> 
> **示例：**
> ```cpp
> template <typename T>
> void foo(T t) {
>     typename T::type x; // typename 告诉编译器 T::type 是一个类型
> }
> ```
> 
> 如果你没有使用 `typename`，编译器可能会认为 `T::type` 是一个变量或静态成员，而不是类型，导致编译错误。
> 
> ### 总结
> - `Dependent names` 依赖于模板参数，它们的解析被推迟到模板实例化阶段。
> - `Non-dependent names` 不依赖于模板参数，它们在模板定义时立即被解析。
> 
> 了解这两者的区别有助于更好地编写和调试C++模板代码。

> 可以通过定义一个结构体（或类），在其中包含一个名为 `type` 的变量或静态成员来演示 `T::type` 作为变量或静态成员的情况。以下是一个示例：
> 
> ```cpp
> #include <iostream>
> 
> // 定义了一个结构体 A，其中包含一个静态成员变量 type
> struct A {
>     static int type;
> };
> 
> // 初始化静态成员变量
> int A::type = 42;
> 
> // 一个简单的模板函数，尝试使用 T::type
> template <typename T>
> void foo(T t) {
>     std::cout << "T::type = " << T::type << std::endl; // 这里 T::type 是一个变量
> }
> 
> int main() {
>     A a;
>     foo(a); // 输出: T::type = 42
>     return 0;
> }
> ```
> 
> ### 解释
> 在这个例子中：
> - `struct A` 包含一个静态成员变量 `type`，类型为 `int`。
> - 模板函数 `foo` 使用了 `T::type`。在这个上下文中，`T::type` 是一个变量，而不是类型。
> - 当我们调用 `foo(a)` 时，由于 `T` 被推导为 `A`，`T::type` 解析为 `A::type`，即一个静态成员变量。
> 
> 由于 `type` 在这个例子中是一个静态成员变量，而不是类型，因此我们不需要使用 `typename` 关键字来声明它。编译器在模板定义时能够立即解析 `T::type` 为一个变量，而不是延迟到实例化时。
> 
> ### 对比
> 如果 `type` 是一个类型而不是变量（如 `typedef` 或 `using`），则必须使用 `typename` 来显式地告诉编译器 `T::type` 是一个类型。否则，编译器会认为 `T::type` 是一个变量或静态成员，这可能导致编译错误。
> 
> ### 额外示例：type 为类型的情况
> ```cpp
> struct B {
>     using type = int;
> };
> 
> template <typename T>
> void foo(T t) {
>     typename T::type x = 10; // 这里 T::type 是一个类型，所以需要 typename
>     std::cout << "x = " << x << std::endl;
> }
> 
> int main() {
>     B b;
>     foo(b); // 输出: x = 10
>     return 0;
> }
> ```
> 
> 在这个示例中，`B::type` 是一个类型别名，因此需要使用 `typename` 来指明 `T::type` 是一个类型。

> 在 C++ 中，成员初始化列表中对 `dependent type` 的使用确实是一个特殊情况，其中不需要使用 `typename` 关键字。这是因为在成员初始化列表中，编译器已经能够确定 `T::type` 是一个类型，即使它是依赖于模板参数的。
> 
> ### 示例：成员初始化列表中的例外情况
> 
> ```cpp
> #include <iostream>
> 
> template <typename T>
> struct Wrapper {
>     typename T::type value;
> 
>     Wrapper(typename T::type val) : value(val) {}  // 在成员初始化列表中不需要使用 typename
> };
> 
> struct Example {
>     using type = int;
> };
> 
> int main() {
>     Example ex;
>     Wrapper<Example> w(10);
>     std::cout << "w.value = " << w.value << std::endl; // 输出: w.value = 10
>     return 0;
> }
> ```
> 
> ### 解释
> 
> 在这个例子中：
> 
> 1. `Wrapper` 是一个模板结构体，接受一个模板参数 `T`。其中的 `value` 成员的类型是 `typename T::type`，这明确了 `T::type` 是一个类型。
> 2. `Wrapper` 的构造函数在成员初始化列表中对 `value` 进行初始化时，使用了 `T::type` 作为类型。在这种情况下，即使 `T::type` 是一个依赖类型，编译器也能够推断出它是类型，因此不需要使用 `typename`。
> 3. `Example` 结构体定义了一个类型别名 `type`，是 `int` 类型。当 `Wrapper<Example>` 实例化时，`T::type` 即 `Example::type` 被解析为 `int`，并用于构造函数的参数和成员初始化列表中。
> 
> ### 总结
> 
> 在成员初始化列表中，C++ 编译器能够自动推断 `T::type` 是一个类型，即使它是依赖于模板参数的。在这种情况下，不需要显式使用 `typename` 关键字。这是 C++ 标准中的一个特例，因为在成员初始化列表中，编译器有足够的信息来解析 `dependent type`，而不需要额外的指示。

## Item 43: Know how to access names in templatized base casses

> 在C++中，如果你继承了一个模板基类，编译器有时可能无法找到基类的成员函数。这是因为基类是一个模板类，编译器在查找成员函数时，**可能不会自动去基类模板中查找**，因为可能有模板特化了某种typename的基类，且这个特化版本中不存在被子类调用的函数，即使非特化模板中存在这样的函数。这个问题通常可以通过以下几种方式来解决：
> 
> ### 1. 使用`this->`来访问基类的成员函数
> 当你在派生类中访问基类的成员函数时，可以使用`this->`来显式地表明你在访问基类的成员函数。例如：
> 
> ```cpp
> template <typename T>
> class Base {
> public:
>     void foo() {
>         // 基类中的函数
>     }
> };
> 
> template <typename T>
> class Derived : public Base<T> {
> public:
>     void bar() {
>         this->foo(); // 使用 this-> 访问基类的成员函数
>     }
> };
> ```
> 
> ### 2. 使用`using`声明
> 你也可以在派生类中使用`using`声明，以便将基类中的函数引入到派生类的作用域中。例如：
> 
> ```cpp
> template <typename T>
> class Base {
> public:
>     void foo() {
>         // 基类中的函数
>     }
> };
> 
> template <typename T>
> class Derived : public Base<T> {
> public:
>     using Base<T>::foo; // 将基类的foo函数引入派生类的作用域
>     void bar() {
>         foo(); // 现在可以直接访问foo
>     }
> };
> ```
> 
> ### 3. 显式地指定基类作用域
> 你可以在调用基类函数时显式地指定基类的作用域。例如：
> 
> ```cpp
> template <typename T>
> class Base {
> public:
>     void foo() {
>         // 基类中的函数
>     }
> };
> 
> template <typename T>
> class Derived : public Base<T> {
> public:
>     void bar() {
>         Base<T>::foo(); // 显式地指定基类的作用域
>     }
> };
> ```
> 
> 这些方法都是为了帮助编译器正确解析基类中的模板成员函数。在继承模板类时，编译器可能无法在派生类中自动找到基类的成员函数，因此需要你显式地指出它们的位置。


## Item 44: Factor parameter-independent code out of templates

Templates are a wonderful way to save time and avoid code replication.

> 在 C++ 中，模板不仅可以接受类型参数，还可以接受非类型参数。非类型模板参数允许你将值传递给模板，而不仅仅是类型。这些非类型参数通常用于常量表达式，比如整数、指针、引用或枚举。
> 
> 以下是一个简单的例子，演示了如何使用非类型模板参数：
> 
> ```cpp
> #include <iostream>
> 
> template<int N>
> void printNumber() {
>     std::cout << "Number: " << N << std::endl;
> }
> 
> int main() {
>     printNumber<5>();   // 输出: Number: 5
>     printNumber<10>();  // 输出: Number: 10
>     return 0;
> }
> ```
> 
> 在这个例子中，`N` 是一个非类型模板参数，它在模板实例化时被指定为一个整数常量。
> 
> 非类型模板参数可以是以下几种类型：
> 1. 整型（包括 `char`, `int`, `long`, `bool` 等等）。
> 2. 指针或引用类型（例如指向对象或函数的指针）。
> 3. `nullptr` 常量。
> 4. 枚举类型。
> 5. std::size_t（通常用于数组大小）。
> 
> 另一个例子展示了使用数组大小作为非类型模板参数：
> 
> ```cpp
> #include <iostream>
> 
> template<typename T, int size>
> class Array {
>     T arr[size];
> public:
>     void set(int index, T value) {
>         if (index >= 0 && index < size) {
>             arr[index] = value;
>         }
>     }
> 
>     T get(int index) const {
>         if (index >= 0 && index < size) {
>             return arr[index];
>         }
>         return T();  // 返回默认值
>     }
> 
>     void print() const {
>         for (int i = 0; i < size; ++i) {
>             std::cout << arr[i] << " ";
>         }
>         std::cout << std::endl;
>     }
> };
> 
> int main() {
>     Array<int, 5> arr;  // 创建一个大小为5的整数数组
>     arr.set(0, 10);
>     arr.set(1, 20);
>     arr.set(2, 30);
>     arr.set(3, 40);
>     arr.set(4, 50);
> 
>     arr.print();  // 输出: 10 20 30 40 50
>     return 0;
> }
> ```
> 
> 在这个例子中，模板参数 `size` 是一个非类型模板参数，它用于指定数组的大小。

> 使用非类型模板参数在实际案例中有多个优势，主要体现在性能优化、类型安全和代码可读性等方面。以下是一些实际案例和其背后的理由。
> 
> ### 1. **编译期优化**
> 非类型模板参数是在编译时决定的，这意味着编译器可以为每个特定的模板实例化生成高度优化的代码。例如，数组大小作为非类型模板参数，可以在编译时知道数组的大小，从而消除运行时检查或动态分配的开销。
> 
> **示例：定长数组的优化**
> ```cpp
> template<int N>
> class FixedArray {
>     int data[N];
> public:
>     int sum() const {
>         int total = 0;
>         for(int i = 0; i < N; ++i) {
>             total += data[i];
>         }
>         return total;
>     }
> };
> ```
> 在这个例子中，数组的大小 `N` 是在编译时已知的，所以编译器可以展开循环，甚至可能进一步优化，产生更高效的代码。
> 
> ### 2. **类型安全**
> 非类型模板参数使得某些信息可以在类型系统中体现出来，从而提供更好的类型安全。例如，矩阵或数组的维度在编译时已知，这可以防止不匹配的操作（例如，尝试加法运算两个维度不同的矩阵）。
> 
> **示例：矩阵乘法**
> ```cpp
> template<int Rows, int Cols>
> class Matrix {
>     float data[Rows][Cols];
> public:
>     // 其他成员函数...
> 
>     // 矩阵乘法（Rows x Cols）乘以（Cols x OtherCols）
>     template<int OtherCols>
>     Matrix<Rows, OtherCols> multiply(const Matrix<Cols, OtherCols>& other) const {
>         Matrix<Rows, OtherCols> result;
>         for (int i = 0; i < Rows; ++i) {
>             for (int j = 0; j < OtherCols; ++j) {
>                 result.data[i][j] = 0;
>                 for (int k = 0; k < Cols; ++k) {
>                     result.data[i][j] += data[i][k] * other.data[k][j];
>                 }
>             }
>         }
>         return result;
>     }
> };
> ```
> 这个例子确保了只有维度匹配的矩阵才能相乘，任何不匹配的操作将在编译时被捕获，而不是在运行时发生错误。
> 
> ### 3. **避免动态分配的开销**
> 如果把非类型参数作为成员函数的参数，那么这些参数的值将需要在运行时传递，这意味着可能需要动态内存分配或者运行时的检查。而使用非类型模板参数则可以将这些信息在编译时固定，减少运行时的复杂性和开销。
> 
> **示例：用于嵌入式系统**
> 嵌入式系统中往往需要非常高效且低开销的代码。使用非类型模板参数可以避免动态分配和运行时计算，直接在编译时确定。
> 
> ### 4. **提高代码可读性和维护性**
> 对于某些情况，通过模板参数明确地表达意图可以提高代码的可读性。例如，使用模板参数明确表达矩阵的维度，可以让代码的使用者一目了然，而不是在函数调用时传递一堆参数。
> 
> ### 总结
> 虽然将非类型参数作为成员函数的参数在某些情况下是可行的，但非类型模板参数的优势在于编译时的确定性、类型安全性、优化的潜力以及在某些场景下避免运行时开销。因此，在需要固定结构或固定尺寸时，非类型模板参数是一种强大且优雅的工具。

> 在C++标准库中，为了减少代码膨胀（code bloat）并提高通用性，有时会使用`void*`类型的指针。这种技术主要用于一些低级别的数据结构或实现细节中，尤其是在实现一些泛型容器时。例如，在实现链表、队列、栈等容器时，使用`void*`可以避免为每种数据类型专门生成代码，减少代码冗余。
> 
> 具体来说，使用`void*`可以让容器类在不关心元素具体类型的情况下进行存储和操作。这样，容器类模板只需要在关键操作中转换`void*`指针，而无需为每种类型都生成单独的代码，从而减少了编译生成的代码量。
> 
> 但是需要注意的是，C++标准库本身并不会直接暴露或使用`void*`作为用户接口，而是通过模板参数化类型来实现泛型编程。标准库的实现可能会在底层使用`void*`来管理内存或实现一些内部机制，但这些细节通常是库的实现细节，用户不需要直接接触。
> 
> 总结来说，`void*`在C++标准库中主要用于内部实现中，通过减少模板实例化的数量来避免代码膨胀，并使得容器类在处理不同类型的数据时更加高效。

## Item 45: Use member function templates to accept "all compatible types"

> 在C++中，**成员模板**（Member Templates）是指在类或结构体中定义的模板函数或模板类成员。它允许我们为类成员函数或嵌套类定义模板，这使得类的成员能够在不同的类型上进行泛型编程。下面详细介绍成员模板的概念、用法及其注意事项。
> 
> ### 1. 成员模板概念
> 
> 成员模板是指可以在类内部定义的模板函数或模板类型。它们允许类的成员具有与模板类型相同的灵活性。这种机制使得一个类可以在其成员函数或嵌套类上实现模板化的功能，而无需将整个类本身模板化。
> 
> ### 2. 成员模板的定义和用法
> 
> #### 2.1 模板成员函数
> 在类中定义模板成员函数的基本语法如下：
> 
> ```cpp
> template <typename T>
> class MyClass {
> public:
>     template <typename U>
>     void myFunction(U param) {
>         // 函数实现
>     }
> };
> ```
> 
> 在这个例子中，`MyClass`是一个非模板类，但它包含了一个模板成员函数`myFunction`。`myFunction`的模板参数类型为`U`，并且可以在函数内部使用`U`类型。
> 
> **示例：**
> 
> ```cpp
> #include <iostream>
> #include <string>
> 
> class MyClass {
> public:
>     template <typename U>
>     void print(U value) {
>         std::cout << value << std::endl;
>     }
> };
> 
> int main() {
>     MyClass obj;
>     obj.print(42);            // 打印整数
>     obj.print(3.14);          // 打印浮点数
>     obj.print("Hello World"); // 打印字符串
>     return 0;
> }
> ```
> 
> 在这个示例中，`MyClass`类的`print`函数是一个模板成员函数，它可以接受任意类型的参数，并将其打印出来。
> 
> #### 2.2 模板嵌套类
> 除了模板成员函数，还可以在类中定义模板嵌套类。
> 
> ```cpp
> template <typename T>
> class MyClass {
> public:
>     template <typename U>
>     class NestedClass {
>     public:
>         void display(U param) {
>             std::cout << param << std::endl;
>         }
>     };
> };
> ```
> 
> 在这个示例中，`MyClass`类中定义了一个模板嵌套类`NestedClass`，它可以接受一个类型`U`，并使用`U`类型的参数。
> 
> ### 3. 成员模板的特性
> 
> 1. **独立性**：成员模板的类型参数与类模板的类型参数是独立的。即使类本身是一个模板类，成员模板的类型参数也不会受到类模板类型参数的影响。
>   
> 2. **模板的灵活性**：成员模板允许类在不同的上下文中使用不同的模板参数，使得类的功能更加灵活和通用。
> 
> 3. **默认模板参数**：在C++11及以后版本中，成员模板也可以有默认的模板参数。
> 
> ### 4. 注意事项
> 
> 1. **访问控制**：成员模板同样受类的访问控制机制（如`public`、`private`、`protected`）的限制。
>   
> 2. **模板实例化**：当调用成员模板时，编译器会根据传入的模板参数进行模板实例化。模板参数可以是基本类型、自定义类型甚至其他模板类型。
> 
> ### 5. 其他相关内容
> 
> - **特化**：可以对成员模板进行特化，但这在实际开发中较为少见，因为成员模板的应用通常需要其泛型特性。
>   
> - **类模板中的成员模板**：在类模板中定义成员模板，这种情况是比较常见的，特别是在实现复杂的泛型数据结构时。
> 
> **示例：**
> 
> ```cpp
> template <typename T>
> class Container {
> public:
>     template <typename U>
>     void add(U element) {
>         // 实现细节
>     }
> };
> ```
> 
> 在上面的例子中，`Container`是一个类模板，而`add`函数是一个成员模板，允许在不同类型的`Container`实例中添加不同类型的元素。
> 
> ### 总结
> 成员模板是C++中强大且灵活的功能，能够在类的成员函数和嵌套类中引入模板机制，从而实现更加通用和可复用的代码。在编写复杂数据结构和算法时，成员模板是一个非常有用的工具。

> 在C++中，**泛化拷贝构造函数**（Generalized Copy Constructor），也称为**模板化拷贝构造函数**，是利用模板机制来定义一个能够处理多种类型的拷贝构造函数。它不仅可以用来复制相同类型的对象，还可以用于复制与原对象类型不同的对象。这个概念通常与类模板或成员模板一起使用。
> 
> ### 1. 泛化拷贝构造函数的基本概念
> 
> 泛化拷贝构造函数是一个模板化的构造函数，它能够接收不同类型的对象，并通过转换或其他手段将这些对象的内容复制到新创建的对象中。这种构造函数通常用于类模板中，以支持跨类型的复制操作。
> 
> ### 2. 泛化拷贝构造函数的定义
> 
> 下面是一个简单的泛化拷贝构造函数的定义示例：
> 
> ```cpp
> template <typename T>
> class MyClass {
> public:
>     T data;
> 
>     // 泛化拷贝构造函数
>     template <typename U>
>     MyClass(const MyClass<U>& other) : data(other.data) {
>         // 其他初始化操作
>     }
> };
> ```
> 
> 在这个例子中，`MyClass`是一个模板类，泛化拷贝构造函数接受另一个类型为`U`的`MyClass`对象，并将其`data`成员复制到新对象中。
> 
> ### 3. 泛化拷贝构造函数的用法
> 
> **示例：**
> 
> ```cpp
> #include <iostream>
> 
> template <typename T>
> class MyClass {
> public:
>     T data;
> 
>     MyClass(T value) : data(value) {}
> 
>     // 泛化拷贝构造函数
>     template <typename U>
>     MyClass(const MyClass<U>& other) : data(other.data) {
>         std::cout << "Generalized copy constructor called" << std::endl;
>     }
> };
> 
> int main() {
>     MyClass<int> obj1(42);          // 创建一个int类型的MyClass对象
>     MyClass<double> obj2(obj1);     // 使用泛化拷贝构造函数复制obj1
> 
>     std::cout << "obj2 data: " << obj2.data << std::endl;
> 
>     return 0;
> }
> ```
> 
> **输出：**
> ```
> Generalized copy constructor called
> obj2 data: 42
> ```
> 
> 在这个示例中，`obj1`是一个`MyClass<int>`类型的对象，而`obj2`是一个`MyClass<double>`类型的对象。泛化拷贝构造函数将`obj1`的`data`复制到`obj2`中，同时输出提示信息。
> 
> ### 4. 泛化拷贝构造函数的应用场景
> 
> - **跨类型复制**：泛化拷贝构造函数允许对象在不同类型之间进行复制，例如从`int`类型的`MyClass`对象复制到`double`类型的`MyClass`对象。
>   
> - **类型转换**：在泛化拷贝构造函数中可以进行类型转换，从而将源对象的类型转换为目标对象的类型。
> 
> - **模板类的增强**：对于模板类，泛化拷贝构造函数增强了类的通用性，使其能够处理更广泛的类型。
> 
> ### 5. 注意事项
> 
> 1. **自动生成的拷贝构造函数**：如果没有定义泛化拷贝构造函数，编译器通常会自动生成一个普通的拷贝构造函数。定义了泛化拷贝构造函数后，默认的拷贝构造函数可能不会被生成，因此在需要保留默认行为的情况下，可能还需要显式定义普通的拷贝构造函数。
> 
> 2. **模板参数匹配**：当泛化拷贝构造函数存在时，它与普通的拷贝构造函数可能会竞争，特别是在模板参数可以相互转换的情况下。编译器会根据模板参数的匹配度来选择最合适的构造函数。
> 
> 3. **效率问题**：在某些情况下，泛化拷贝构造函数可能会引入额外的开销，特别是当涉及到类型转换时。
> 
> ### 6. 其他相关的设计模式
> 
> - **CRTP（Curiously Recurring Template Pattern）**：一种常见的设计模式，利用模板参数来增强类的功能。
> 
> - **SFINAE（Substitution Failure Is Not An Error）**：用于条件选择模板实例化的方法，可以与泛化拷贝构造函数结合使用，以限制其适用范围。
> 
> ### 总结
> 泛化拷贝构造函数是C++中一个强大的特性，它利用模板机制使得构造函数能够在不同类型的对象之间进行拷贝操作。通过使用泛化拷贝构造函数，可以提高代码的复用性和灵活性，同时也能够处理更多样化的类型和场景。在实际开发中，合理使用泛化拷贝构造函数可以简化代码并减少冗余。


> 在C++中，如果你定义了一个模板化的拷贝构造函数（通常是为了支持从不同类型的对象进行拷贝），你仍然可能需要定义一个非模板版本的普通拷贝构造函数，具体取决于你的需求。一般情况下推荐都定义，因为如果不定义普通版本的拷贝构造函数，即使是一样的类型，仍然会通过编译器生成函数。
> 
> **普通拷贝构造函数：**
> - 是一种特定的拷贝构造函数，只有在传入的参数类型完全匹配时才会被调用。它用于从同一类型的对象中拷贝数据。
> 
> **模板拷贝构造函数：**
> - 是一种通用的拷贝构造函数，通常用于从不同类型的对象中拷贝数据，适用于更广泛的情况。
> 
> ### 是否需要定义普通拷贝构造函数：
> 1. **需要定义普通拷贝构造函数的情况：**
>    - 如果你希望类能够使用默认的拷贝行为或自定义的特定拷贝行为（即当源对象和目标对象类型相同时），并且不希望模板化的拷贝构造函数捕获所有情况。
>    - 模板化拷贝构造函数不会阻止编译器生成默认的拷贝构造函数，因此如果你不定义普通版本的拷贝构造函数，编译器仍然会生成一个。
> 
> 2. **不需要定义普通拷贝构造函数的情况：**
>    - 如果你希望模板化的拷贝构造函数处理所有拷贝情况，包括从同类型对象的拷贝。
> 
> ### 举个例子：
> 
> ```cpp
> template <typename T>
> class MyClass {
> public:
>     // 模板拷贝构造函数
>     template <typename U>
>     MyClass(const MyClass<U>& other) {
>         // 处理从不同类型对象的拷贝
>     }
> 
>     // 普通拷贝构造函数
>     MyClass(const MyClass& other) {
>         // 处理从相同类型对象的拷贝
>     }
> };
> ```
> 
> 在这个例子中，普通拷贝构造函数和模板拷贝构造函数可以共存，这样可以根据需要处理不同的拷贝情况。如果你不定义普通拷贝构造函数，模板版本的构造函数将会处理所有类型的拷贝，包括从相同类型的对象拷贝。

## Item 46: Define non-member functions inside templates when type conversions are desired

This item extends the discussion in item 24.
```cpp
template<typename T>
const Rational<T> operator*(const Rational<T>&lhs,const Rational<T>&rhs);

Rational<int> oneHalf(1,2);

Rational<int> result=oneHalf*2;     //error only templatized item 24

//2 don't convert to Rational<int> in template

```

> 在C++中，当使用模板时，模板参数的类型推断（deduction）不会通过构造函数来进行隐式类型转换。 
> 具体来说，当你调用一个模板函数或类模板时，编译器会尝试从你提供的实参来推断模板参数的类型。如果实参的类型与模板参数的类型不完全匹配，而只是能够通过构造函数转换成模板参数的类型，那么编译器不会进行这种转换。
>
> 函数调用的时候是会进行隐式类型转换，但是调用函数前必须存在这个函数，这就必须实例化函数模板，但是实例化函数模板前必须知道参数类型，这就不能进行类型转换了。但是通过友元函数的小技巧能让oneHalf在声明的时候就实例化整个模板类，包括模板类中声明的友元函数 operator*，之后再调用的时候就能进行类型推断了，因为此时函数已经存在了
>
> 例如，假设有以下代码：
> 
> ```cpp
> template<typename T>
> void func(T value) {
>     // 一些操作
> }
> 
> int main() {
>     func(3.14);  // double类型的3.14
> }
> ```
> 
> 在上面的例子中，`func`的模板参数`T`会被推断为`double`，因为你传递了一个`double`类型的值`3.14`。
> 
> 但如果你有一个自定义的类和构造函数，情况可能会有所不同：
> 
> ```cpp
> class MyClass {
> public:
>     MyClass(int) {}
> };
> 
> template<typename T>
> void func(T value) {
>     // 一些操作
> }
> 
> int main() {
>     func(10);  // 传递一个int类型的10
> }
> ```
> 
> 在这个例子中，虽然`MyClass`有一个可以接受`int`类型参数的构造函数，但模板参数推断不会将`int`隐式转换为`MyClass`。编译器会试图直接推断`T`的类型为`int`，而不是`MyClass`。
> 
> 如果你希望使用这种类型的转换，你需要显式地指定模板参数，或者在调用模板函数时显式地创建一个`MyClass`对象：
> 
> ```cpp
> int main() {
>     func(MyClass(10));  // 显式创建MyClass对象
> }
> ```
> 
> 或者：
> 
> ```cpp
> int main() {
>     func<MyClass>(10);  // 显式指定模板参数类型为MyClass
> }
> ```
> 
> 这样做编译器就可以正常推断并编译代码了。


> 通过声明友元函数能使得在实例化类模板的同时，实例化所需要的函数。友元的作用是在类内部定义一个非成员函数，没有friend的话，不可能在类中定义一个非成员函数
> 
> ```cpp
> #include <iostream>
> 
> // 声明一个类模板
> template<typename T>
> class MyClass {
> private:
>     T data;
> 
> public:
>     MyClass(T val) : data(val) {}
> 
>     // 声明一个非模板的友元函数
>     friend void showData(const MyClass& obj) {
>         std::cout << "Data: " << obj.data << std::endl;
>     }
> };
> 
> int main() {
>     MyClass<int> obj1(42);
>     MyClass<double> obj2(3.14);
> 
>     showData(obj1);   // 输出: Data: 42
>     showData(obj2);   // 输出: Data: 3.14
> 
>     return 0;
> }
> ```
> 
> 在这个示例中：
> 
> 1. `MyClass` 是一个类模板，有一个私有成员 `data`。
> 2. `friend void showData(const MyClass& obj);` 是一个非模板的友元函数声明，它直接使用 `MyClass` 的实例类型 `T`。
> 3. 因为友元函数是非模板的，所以它的定义必须在类模板的内部，因为只有类模板才能确定它所使用的类型 `T`。
> 4. 在 `main` 函数中，`MyClass<int>` 和 `MyClass<double>` 的对象都可以调用 `showData` 函数访问其私有成员。
> 
> 这种方式的特点是：
> 
> - 友元函数不需要是模板函数，直接和类模板实例化后的具体类型绑定。
> - `showData` 函数可以在类模板实例化后的任何类型对象上使用，只要函数的定义在类模板内部。
> 
> 因此，使用这种方式时，`showData` 函数会随着 `MyClass` 类模板的每个实例化类型自动生成适当的版本，此时就可以在自动生成的函数版本上使用隐式类型转换

When writing a class template that offer functions relalted to the template that support implicit type conversions on all parameters, **define those functions as friends inside the class template.**

## Item 47: Use traits classes for information about types

> STL（Standard Template Library）中的迭代器（Iterator）是一种用于遍历和操作容器中元素的工具。迭代器可以被看作是指向容器中元素的指针，它们支持类似指针的操作，如解引用（`*`）、递增（`++`）等。
> 
> STL中提供了五种不同类型的迭代器，它们分别支持不同级别的操作：
> 
> 1. **输入迭代器（Input Iterator）**
>     - **特点**：只读迭代器，用于从容器中读取元素。
>     - **功能**：只支持单向遍历（前向遍历），即只能从起始位置向前移动，不能后退。可以进行递增（`++`）操作以及解引用（`*`）操作，但解引用只能进行读取操作，不能修改。
>     - **用途**：适用于只需读取容器中元素且不需要修改元素的场景，例如读取文件数据或从输入流中读取数据。
> 
> 2. **输出迭代器（Output Iterator）**
>     - **特点**：只写迭代器，用于向容器中写入元素。
>     - **功能**：与输入迭代器相反，输出迭代器只支持写操作。只允许对迭代器进行递增（`++`）操作和解引用（`*`）操作，解引用只能用于赋值操作，不能读取容器中已有的数据。
>     - **用途**：适用于向容器中写入数据的场景，如输出流操作或对容器元素进行修改。
> 
> 3. **前向迭代器（Forward Iterator）**
>     - **特点**：既可读也可写的迭代器，支持单向遍历。
>     - **功能**：支持输入和输出迭代器的所有操作，可以进行递增（`++`）操作和解引用（`*`）操作，并且能够多次遍历容器（即可以重复遍历）。
>     - **用途**：适用于需要读写容器中元素且只需前向遍历的场景，如链表等。
> 
> 4. **双向迭代器（Bidirectional Iterator）**
>     - **特点**：可读可写，支持双向遍历的迭代器。
>     - **功能**：除了前向迭代器的所有功能外，还支持递减（`--`）操作，即可以向前和向后遍历容器。
>     - **用途**：适用于需要前后遍历容器的场景，如双向链表、集合等。
> 
> 5. **随机访问迭代器（Random Access Iterator）**
>     - **特点**：最强大的一种迭代器，支持常数时间的任意位置访问。
>     - **功能**：支持所有双向迭代器的功能外，还可以通过加减整数（`+`、`-`）进行跳跃访问，并支持迭代器之间的比较（`<`、`>`、`<=`、`>=`）、加减（`+=`、`-=`）等操作。
>     - **用途**：适用于需要高效随机访问元素的场景，如数组、向量（`vector`）等顺序容器。
> 
> 这五种迭代器按照功能由弱到强依次排列。输入迭代器和输出迭代器最基础，随机访问迭代器功能最强大。每种迭代器的功能是逐步增强的，能满足不同复杂度的需求。

> **Traits Class** 是C++模板编程中的一种技巧，用于提取或推断模板参数的属性或特征，以便在编译时做出决策。Traits Class通过定义模板类（或结构体），并通过模板特化或偏特化，为不同类型提供不同的属性信息或行为。
> 
> ### 主要作用
> 
> 1. **类型信息提取**：Traits Class可以从类型中提取信息，如类型是否为指针、类型的常量性、类型的基础类型等。
> 
> 2. **类型转换**：Traits Class可以用于类型转换，例如在模板中判断某个类型是指针时，可以提取指向的基础类型。
> 
> 3. **编译期分支**：通过Traits Class可以在编译时根据类型特性进行分支选择，从而实现针对不同类型的优化或特定处理。
> 
> ### 典型例子
> 
> 1. **类型判断与修饰：`std::is_same` 和 `std::remove_const`**
> 
>    - `std::is_same`：判断两个类型是否相同。
> 
>    - `std::remove_const`：去除类型的`const`修饰符。
> 
>    这些类型是通过模板特化实现的。例如，`std::is_same`模板的部分实现可能如下：
> 
>    ```cpp
>    template<typename T, typename U>
>    struct is_same {
>        static const bool value = false;
>    };
> 
>    template<typename T>
>    struct is_same<T, T> {
>        static const bool value = true;
>    };
>    ```
> 
>    使用时：
> 
>    ```cpp
>    std::cout << std::is_same<int, int>::value;  // 输出 1 (true)
>    std::cout << std::is_same<int, float>::value; // 输出 0 (false)
>    ```
> 
> 2. **提取基础类型：`std::remove_pointer`**
> 
>    如果需要提取一个指针类型的基础类型，可以使用`std::remove_pointer`：
> 
>    ```cpp
>    template<typename T>
>    struct remove_pointer {
>        using type = T;
>    };
> 
>    template<typename T>
>    struct remove_pointer<T*> {
>        using type = T;
>    };
>    ```
> 
>    使用时：
> 
>    ```cpp
>    using T = std::remove_pointer<int*>::type;  // T 为 int
>    ```
> 
> 3. **数字类型判断：`std::is_integral`**
> 
>    判断一个类型是否为整数类型，可以用`std::is_integral`：
> 
>    ```cpp
>    template<typename T>
>    struct is_integral {
>        static const bool value = false;
>    };
> 
>    template<>
>    struct is_integral<int> {
>        static const bool value = true;
>    };
> 
>    template<>
>    struct is_integral<short> {
>        static const bool value = true;
>    };
>    // 其他整数类型类似
>    ```
> 
>    使用时：
> 
>    ```cpp
>    std::cout << std::is_integral<int>::value;  // 输出 1 (true)
>    std::cout << std::is_integral<float>::value; // 输出 0 (false)
>    ```
> 
> ### 自定义 Traits Class
> 
> 用户可以定义自己的Traits Class来提取或判断类型的某些特性。假设我们有一个Traits Class `is_pointer_like`，用来判断一个类型是否像指针类型（包括实际的指针和智能指针）：
> 
> ```cpp
> template<typename T>
> struct is_pointer_like {
>     static const bool value = false;
> };
> 
> template<typename T>
> struct is_pointer_like<T*> {
>     static const bool value = true;
> };
> 
> template<typename T>
> struct is_pointer_like<std::shared_ptr<T>> {
>     static const bool value = true;
> };
> ```
> 
> 使用时：
> 
> ```cpp
> std::cout << is_pointer_like<int*>::value; // 输出 1 (true)
> std::cout << is_pointer_like<std::shared_ptr<int>>::value; // 输出 1 (true)
> std::cout << is_pointer_like<int>::value; // 输出 0 (false)
> ```
> 
> ### 总结
> 
> Traits Class是C++模板元编程中的核心技术，通过模板的特化、偏特化来提取、判断和操作类型信息，使得模板能够根据类型特性在编译时进行优化和调整。C++标准库中包含了大量的Traits Class，如`std::is_same`、`std::remove_const`、`std::is_integral`等，它们极大地增强了模板编程的灵活性和可维护性。


> 在C++中，STL中的各种迭代器都通过特定的 **traits class** 来提供类型信息。这些traits class通常会用来定义迭代器的类别、迭代器所指向的元素类型等。这些traits class使得算法和容器能够在编译时根据迭代器的类型进行适当的操作和优化。
> 
> iterator_traits是一个单独的类，算法实现时将迭代器具体类型传送给iterator_traits类，萃取类就能返回迭代器的各种具体信息
> 
> 主要的迭代器相关的traits class包括：
> 
> 1. **`iterator_traits`**：主要用于提取迭代器的类型信息。
> 2. **迭代器类别（Iterator Category）**：用来标识迭代器的类型（如输入迭代器、输出迭代器、前向迭代器、双向迭代器、随机访问迭代器）。
> 
> ### `iterator_traits` 的实现
> 
> `iterator_traits` 是一个标准的traits class，用于提取迭代器类型的相关信息，如迭代器的类别（`iterator_category`）、指向的元素类型（`value_type`）、指向的元素的指针类型（`pointer`）、指向的元素的引用类型（`reference`）、以及用于计算距离的类型（`difference_type`）。
> 
> 它通常是通过模板特化实现的，主要包括通用版本和指针类型的特化。
> 
> #### 通用版本
> 
> ```cpp
> template <typename Iterator>
> struct iterator_traits {
>     using iterator_category = typename Iterator::iterator_category;
>     using value_type        = typename Iterator::value_type;
>     using difference_type   = typename Iterator::difference_type;
>     using pointer           = typename Iterator::pointer;
>     using reference         = typename Iterator::reference;
> };
> ```
> 
> 在通用版本中，`iterator_traits`假定传入的类型是一个自定义的迭代器类型，这个类型定义了自己的类型成员，如`iterator_category`等。因此，通过`typename Iterator::iterator_category`来获取这些信息。
> 
> #### 指针类型的特化
> 
> 指针本身也可以作为迭代器使用，所以`iterator_traits`需要为指针类型做特化：
> 
> ```cpp
> template <typename T>
> struct iterator_traits<T*> {
>     using iterator_category = std::random_access_iterator_tag;
>     using value_type        = T;
>     using difference_type   = std::ptrdiff_t;
>     using pointer           = T*;
>     using reference         = T&;
> };
> ```
> 
> 这里，`T*`被认为是一个随机访问迭代器（`std::random_access_iterator_tag`），指向的元素类型为`T`。
> 
> #### 常量指针类型的特化
> 
> 类似地，对于常量指针类型`const T*`也有特化：
> 
> ```cpp
> template <typename T>
> struct iterator_traits<const T*> {
>     using iterator_category = std::random_access_iterator_tag;
>     using value_type        = T;
>     using difference_type   = std::ptrdiff_t;
>     using pointer           = const T*;
>     using reference         = const T&;
> };
> ```
> 
> ### 迭代器类别（Iterator Category）
> 
> C++标准库定义了五种主要的迭代器类别，每种类别对应一个结构体标签：
> 
> - `std::input_iterator_tag`
> - `std::output_iterator_tag`
> - `std::forward_iterator_tag`
> - `std::bidirectional_iterator_tag`
> - `std::random_access_iterator_tag`
> 
> 这些标签通常用于模板特化和重载中，以根据迭代器类别选择正确的算法或操作，因为继承使得能用在 forward_iterator_tag 的地方也能用 bidirectional_iterator_tag，编译阶段匹配最合适的函数来调用的时候就可以更加精细地匹配，例如下面的 advance_impl，编译器就能找到最匹配的实现。例如：
> 
> ```cpp
> struct input_iterator_tag {};
> struct output_iterator_tag {};
> struct forward_iterator_tag : public input_iterator_tag {};
> struct bidirectional_iterator_tag : public forward_iterator_tag {};
> struct random_access_iterator_tag : public bidirectional_iterator_tag {};
> ```
> 
> ### `iterator_traits` 的使用示例
> 
> 在编写泛型算法时，可以通过`iterator_traits`来获取迭代器的类别，并根据类别执行不同的操作。例如：
> 
> ```cpp
> template <typename Iterator>
> void advance(Iterator& it, typename std::iterator_traits<Iterator>::difference_type n) {
>     using category = typename std::iterator_traits<Iterator>::iterator_category; //将具体的迭代器传送给 iterator_traits 这样能够返回具体信息
>     advance_impl(it, n, category());
> }
> 
> template <typename Iterator>
> void advance_impl(Iterator& it, typename std::iterator_traits<Iterator>::difference_type n, std::random_access_iterator_tag) {
>     it += n;  // 对于随机访问迭代器，可以直接进行指针运算
> }
> 
> template <typename Iterator>
> void advance_impl(Iterator& it, typename std::iterator_traits<Iterator>::difference_type n, std::input_iterator_tag) {
>     while (n > 0) {
>         ++it;
>         --n;
>     }
> }
> ```
> 
> 在这个例子中，`advance`函数根据迭代器的类别来选择最合适的实现。例如，如果迭代器是随机访问迭代器，则直接进行指针加法；如果是输入迭代器，则使用循环递增迭代器。
> 
> ### 总结
> 
> `iterator_traits`是C++标准库中一个非常重要的traits class，通过它可以抽象化处理各种迭代器类型，从而编写出适用于不同容器和迭代器的泛型算法。通过特化，可以让`iterator_traits`支持更多类型（如普通指针），进一步提高算法的通用性和效率。
> 

> `deque`（双端队列）是C++ STL中的一种容器，支持在容器的两端高效地插入和删除元素。`deque` 的实现利用了迭代器和 `traits class` 来管理和操作其元素。通过 `traits class`，`deque` 实现能够适应不同类型的迭代器，确保各种操作的正确性和高效性。
> 
> 为了说明 `traits class` 在 `deque` 实现中的工作原理，我们可以关注以下几个方面：
> 
> 1. **迭代器的类型定义和 `iterator_traits` 的使用**：
>    - `deque` 自己定义了一种迭代器类型，该迭代器与 `deque` 的内存布局紧密相关。
>    - `deque` 迭代器依赖 `iterator_traits` 来获取各种类型信息（例如迭代器类别、元素类型、指针类型、引用类型等）。
> 
> 2. **`iterator_category` 的作用**：
>    - 通过 `iterator_category` 来标识 `deque` 迭代器的类型，从而影响算法和操作的选择。
> 
> 3. **`traits class` 在 `deque` 操作中的应用**：
>    - `deque` 中的许多操作，如 `advance`、`distance`、`copy` 等，都依赖 `traits class` 来优化或确保对不同类型迭代器的正确操作。
> 
> ### `deque` 的迭代器实现
> 
> `deque` 使用一种随机访问迭代器，这种迭代器类似于普通指针，支持常数时间的任意位置访问。
> 
> ```cpp
> //自己的实现中要提供 iterator_traits 需要的各种信息
> template <typename T, typename Ref, typename Ptr>
> struct deque_iterator {
>     using iterator_category = std::random_access_iterator_tag;
>     using value_type        = T;
>     using difference_type   = std::ptrdiff_t;
>     using pointer           = Ptr;
>     using reference         = Ref;
> 
>     // 迭代器成员变量，如当前元素的指针、控制块指针等
>     // ...
> 
>     // 迭代器操作，例如 ++, --, +, -, * 解引用等
>     // ...
> };
> ```
> 
> - **`iterator_category`**：声明了这是一个随机访问迭代器。
> - **`value_type`**：表示迭代器所指向的元素类型。
> - **`difference_type`**：用于表示两个迭代器之间的距离。
> - **`pointer` 和 `reference`**：分别表示指针类型和引用类型。
> 
> ### `iterator_traits` 的使用
> 
> 在泛型算法中，我们通常使用 `iterator_traits` 来提取迭代器的类型信息。假设我们有一个泛型函数 `distance`，用来计算两个迭代器之间的距离：
> 
> ```cpp
> template <typename Iterator>
> typename std::iterator_traits<Iterator>::difference_type
> distance(Iterator first, Iterator last) {
>     using category = typename std::iterator_traits<Iterator>::iterator_category;
>     return distance_impl(first, last, category());
> }
> 
> template <typename RandomAccessIterator>
> typename std::iterator_traits<RandomAccessIterator>::difference_type
> distance_impl(RandomAccessIterator first, RandomAccessIterator last, std::random_access_iterator_tag) {
>     return last - first;  // 对于随机访问迭代器，直接使用指针算术
> }
> 
> template <typename InputIterator>
> typename std::iterator_traits<InputIterator>::difference_type
> distance_impl(InputIterator first, InputIterator last, std::input_iterator_tag) {
>     typename std::iterator_traits<InputIterator>::difference_type n = 0;
>     while (first != last) {
>         ++first;
>         ++n;
>     }
>     return n;
> }
> ```
> 
> 在这个例子中，`distance` 函数根据迭代器的类型（通过 `iterator_traits` 提取 `iterator_category`）选择合适的实现方式。对于 `deque` 迭代器，由于它是随机访问迭代器，`distance` 会选择第一个 `distance_impl`，直接使用指针算术进行高效的距离计算。
> 
> ### `deque` 的迭代器与 `traits class` 的结合
> 
> 在 `deque` 的实现中，许多操作都需要与迭代器类型交互。`traits class` 提供了一种统一的方式来提取这些类型信息，从而简化了代码的实现，并确保泛型算法能够正确处理不同类型的迭代器。
> 
> 例如，当我们在 `deque` 中进行元素拷贝时，可能会使用类似于 `std::copy` 的算法：
> 
> ```cpp
> template <typename InputIterator, typename OutputIterator>
> OutputIterator copy(InputIterator first, InputIterator last, OutputIterator result) {
>     using category = typename std::iterator_traits<InputIterator>::iterator_category;
>     return copy_impl(first, last, result, category());
> }
> 
> template <typename RandomAccessIterator, typename OutputIterator>
> OutputIterator copy_impl(RandomAccessIterator first, RandomAccessIterator last, OutputIterator result, std::random_access_iterator_tag) {
>     auto n = last - first;  // 高效的距离计算
>     while (n-- > 0) {
>         *result = *first;
>         ++first;
>         ++result;
>     }
>     return result;
> }
> 
> template <typename InputIterator, typename OutputIterator>
> OutputIterator copy_impl(InputIterator first, InputIterator last, OutputIterator result, std::input_iterator_tag) {
>     while (first != last) {
>         *result = *first;
>         ++first;
>         ++result;
>     }
>     return result;
> }
> ```
> 
> 在这个例子中，`copy` 函数根据迭代器类别选择最合适的实现方式。对于 `deque` 的迭代器，`copy` 函数将使用针对随机访问迭代器的实现方式，这样可以利用指针算术实现高效拷贝。
> 
> ### 总结
> 
> 在 `deque` 的实现中，`traits class`，特别是 `iterator_traits`，起到了关键作用。通过 `iterator_traits`，`deque` 能够在编译时获取迭代器的相关类型信息，并根据这些信息选择最合适的算法和操作方式。这种设计使得 `deque` 和其他 STL 容器能够支持多种不同类型的迭代器，并且确保操作的正确性和高效性。

![iterator_traits](iterator_traits.png)


> 是的，理论上在算法实现中，可以直接使用迭代器本身提供的类型，而不依赖于 `iterator_traits`。但是，`iterator_traits` 的引入有其重要的意义和优势，尤其是在泛型编程中。
> 
> ### 直接使用迭代器提供的类型
> 
> 如果迭代器本身已经定义了相关的类型（如 `value_type`、`iterator_category` 等），你可以直接在算法中使用这些类型。例如：
> 
> ```cpp
> template <typename Iterator>
> typename Iterator::difference_type distance(Iterator first, Iterator last) {
>     typename Iterator::difference_type n = 0;
>     while (first != last) {
>         ++first;
>         ++n;
>     }
>     return n;
> }
> ```
> 
> ### 这样做的局限性
> 
> 1. **通用性不足**：某些类型，比如原生指针（`T*`），并不定义这些类型成员。如果你直接使用迭代器的类型，而不是通过 `iterator_traits`，这些指针类型将无法正常工作。例如，`int*` 没有 `difference_type` 或 `iterator_category` 等成员类型：
> 
>    ```cpp
>    int arr[] = {1, 2, 3};
>    int* first = arr;
>    int* last = arr + 3;
>    auto dist = distance(first, last);  // 如果 distance 直接使用 Iterator::difference_type，会编译失败
>    ```
> 
>    使用 `iterator_traits`，可以通过模板特化为指针类型定义这些成员，从而解决这个问题。
> 
> 2. **一致性和代码维护**：`iterator_traits` 提供了一种一致的方式来访问迭代器的类型信息。它统一了不同类型迭代器（包括标准库迭代器、指针、用户自定义迭代器等）的接口，使得编写泛型算法更简单、更一致。
> 
> 3. **元编程能力**：`iterator_traits` 可以通过模板特化为特定类型提供不同的实现，从而增强了C++元编程的能力。这意味着你可以为不同类型的迭代器提供特化版本，进行额外的优化或定制，而不必修改算法的主要逻辑。
> 
> 4. **兼容性与扩展性**：标准库及用户定义的迭代器都遵循 `iterator_traits` 的接口标准，使得新的迭代器类型能够无缝集成到现有的算法中，而不需要更改这些算法。
> 
> ### 通过 `iterator_traits` 的实现
> 
> 通过使用 `iterator_traits`，你可以确保算法能处理更广泛的迭代器类型，提供更好的通用性和扩展性：
> 
> ```cpp
> template <typename Iterator>
> typename std::iterator_traits<Iterator>::difference_type distance(Iterator first, Iterator last) {
>     typename std::iterator_traits<Iterator>::difference_type n = 0;
>     while (first != last) {
>         ++first;
>         ++n;
>     }
>     return n;
> }
> ```
> 
> 在这个例子中，即使 `Iterator` 是指针，`iterator_traits` 也会提供正确的 `difference_type` 和其他类型信息。
> 
> ### 总结
> 
> 虽然在某些简单场景下，直接使用迭代器提供的类型可能足够，但使用 `iterator_traits` 带来了更强的通用性、一致性和灵活性。`iterator_traits` 是泛型编程中的重要工具，能够帮助你编写更健壮、更通用的算法，从而更好地处理不同类型的迭代器。

## Item 48: Be aware of template metaprogramming

> 模板元编程（Template Metaprogramming，简称 TMP）是 C++ 中一种高级编程技术，利用模板在编译期执行计算。它主要基于 C++ 的模板机制，允许开发者编写在编译时生成和优化代码的程序。TMP 通过静态多态性、递归和类型推导，能够实现类似于函数编程中的计算过程，但这些计算是在编译阶段完成的。
> 
> 以下是模板元编程的几个核心概念和技术点：
> 
> ### 1. **模板的基础**
> 模板元编程的基础是 C++ 的模板机制，它允许编写参数化的代码。在模板中，类型（类模板）或值（函数模板）可以作为参数传递。
> 
> ```cpp
> template <typename T>
> T add(T a, T b) {
>     return a + b;
> }
> ```
> 
> 上面的 `add` 函数是一个普通的函数模板，它接受任意类型 `T`，并在运行时执行加法。
> 
> ### 2. **编译期计算**
> 与普通的函数模板不同，TMP 利用模板在编译期间进行计算。例如，可以利用模板递归进行常量的计算：
> 
> ```cpp
> template <int N>
> struct Factorial {
>     static const int value = N * Factorial<N - 1>::value;
> };
> 
> // 基本情况，0 的阶乘为 1
> template <>
> struct Factorial<0> {
>     static const int value = 1;
> };
> 
> int main() {
>     int result = Factorial<5>::value;  // result = 120
> }
> ```
> 
> 在上面的例子中，`Factorial` 是一个递归的模板结构体，它在编译期计算阶乘。编译时，编译器会将 `Factorial<5>` 展开为一个常量值 `120`。
> 
> ### 3. **类型推导与递归**
> TMP 允许你在编译期间推导类型，常见的应用是类型转换、类型计算和类型萃取（traits）。一个典型的例子是编写一个获取类型大小的模板：
> 
> ```cpp
> template <typename T>
> struct TypeSize {
>     static const int value = sizeof(T);
> };
> 
> int main() {
>     int size = TypeSize<int>::value;  // size = 4 (假设 int 是 4 字节)
> }
> ```
> 
> 类型递归是 TMP 中的关键技巧，它通过递归推导不同的模板实例来实现更复杂的类型计算。
> 
> ### 4. **SFINAE（替换失败不是错误）**
> SFINAE（Substitution Failure Is Not An Error）是 TMP 中的一个重要原则。当模板的某些参数替换失败时，编译器不会报错，而是会尝试其他模板的替换。这使得 TMP 可以实现一些复杂的编译期条件判断和函数重载选择。
> 
> 例如，使用 SFINAE 实现某个类型是否有某个成员函数的判断：
> 
> ```cpp
> template <typename T>
> class has_toString {
>     template <typename U>
>     static auto check(int) -> decltype(std::declval<U>().toString(), std::true_type());
> 
>     template <typename>
>     static std::false_type check(...);
> 
> public:
>     static const bool value = decltype(check<T>(0))::value;
> };
> ```
> 
> 上面的例子利用 SFINAE 技术，在编译期判断某个类型 `T` 是否有 `toString` 成员函数。
> 
> ### 5. **类型萃取 (Type Traits)**
> 类型萃取是 TMP 的一个重要应用，用来在编译期分析和操作类型。C++ 标准库提供了一系列类型萃取工具，例如 `std::is_integral`、`std::is_same` 等，用于判断类型是否是整数类型或两个类型是否相同。
> 
> 自定义一个简单的类型萃取：
> 
> ```cpp
> template <typename T>
> struct is_pointer {
>     static const bool value = false;
> };
> 
> template <typename T>
> struct is_pointer<T*> {
>     static const bool value = true;
> };
> 
> int main() {
>     bool isIntPtr = is_pointer<int*>::value;  // true
>     bool isInt = is_pointer<int>::value;      // false
> }
> ```
> 
> ### 6. **递归与条件表达式**
> 在 TMP 中，可以用递归和条件表达式来实现逻辑运算。C++11 引入了 `std::conditional` 使得条件选择更为方便。
> 
> ```cpp
> template <bool Condition, typename TrueType, typename FalseType>
> struct conditional {
>     typedef TrueType type;
> };
> 
> template <typename TrueType, typename FalseType>
> struct conditional<false, TrueType, FalseType> {
>     typedef FalseType type;
> };
> 
> int main() {
>     using type = conditional<true, int, double>::type;  // type 是 int
> }
> ```
> 
> ### 7. **编译时的性能优化**
> 由于 TMP 发生在编译阶段，因此其主要目的是优化运行时性能。例如，编译器可以根据 TMP 的结果生成最优的机器代码，从而减少运行时的计算负担。
> 
> ### 8. **局限性与复杂度**
> 虽然 TMP 非常强大，但也带来了代码可读性和调试的复杂性。TMP 的错误信息通常非常冗长，且由于所有计算在编译时完成，可能会显著增加编译时间。
> 
> ### 总结
> 模板元编程是 C++ 中一个非常强大的编译期编程技术，它可以实现编译期的计算、类型推导和条件编译，还能够进行更早期的错误检测，极大地提高了代码的灵活性和性能。然而，TMP 也带来了较高的复杂度，因此在实际编写 TMP 代码时，需要权衡可读性和性能之间的关系。


> 在 C++ 中，`typeid` 是一个用于在运行时获取类型信息的操作符，它属于 C++ 的 RTTI（运行时类型识别）功能。使用 `typeid` 操作符可以获得某个对象或类型的类型信息，并返回一个 `type_info` 对象。`type_info` 是一个标准库中的类，用于表示类型信息。
> 
> `typeid` 的典型用法有以下几种：
> 
> ### 1. 对象的类型
> 你可以通过 `typeid` 来获取一个对象的类型。例如：
> 
> ```cpp
> #include <iostream>
> #include <typeinfo>
> 
> class Base {};
> class Derived : public Base {};
> 
> int main() {
>     Base b;
>     Derived d;
> 
>     std::cout << "Type of b: " << typeid(b).name() << std::endl;
>     std::cout << "Type of d: " << typeid(d).name() << std::endl;
> 
>     return 0;
> }
> ```
> 
> 输出可能是：
> 
> ```
> Type of b: 4Base
> Type of d: 7Derived
> ```
> 
> 注意，`typeid(b).name()` 返回的是编译器定义的类型名称，因此不同编译器可能会有不同的结果。某些编译器输出的并不是直接的类名，而是经过某种编码的名称。
> 
> ### 2. 使用多态类型
> `typeid` 对于多态类型（即具有虚函数的类）尤其有用。它能在运行时识别出对象的真实类型，而不仅仅是指针或引用的静态类型：
> 
> ```cpp
> #include <iostream>
> #include <typeinfo>
> 
> class Base {
> public:
>     virtual ~Base() {}
> };
> 
> class Derived : public Base {};
> 
> int main() {
>     Base* pBase = new Derived();
> 
>     std::cout << "Type of *pBase: " << typeid(*pBase).name() << std::endl;
> 
>     delete pBase;
>     return 0;
> }
> ```
> 
> 在这个例子中，由于 `Base` 类有虚函数，`typeid(*pBase)` 将返回 `Derived` 类型的信息，而不是 `Base`。
> 
> ### 3. 原始类型
> 你还可以使用 `typeid` 来获取原始类型的类型信息：
> 
> ```cpp
> #include <iostream>
> #include <typeinfo>
> 
> int main() {
>     int i = 42;
>     double d = 3.14;
> 
>     std::cout << "Type of i: " << typeid(i).name() << std::endl;
>     std::cout << "Type of d: " << typeid(d).name() << std::endl;
> 
>     return 0;
> }
> ```
> 
> ### 注意事项
> - `typeid` 的使用要求类型必须已知。例如，不能对不完全类型（forward declaration）使用 `typeid`。
> - 对于没有虚函数的非多态类，`typeid` 的结果是编译时已知的，而不会在运行时计算。
> - 使用 `typeid` 需要包含头文件 `<typeinfo>`。
> 
> 总结起来，`typeid` 在 C++ 中用于获取类型信息，尤其在处理多态类型时，它能够帮助识别对象的实际类型。

> C++ 编译器确实要求所有的源代码都是有效的，即使某些代码永远不会被执行。具体来说，这意味着即使某段代码由于逻辑分支、条件或宏定义等原因不会在程序运行时执行，编译器仍然会对该代码进行语法检查、类型检查等。这种行为是为了确保代码的完整性和一致性，从而避免潜在的错误。
> 
> ### 为什么 C++ 要求所有源代码有效？
> 
> 这是因为 C++ 是一个静态类型检查的编译语言，编译器需要在编译阶段检查整个代码库的合法性，才能生成可执行的机器代码。在这个过程中，编译器不会忽略任何部分的代码，即便它们不会在程序运行时执行，原因包括：
> 
> 1. **类型安全**：编译器在编译时会对每一段代码进行类型检查，以确保没有类型不匹配或非法操作。这种检查帮助程序员在编写代码时及早发现问题，即便这些代码在运行时从未执行过。
> 
> 2. **模板和宏的潜在使用**：C++ 中广泛使用模板和宏，它们的扩展往往取决于编译期的类型推导和条件。如果某些模板函数或宏在某个场景下可能会用到，编译器必须确保它们的定义是有效的。
> 
> 3. **潜在的可执行路径**：即使某段代码当前不会被执行，可能由于未来修改或代码重构，它将来会被激活执行。编译器的检查可以确保代码在这种情况下仍然是合法的。
> 
> ### 示例
> 
> 假设你有以下代码片段，其中包含一个不会被执行的分支：
> 
> ```cpp
> int main() {
>     if (false) {
>         int x = "this is a string";  // 错误: 不允许将字符串赋值给整数
>     }
>     return 0;
> }
> ```
> 
> 尽管 `if (false)` 保证了代码永远不会执行，但编译器仍然会对 `int x = "this is a string";` 进行类型检查，并报告类型不匹配的错误。这是因为编译器要求源代码中的每个部分都是有效的，即使这些部分在运行时不会执行。
> 
> ### 编译期和运行期的区分
> 
> C++ 编译器执行两种主要的检查：
> 
> - **编译期检查**：编译器在编译代码时会检查语法、类型等错误。即使某些代码不会被执行，只要代码是可达的，编译器都会进行检查。
>   
> - **运行期检查**：某些逻辑错误和异常只有在运行时才能捕捉到。然而，编译器并不保证检查这些错误，比如除以零或数组越界。这类错误需要程序在运行时处理。
> 
> ### 特例：条件编译
> 
> 通过条件编译（如使用预处理指令 `#ifdef` 或 `#ifndef`），某些代码可以在编译时被完全忽略：
> 
> ```cpp
> #ifdef ENABLE_FEATURE
> void feature() {
>     // 这个代码块在 ENABLE_FEATURE 未定义时不会被编译
> }
> #endif
> ```
> 
> 在这种情况下，未定义 `ENABLE_FEATURE` 时，`feature` 函数的代码会被预处理器完全排除在编译之外，因此不会被检查。
> 
> ### 总结
> 
> C++ 编译器要求所有源代码在编译时都是有效的，即使某些代码在运行时永远不会执行。这种设计是为了保证代码的完整性，减少潜在的错误，提高类型安全性和代码健壮性。


> C++中的模板元编程（Template Metaprogramming，TMP）是图灵完备的。模板元编程可以执行任意复杂的计算，并且能够模拟条件语句、循环等编程结构，理论上可以用来实现任何图灵机能做到的计算。图灵完备意味着该编程模型可以模拟图灵机，具备通用的计算能力。
> 
> 在C++模板元编程中，编译器会在编译时对模板进行展开与计算，这使得一些计算可以在编译时完成，而不是在运行时。因此，TMP是一种编译期编程技术，其操作对象是类型、常量等，而不是运行时的具体值。
> 
> C++模板元编程的图灵完备性主要体现在：
> 1. **递归**：可以通过模板的递归定义实现循环。
> 2. **条件判断**：可以通过模板特化（template specialization）实现类似于条件分支的功能。
> 3. **自定义数据结构**：通过模板参数列表，可以模拟链表等数据结构。
> 
> 由于这些特性，模板元编程具有极大的灵活性和强大的计算能力，但也可能导致编译器过度复杂的错误信息以及较长的编译时间。


# 8.Customizing new and delete

## Item 49: Understand the behavior of the new-handler

> c++中的new执行:分配内存(operator new) => static_cast<type> 转成类指针类型 => 用这个指针调用构造函数
> c++中的delete执行:先调用析构函数，再用指针调用operator delete


> 在 C++ 的多线程环境中，堆（heap）和 `new-handler` 作为全局资源的确可能会引发一些问题，尤其是它们的非线程安全特性。
> 
> ### 1. 堆 (Heap) 管理问题：
> C++ 动态内存分配使用堆，在多线程环境中，多个线程可能同时请求内存分配和释放。如果内存分配器未加锁保护，则可能会导致竞争条件，最终引发内存损坏、程序崩溃或未定义行为。
> 
> #### 解决方案：
> - **使用线程安全的内存分配器**：现代的 C++ 标准库实现大多数都提供了线程安全的内存分配器，这会自动在多线程环境中管理堆操作。
> - **自定义内存分配器**：在需要优化多线程环境下的内存管理时，可以实现自己的线程安全内存分配器。
> - **锁保护**：在特定情况下，手动使用锁（如 `std::mutex`）保护内存分配和释放操作。
> 
> ### 2. `new-handler` 问题：
> `new-handler` 是一个全局函数指针，当 `new` 操作无法分配内存时会调用这个处理程序。由于 `new-handler` 是全局资源，在多线程环境中，如果多个线程同时尝试设置或修改 `new-handler`，可能会导致未定义行为。
> 
> #### 解决方案：
> - **避免频繁修改 `new-handler`**：在多线程程序中，尽量避免频繁修改全局的 `new-handler`。一般只需在程序初始化时设置一次。
> - **锁保护**：如果确实需要在运行过程中修改 `new-handler`，可以使用锁（如 `std::mutex`）来保护修改操作，确保线程安全。
>   
> ### 总结
> C++ 标准库已经在很大程度上解决了多线程环境下的内存管理问题，但对于一些特定的场景，如自定义内存分配器或频繁修改 `new-handler`，仍需开发者自己确保线程安全。

> 在 C++ 中，`operator new` 和 `operator delete` 确实是针对单个对象的内存分配和释放操作，而 `operator new[]` 和 `operator delete[]` 是专门用于数组对象的内存分配和释放。虽然它们看似相似，但它们实际上是不同的函数，承担不同的职责。
> 
> ### 区别如下：
> 
> 1. **`operator new` 和 `operator delete`**：
>    - 负责单个对象的内存分配和释放。
>    - 当你使用 `new` 来创建一个对象时，会调用 `operator new` 来分配足够的内存。
>    - 对象的析构函数被调用后，`operator delete` 负责释放这块内存。
> 
>    示例：
>    ```cpp
>    int* p = new int;     // 使用 operator new 分配内存
>    delete p;             // 使用 operator delete 释放内存
>    ```
> 
> 2. **`operator new[]` 和 `operator delete[]`**：
>    - 负责数组对象的内存分配和释放。
>    - 当你使用 `new[]` 创建一个对象数组时，会调用 `operator new[]` 来分配足够的内存。
>    - 当调用 `delete[]` 时，会调用 `operator delete[]` 来释放内存。
> 
>    示例：
>    ```cpp
>    int* arr = new int[10];  // 使用 operator new[] 分配内存
>    delete[] arr;            // 使用 operator delete[] 释放内存
>    ```
> 
> ### 重要注意点：
> - `operator new` 和 `operator new[]` 是两个不同的函数。它们在标准库中可以被重载，但它们的行为和用途不同。
> - 同样，`operator delete` 和 `operator delete[]` 也不同，它们分别用于释放单个对象和数组对象的内存。
> - 如果你混用 `new` 和 `delete[]`（或者 `new[]` 和 `delete`），会导致未定义行为，可能会引发程序崩溃或内存泄漏。
> 
> 因此，必须匹配使用 `new` 与 `delete`，`new[]` 与 `delete[]`，否则会导致严重的内存管理问题。

> 在 C++ 标准模板库（STL）中，所有容器的内存分配都是通过一个名为 `allocator` 的对象来管理的。`allocator` 是 C++ 中的一种内存分配器，允许容器灵活地控制内存的分配和释放。
> 
> ### `allocator` 的作用：
> - **内存分配和释放**：`allocator` 负责从自由存储区（通常是堆）分配内存给容器，同时在容器析构时释放这部分内存。
> - **对象构造与析构**：`allocator` 不仅仅分配内存，还负责在分配的内存上构造对象和析构对象，这确保了对象的正确初始化和销毁。
> 
> ### STL 容器如何使用 `allocator`：
> 1. **默认的 `allocator`**：
>    - 所有的 STL 容器默认使用 `std::allocator<T>` 来进行内存管理。例如，`std::vector<int>` 使用的是 `std::allocator<int>`，这也是最常用的内存分配器，它直接使用 `new` 和 `delete` 来分配和释放内存。
> 
> 2. **自定义 `allocator`**：
>    - STL 容器支持用户自定义 `allocator`。这使得开发者可以实现自己的内存分配策略，而不是依赖默认的 `std::allocator`。比如，如果你有一种优化的内存池，可以通过自定义 `allocator` 来提升内存管理的效率。
> 
> ### `allocator` 的基本接口：
> `allocator` 提供了一些必要的函数来实现内存管理。以下是 `std::allocator` 的一些关键函数：
> - `allocate()`：分配一块原始的未初始化内存。
> - `deallocate()`：释放由 `allocate()` 分配的内存。
> - `construct()`：在已分配的内存上构造对象。
> - `destroy()`：调用对象的析构函数，销毁对象。
> 
> ### 示例：
> 以下是一个简单的例子，展示了 `std::allocator` 的基本用法：
> 
> ```cpp
> #include <iostream>
> #include <memory>  // 引入 std::allocator
> 
> int main() {
>     std::allocator<int> alloc;   // 创建一个 allocator 对象
>     int* p = alloc.allocate(5);  // 分配内存给 5 个 int 对象
> 
>     // 在分配的内存上构造对象
>     for (int i = 0; i < 5; ++i) {
>         alloc.construct(p + i, i + 1);
>     }
> 
>     // 输出构造的对象
>     for (int i = 0; i < 5; ++i) {
>         std::cout << p[i] << " ";
>     }
>     std::cout << std::endl;
> 
>     // 销毁对象并释放内存
>     for (int i = 0; i < 5; ++i) {
>         alloc.destroy(p + i);
>     }
>     alloc.deallocate(p, 5);
> 
>     return 0;
> }
> ```
> 
> ### STL 容器中的 `allocator`：
> 几乎所有的 STL 容器都支持 `allocator`，如：
> - `std::vector`
> - `std::list`
> - `std::map`
> - `std::set`
> - `std::deque`
> 
> 每个容器内部都会有一个 `allocator` 对象，管理其元素的内存分配和释放工作。
> 
> 总结来说，`allocator` 是 STL 容器内存管理的重要组成部分，默认的 `std::allocator` 提供了合理的内存分配方式，但用户可以根据需要自定义 `allocator` 来优化内存使用。


> 在C++中，当使用`operator new`进行内存分配时，如果无法分配到足够的内存，默认情况下会抛出一个`std::bad_alloc`异常。然而，C++允许开发者通过`set_new_handler`函数来自定义内存分配失败时的处理方式。
> 
> ### 具体行为
> 
> 1. **默认情况下的行为**:
>    - 当`operator new`无法分配到足够的内存时，它会抛出一个`std::bad_alloc`异常。这通常表示系统内存不足，无法满足程序的内存请求。
>    - 抛出`std::bad_alloc`后，程序可以捕获该异常并决定如何处理，例如释放一些资源，减少内存消耗，或者通知用户。
> 
> 2. **使用`set_new_handler`的情况**:
>    C++允许程序员通过`std::set_new_handler`函数指定一个自定义的处理函数，当`operator new`无法分配到内存时会调用这个函数，而不是立即抛出`std::bad_alloc`。
> 
>    - `set_new_handler`的签名如下：
>      ```cpp
>      typedef void (*new_handler)() new_handler; //返回值为void,参数列表是()的函数指针 
>      std::new_handler set_new_handler(std::new_handler new_p) noexcept;
>      ```
>      这里的`new_p`是一个指向无返回值、无参数的函数指针，set_new_handler 的返回值是原来的 handler，如果原来没有的话，返回的是 nullptr
> 
>    - 如果调用了`set_new_handler`来设置自定义的处理函数，当内存分配失败时，`operator new`会调用这个处理函数。处理函数可以尝试进行一些操作，例如：
>      - 释放内存或其他资源。
>      - 记录日志或提供错误提示。
>      - 甚至可以通过某种策略降低程序的内存需求。
> 当operator new 分配内存失败时，可以调用 handler 函数，然后再次尝试分配内存，如果仍然不行就再次调用 handler，这是一个死循环，因此要合理设计 handler，要么能分配更多的空间以满足 new 的需求，要么就抛出异常。如果一开始就没有设定 handler，无法分配足够的空间时就抛出异常
> 
> ### 代码示例
> 
> 以下是一个使用`set_new_handler`的简单例子：
> 
> ```cpp
> #include <iostream>
> #include <new>
> 
> void my_new_handler() {
>     std::cerr << "Memory allocation failed, trying to free resources..." << std::endl;
>     // 尝试释放一些资源，例如删除缓存等
>     // 如果无法解决问题，抛出异常或终止程序
>     throw std::bad_alloc();  // 这里重新抛出异常
> }
> 
> int main() {
>     std::set_new_handler(my_new_handler);  // 设置自定义的处理函数
> 
>     try {
>         // 尝试分配大量内存
>         int* p = new int[100000000000];
>     } catch (const std::bad_alloc& e) {
>         std::cerr << "Caught exception: " << e.what() << std::endl;
>     }
> 
>     return 0;
> }
> ```
> 
> 在这个例子中，当内存分配失败时，`my_new_handler`会被调用，试图进行一些处理。如果问题无法解决，它会抛出`std::bad_alloc`，并在程序中捕获该异常。
> 
> ### 总结
> 
> - 默认情况下，`operator new`在内存分配失败时会抛出`std::bad_alloc`异常。
> - 使用`set_new_handler`可以自定义内存分配失败时的处理逻辑，提供机会释放资源或采取其他恢复措施。
> - 如果自定义的`new_handler`无法处理内存不足的问题，可以抛出`std::bad_alloc`异常或者终止程序。

A well-designed new-handler function must do one of the following:
- Make more memory available
- Install a different new-handler
- Deinstall the new-handler
- Throw an exception
- Not return (`abort` or `exit`)

> 在C++中，可以针对**全局**设置`set_new_handler`，即当程序中任何地方调用`operator new`进行内存分配时，都会使用已设置的处理程序。然而，C++并没有直接为**特定的类**提供内建机制来指定`set_new_handler`。不过，开发者可以通过一些编程技巧，在类级别实现自定义的内存分配失败处理机制。
> 
> 可以通过为特定的类重载`operator new`，并在该重载函数中调用特定的`new_handler`，从而实现针对某个类的自定义内存分配失败处理。
> 
> ### 针对特定类实现`set_new_handler`
> 
> 要实现针对某个类的`set_new_handler`功能，我们可以在类中进行如下步骤：
> 
> 1. **重载`operator new`**: 为特定类重载`operator new`，从而控制该类的内存分配过程。
> 2. **管理类自己的`new_handler`**: 在类中提供静态函数来设置和获取类特有的`new_handler`。
> 3. **调用类特定的`new_handler`**: 在重载的`operator new`中，调用类的`new_handler`以处理内存分配失败的情况。
> 
> ### 代码示例
> 
> 以下是一个针对特定类实现自定义`new_handler`的示例：
> 
> ```cpp
> #include <iostream>
> #include <new>
> 
> class MyClass {
> public:
>     // 类级别的new_handler
>     static std::new_handler set_new_handler(std::new_handler p) noexcept {
>         std::new_handler oldHandler = currentHandler;
>         currentHandler = p;
>         return oldHandler;
>     }
> 
>     // 重载operator new
>     static void* operator new(std::size_t size) {
>         std::new_handler globalHandler = std::set_new_handler(currentHandler);  // 使用类的handler
>         void* memory;
>         try {
>             memory = ::operator new(size);  // 尝试分配内存
>         } catch (const std::bad_alloc&) {
>             std::set_new_handler(globalHandler);  // 还原全局new_handler
>             throw;  // 如果内存分配失败，重新抛出异常
>         }
>         std::set_new_handler(globalHandler);  // 还原全局new_handler
>         return memory;
>     }
> 
> private:
>     static std::new_handler currentHandler;  // 静态变量保存当前的new_handler
> };
> 
> // 定义静态成员
> std::new_handler MyClass::currentHandler = nullptr;
> 
> // 自定义的处理函数
> void my_class_new_handler() {
>     std::cerr << "MyClass: Memory allocation failed, trying to recover..." << std::endl;
>     throw std::bad_alloc();  // 这里抛出异常，也可以采取其他措施
> }
> 
> int main() {
>     // 设置MyClass的new_handler
>     MyClass::set_new_handler(my_class_new_handler);
> 
>     try {
>         // 尝试为MyClass分配大量内存
>         MyClass* obj = new MyClass[100000000000];
>     } catch (const std::bad_alloc& e) {
>         std::cerr << "Caught exception: " << e.what() << std::endl;
>     }
> 
>     return 0;
> }
> ```
> 
> ### 代码解析
> 
> - **`static std::new_handler currentHandler`**: 这是一个静态成员变量，保存特定于类的`new_handler`，用于处理该类的内存分配失败。
> - **`set_new_handler`函数**: 允许为特定类设置`new_handler`，类似于全局的`std::set_new_handler`，但只对该类生效。
> - **重载的`operator new`**: 当使用`new`操作符为该类分配内存时，首先设置类特有的`new_handler`，然后尝试分配内存。如果分配失败，调用类的`new_handler`，最终恢复全局的`new_handler`。
> - **`my_class_new_handler`**: 这是自定义的内存分配失败处理函数。当`MyClass`的内存分配失败时，这个函数会被调用，并抛出`std::bad_alloc`。
> 
> ### 总结
> 
> 虽然C++标准库并没有直接为每个类提供`set_new_handler`的功能，但通过重载`operator new`并结合类静态成员，可以实现类级别的`new_handler`。这样，特定的类可以有自己的内存分配失败处理策略，而不影响全局的`new_handler`。

> 在 C++ 中，**mixin-style 基类**（混入类）是一种设计模式，通常用于通过组合多个类的行为来增加功能，而不是通过传统的继承层次结构。Mixin 类通常是轻量级的，不独立使用，通常只包含特定的功能或方法，可以与其他类组合使用。基类在实际使用中可以作为模板存在，这样更加灵活
> 
> ### 主要特点：
> 1. **组合而非继承**：mixin 类通常是通过多重继承添加到其他类中的，这与传统的继承模式不同，它更注重类的“横向”扩展（功能组合）而非“纵向”继承（类型层次）。
> 2. **无状态或轻状态**：mixin 类通常没有大量的成员变量（或者根本没有），它们专注于提供行为和功能，而不是管理状态。
> 3. **灵活性**：使用 mixin 可以灵活地将功能混入任何类中，而不需要为每个功能创建复杂的继承关系。
> 
> ### 示例代码：
> ```cpp
> #include <iostream>
> 
> // 定义一个 mixin-style 的基类，提供日志功能
> class LoggerMixin {
> public:
>     void log(const std::string& message) const {
>         std::cout << "Log: " << message << std::endl;
>     }
> };
> 
> // 定义另一个 mixin-style 的基类，提供序列化功能
> class SerializerMixin {
> public:
>     std::string serialize() const {
>         return "Serialized object";
>     }
> };
> 
> // 实际的业务类，通过继承多个 mixin 基类来增加功能
> class MyClass : public LoggerMixin, public SerializerMixin {
> public:
>     void doSomething() {
>         log("Doing something...");
>     }
> };
> 
> int main() {
>     MyClass obj;
>     obj.doSomething();
>     std::cout << obj.serialize() << std::endl;
> 
>     return 0;
> }
> ```
> 
> ### 解释：
> - `LoggerMixin` 提供了一个简单的日志功能。
> - `SerializerMixin` 提供了序列化功能。
> - `MyClass` 通过继承这两个 mixin-style 基类获得了日志和序列化的功能。
> 
> 这样做的好处是，`MyClass` 可以通过组合多个 mixin 基类灵活地获得不同的功能，而不需要定义一个庞大的继承层次。

> 在 C++ 中，**CRTP（Curiously Recurring Template Pattern）** 是一种使用模板的编程技巧，常用于实现静态多态性。它允许派生类在编译时将自己作为模板参数传递给基类，从而在基类中实现与派生类相关的功能。CRTP 主要用于提升性能，减少动态多态性（如虚函数）带来的运行时开销。可以理解成 Do It For Me
> 
> ### CRTP 的基本结构
> 
> ```cpp
> template <typename Derived>
> class Base {
> public:
>     void interface() {
>         static_cast<Derived*>(this)->implementation();
>     }
> 
>     // 默认的实现方法
>     void implementation() {
>         std::cout << "Base implementation\n";
>     }
> };
> 
> class Derived : public Base<Derived> {
> public:
>     void implementation() {
>         std::cout << "Derived implementation\n";
>     }
> };
> 
> int main() {
>     Derived d;
>     d.interface();  // 输出: Derived implementation
> }
> ```
> 
> ### CRTP 的工作原理
> - `Base` 类是一个模板类，它接受一个模板参数 `Derived`。
> - 在 `Base` 类中，调用了 `static_cast<Derived*>(this)->implementation()`，这会将当前对象转换为 `Derived` 类型，并调用 `Derived` 类的 `implementation` 方法。
> - `Derived` 类继承了 `Base<Derived>`，并实现了自己的 `implementation` 方法。
> 
> 通过这种方式，`Base` 类在编译时可以根据派生类的实现进行多态调用，而不需要使用虚函数，从而避免了运行时的开销。
> 
> ### CRTP 的优点
> 1. **静态多态性**：通过模板的方式实现编译时的多态性，避免了动态多态性带来的性能开销。
> 2. **灵活性**：可以在基类中定义通用接口，而派生类可以根据需要重载或实现自己的功能。
> 3. **性能优化**：由于是在编译时决定的多态性，CRTP 能避免虚函数带来的开销。
> 
> ### CRTP 的典型应用
> 1. **静态接口实现**：在基类中定义接口，并通过 CRTP 要求派生类实现该接口。
> 2. **对象计数器**：可以通过 CRTP 实现每个派生类的对象计数器。
> 3. **表达式模板**：在数值计算库中，用于避免不必要的临时对象创建。
> 
> CRTP 是 C++ 模板元编程中的重要技巧之一，在需要静态多态性的场景中非常有用。

> 在 C++ 中，`new (std::nothrow)` 是一种内存分配的方式，用于防止在内存分配失败时抛出异常。通常，`new` 运算符在内存分配失败时会抛出 `std::bad_alloc` 异常，而使用 `std::nothrow` 可以避免这种情况，而是返回一个空指针 `nullptr`。
> 
> ### 示例代码
> 
> ```cpp
> #include <iostream>
> #include <new>  // 引入 std::nothrow
> 
> int main() {
>     int* p = new (std::nothrow) int[1000000000000];  // 试图分配大量内存
> 
>     if (!p) {
>         std::cout << "内存分配失败！" << std::endl;
>     } else {
>         std::cout << "内存分配成功！" << std::endl;
>         delete[] p;  // 不要忘记释放内存
>     }
> 
>     return 0;
> }
> ```
> 
> ### 解释
> 
> - `new (std::nothrow) int[1000000000000];`：尝试分配一个非常大的内存块。如果分配失败，`new` 不会抛出异常，而是返回 `nullptr`。
> - `if (!p)`：检查指针是否为 `nullptr`，以确定内存是否分配成功。
> - 如果成功分配了内存，不要忘记用 `delete[]` 来释放分配的数组。
> 
> 这种方式适合那些希望通过检查返回值而不是处理异常来应对内存分配失败的场景。异常仅仅是 operator new 不返回，构造函数还是能返回的。在任何情况下，用户都永远不会需要这种new

## Item 50. Understand when it makes sense to replace new and delete

The reasons for replacing operator new and operator delete:
- To detect usage errors
> 在 C++ 编程中，`overrun` 和 `underrun` 通常与内存管理和缓冲区的操作有关，尤其是在动态内存分配和数组操作时。它们描述了内存操作过程中不正确的访问，导致对未授权的内存区域进行读写。
> 
> 1. **Overrun（内存越界/溢出）**：
>    - 这通常指的是 **缓冲区溢出**（Buffer Overrun/Overflow），即当你试图写入数据到分配给缓冲区或数组的内存区域时，超出了分配的范围（边界），导致写入了超出缓冲区的其他内存。
>    - 例如，如果你分配了一个大小为 10 的数组，但试图写入第 11 个元素，就会发生 overrun。这个行为会覆盖紧邻该数组的其他内存数据，可能导致程序崩溃或引发安全漏洞。
> 
>    示例：
>    ```cpp
>    int arr[10];   // 分配了 10 个整数的数组
>    arr[10] = 42;  // 这是一个超出数组范围的写入操作，导致 overrun
>    ```
> 
> 2. **Underrun（内存下溢）**：
>    - 这是指 **缓冲区下溢**（Buffer Underrun），即访问分配给缓冲区或数组的起始位置之前的内存区域。通常，这发生在尝试读取或写入负索引时，或者错误地认为缓冲区的最小索引比实际的小。
>    - 例如，如果你有一个数组，试图访问负索引或者比数组起始位置更早的内存地址，会导致 underrun。
> 
>    示例：
>    ```cpp
>    int arr[10];   // 分配了 10 个整数的数组
>    arr[-1] = 42;  // 这是一个访问负索引的操作，导致 underrun
>    ```
> 
> ### Overrun 和 Underrun 与内存分配的关系
> - 这些问题通常发生在动态内存分配时（如使用 `new` 或 `malloc` 分配内存）。如果程序员没有正确管理内存的边界，可能会导致访问未分配或已释放的内存区域。
> - **Overrun** 通常发生在分配的内存块之后，写入超出分配区域的数据。
> - **Underrun** 则发生在内存块分配之前，试图访问比分配地址更早的内存位置。
> 
> 这两种错误都会导致不确定的行为，可能影响程序的稳定性和安全性。
- To improve efficiency
- To collect usage statistics

> ### 内存对齐概念
> 
> **内存对齐**是指数据在内存中的地址必须按照特定的边界对齐，这样可以提高数据访问的效率。对于不同的数据类型（如 `int`、`float`、`double`），计算机可能要求它们存储在内存中某些特定的地址上，而不是任意地址。例如，32 位系统中，4 字节的整数（`int`）可能要求地址是 4 的倍数。
> 
> #### 举例说明：
> 假设一个 32 位系统中，`int` 类型需要 4 字节。如果 `int` 类型的变量存储在一个未对齐的内存地址上（比如不是 4 的倍数的地址），则 CPU 可能需要额外的内存访问操作，导致性能下降。
> 
> - 如果变量在地址 `0x1000` 处（4 的倍数），访问是对齐的，CPU 可以直接读取。
> - 如果变量在地址 `0x1003` 处（不是 4 的倍数），访问是不对齐的，CPU 需要做更多的工作，例如跨越两个内存块来读取完整数据，这会增加操作的复杂性和延迟。
> 
> ### 为什么需要内存对齐
> 
> 1. **性能原因**：现代 CPU 通常以字（word）为单位进行数据读取。未对齐的内存访问可能需要 CPU 进行多次访问，从而降低性能。
> 2. **硬件要求**：某些处理器严格要求内存对齐，如果不对齐的内存访问会引发硬件异常或系统崩溃。
> 3. **缓存效率**：内存对齐能够提高缓存命中率，减少由于跨缓存行访问数据而产生的延迟。
> 
> ### 不同系统中的内存对齐
> 
> 不同的计算机架构对于内存对齐有不同的要求：
> 
> - **32 位系统**：通常要求 4 字节的类型（如 `int`）对齐到 4 字节边界，8 字节的类型（如 `double`）对齐到 8 字节边界。
> - **64 位系统**：要求更大数据类型（如 8 字节的 `long long` 和 `double`）对齐到 8 字节边界。
> 
> ### 对齐填充（Padding）
> 
> 为了确保内存对齐，编译器有时需要在数据结构（如 `struct`）中插入额外的字节，这些字节叫做**填充字节**。它们并不存储有效数据，而是用于对齐结构体的各个成员。
> 
> #### 例子：
> ```cpp
> struct Example {
>     char a;
>     int b;
> };
> ```
> 在默认情况下，`char` 类型的 `a` 占用 1 字节，而 `int` 类型的 `b` 占用 4 字节。为了使 `b` 对齐到 4 字节边界，编译器可能会在 `a` 和 `b` 之间插入 3 字节的填充。
> 
> ```
> | a (1 byte) | padding (3 bytes) | b (4 bytes) |
> ```
> 
> 总共占用 8 字节，而不是 5 字节。
> 
> ### 自定义内存对齐
> 
> 在 C++ 中，可以使用 `alignas` 关键字或特定编译器指令来指定对齐方式。例如：
> ```cpp
> struct AlignedData {
>     alignas(16) int data[4];  // 对齐到 16 字节边界
> };
> ```
> 
> ### 总结
> 
> 内存对齐主要是为了满足硬件架构的要求，提升数据访问的效率，减少未对齐访问带来的额外开销。在不同的系统架构中，内存对齐要求不同，编译器有时会自动插入填充字节以确保对齐。

Writing a custom memory manager that almost works is pretty easy. Writing one that works well is a lot harder.

> 很多现代平台和操作系统已经有非常高效的内存管理器，因此在大多数情况下，手动自定义 `operator new` 和 `operator delete` 并不是必要的。以下是一些关键点，解释为什么现代平台上通常不需要自定义内存管理器：
> 
> ### 1. **操作系统级别的优化**
> 
> 大多数操作系统都有经过高度优化的内存分配机制，能很好地满足大多数应用场景的性能需求。这些系统通常包括：
> 
> - **glibc 的 malloc/free**：在 Linux 系统中，`malloc` 和 `free` 是由 GNU C 库（glibc）实现的，它包含了经过多次优化的内存分配器，能够很好地处理大多数情况下的内存分配需求。
>   
> - **Windows 的 HeapAlloc/HeapFree**：Windows 系统中使用的内存管理器也经过高度优化，能够有效处理内存碎片和多线程并发的内存分配。
> 
> - **tcmalloc/jemalloc**：这些是专门用于提高性能的内存分配器，广泛用于高并发、低延迟的应用场景，尤其是服务器和数据库系统中。它们的设计重点在于减少内存碎片、提高多线程环境下的分配速度，并且已经集成到许多现代应用程序和平台中。
> 
> ### 2. **内存池和缓存分配器**
> 
> 一些现代内存管理器，如 `jemalloc` 和 `tcmalloc`，会使用**内存池**和**缓存分配器**，这使得它们能够以更高的效率处理小对象的频繁分配和释放。由于这些机制的存在，很多情况下不需要手动管理小块内存分配。
> 
> ### 3. **垃圾收集器 (GC) 和智能指针**
> 
> 在某些高级语言（如 Java 和 C#）中，使用**垃圾收集器 (Garbage Collector)** 来管理内存，这消除了手动内存管理的需求。在 C++ 中，`std::shared_ptr` 和 `std::unique_ptr` 等智能指针极大地简化了内存的管理和释放，减少了内存泄漏的风险。
> 
> ### 4. **专用分配器**
> 
> 某些平台或应用会选择在特定场景下使用专用分配器（如 `tcmalloc`、`jemalloc` 或 `dlmalloc`），这些专用分配器在性能和内存利用率方面比系统默认的分配器更优。它们被广泛应用于数据库、Web 服务器和高性能计算等领域。使用这些专用分配器通常可以避免手动自定义 `operator new`。
> 
> ### 5. **STL 提供的分配器支持**
> 
> C++ 标准库提供了一个灵活的分配器接口，允许程序开发者自定义分配器，而不需要自定义 `operator new`。通过自定义分配器，开发者可以为特定的容器或数据结构定制内存分配策略，而不影响其他代码。
> 
> 例如：
> ```cpp
> std::vector<int, MyCustomAllocator<int>> vec;
> ```
> 
> 这种方式允许开发者只在必要的场景下定制内存管理，而不是全局修改 `operator new` 和 `operator delete`。
> 
> ### 6. **现代编译器和硬件优化**
> 
> 现代编译器和硬件已经对内存访问做了很多优化。例如：
> 
> - **缓存机制**：CPU 的缓存机制可以极大地提高内存访问的效率，即使某些情况下内存访问没有严格对齐。
> - **预取（Prefetching）**：现代处理器能够通过预取机制提前加载内存数据，减小访问延迟。
> 
> 这些硬件级别的优化进一步减少了手动优化内存分配和管理的需求。
> 
> ### 7. **何时需要自定义 `operator new`**
> 
> 虽然大多数情况下无需自定义 `operator new` 和 `operator delete`，但在以下场景中仍然可能需要手动优化内存管理：
> 
> - **嵌入式系统**：内存资源极其有限的环境中，定制内存分配策略可以显著提升性能和资源利用率。
> - **实时系统**：需要确保内存分配和释放的时间复杂度是可控的，以避免延迟问题。
> - **游戏引擎**：对于一些高性能场景，尤其是游戏开发，手动控制内存分配有时能进一步优化性能，减少内存碎片。
> - **特殊对齐需求**：在某些情况下，程序可能需要特定的内存对齐（如使用 SIMD 指令），这时自定义 `operator new` 以确保对齐是必要的。
> 
> ### 总结
> 
> 大多数现代平台已经有非常优秀的内存管理器，满足了大部分应用的需求，通常不需要自定义 `operator new`。除非在一些特殊场景下（如嵌入式系统、游戏引擎或实时系统），手动管理内存可能会带来一定的性能收益，但需要谨慎使用，以避免引入复杂性和潜在的错误。

> 内存管理器在不同的场景下有不同的需求，商用和开源的内存管理器非常丰富，既有专为高性能应用设计的分配器，也有适合嵌入式系统、游戏开发或特定应用的内存管理器。以下是一些常见的商用和开源内存管理器：
> 
> ### 1. **开源内存管理器**
> 
> #### 1.1. **tcmalloc**
> - **全称**：Thread-Caching Malloc
> - **维护者**：Google
> - **用途**：广泛用于高并发、低延迟的场景，例如 Google 的服务器和大型应用。
> - **特性**：
>   - 对小对象有极高的分配效率。
>   - 每个线程维护独立的缓存，减少线程间的竞争。
>   - 减少内存碎片，提高多线程性能。
>   - 常用于 C++ 高性能服务器应用中，例如 `gperftools` 中的 `tcmalloc` 。
>   
> - **项目地址**：[gperftools/tcmalloc](https://github.com/gperftools/gperftools)
> 
> #### 1.2. **jemalloc**
> - **维护者**：Jason Evans
> - **用途**：广泛用于数据库、操作系统和服务器等高性能应用，最著名的用户是 Redis 和 Facebook。
> - **特性**：
>   - 高效处理多线程环境下的内存分配。
>   - 减少内存碎片，并优化内存使用。
>   - 提供了多种工具用于内存分析和调优。
>   
> - **项目地址**：[jemalloc/jemalloc](https://github.com/jemalloc/jemalloc)
> 
> #### 1.3. **Hoard**
> - **维护者**：Emery Berger
> - **用途**：用于减少内存碎片并提高多线程环境下的分配性能，适用于服务器和并发性强的应用。
> - **特性**：
>   - 高效支持多线程，并减少竞争锁定。
>   - 支持大规模并发应用和高性能服务器。
>   - 通过层次化的分配器减少了锁竞争。
>   
> - **项目地址**：[Hoard](https://github.com/emeryberger/Hoard)
> 
> #### 1.4. **dlmalloc**
> - **全称**：Doug Lea's Malloc
> - **维护者**：Doug Lea
> - **用途**：经典的内存分配器，在很多操作系统和嵌入式系统中使用，因其简单和高效而著称。
> - **特性**：
>   - 内存碎片较少，性能较好。
>   - 被广泛用于早期的 Linux、FreeBSD 等系统。
>   - 设计上比较简单，适合嵌入式系统。
>   
> - **项目地址**：[dlmalloc](http://g.oswego.edu/dl/html/malloc.html)
> 
> #### 1.5. **ptmalloc**
> - **全称**：Pthread Malloc
> - **维护者**：Wolfram Gloger
> - **用途**：基于 `dlmalloc`，针对多线程进行了优化。它曾是 GNU C 库（glibc）中的默认内存管理器。
> - **特性**：
>   - 针对多线程场景的优化。
>   - 继承了 `dlmalloc` 的特点，并减少了锁竞争。
>   
> - **项目地址**：[ptmalloc](http://www.malloc.de/en/)
> 
> #### 1.6. **Scalloc**
> - **全称**：Scalable allocator
> - **维护者**：团队开发
> - **用途**：一种针对高并发、多核系统设计的分配器，侧重于提高并发性能。
> - **特性**：
>   - 通过缓存局部性和分离分配机制提升性能。
>   - 在高度并发的环境下仍能保证良好的性能。
>   
> - **项目地址**：[Scalloc](https://github.com/cksystemsgroup/scalloc)
> 
> #### 1.7. **bdwgc**
> - **全称**：Boehm-Demers-Weiser Garbage Collector
> - **维护者**：Hans Boehm 等
> - **用途**：基于标记-清除算法的垃圾收集器，广泛用于 C/C++ 代码中，适合需要自动垃圾回收的场景。
> - **特性**：
>   - 自动内存管理，减少内存泄漏。
>   - 支持多线程程序的垃圾回收。
>   
> - **项目地址**：[bdwgc](https://github.com/ivmai/bdwgc)
> 
> ### 2. **商用内存管理器**
> 
> #### 2.1. **Intel Memory Allocator (Intel TBB malloc)**
> - **全称**：Threading Building Blocks Malloc
> - **维护者**：Intel
> - **用途**：为多核处理器设计，提供高效的内存分配解决方案。适合并发和多线程程序，尤其是在 Intel 处理器上优化。
> - **特性**：
>   - 对多线程应用进行优化，减少线程间的内存竞争。
>   - 可以作为替代标准库的 `malloc` 使用。
>   
> - **相关产品**：包含在 Intel TBB 库中，用于高性能计算和多线程程序开发。
> 
> #### 2.2. **Microsoft Windows Low Fragmentation Heap (LFH)**
> - **维护者**：Microsoft
> - **用途**：Windows 操作系统中的默认内存管理器之一，专门用于减少内存碎片，提升系统整体性能。
> - **特性**：
>   - 用于避免标准堆中的碎片问题。
>   - 通过自动分配低碎片内存池，提升应用性能。
>   
> - **相关产品**：Windows 操作系统的内存管理机制。
> 
> #### 2.3. **VMem**
> - **维护者**：VMware
> - **用途**：VMem 是一个商用的内存管理解决方案，专门为 VMware 的虚拟化产品优化，能够高效地处理虚拟机的内存需求。
> - **特性**：
>   - 处理虚拟化环境中复杂的内存分配需求。
>   - 高效管理虚拟机中的大块和小块内存分配。
> 
> #### 2.4. **RPMalloc**
> - **维护者**：Randy Gaul
> - **用途**：一个小而高效的多线程内存分配器，特别适合游戏开发和嵌入式系统。
> - **特性**：
>   - 高效支持多线程，并保证低碎片和高并发性能。
>   - 适合小块内存分配和实时系统。
>   
> - **项目地址**：[RPMalloc](https://github.com/rampantpixels/rpmalloc)
> 
> #### 2.5. **EASTL Allocator**
> - **维护者**：Electronic Arts
> - **用途**：专为游戏开发优化，Electronic Arts 开发的内存分配器与其专有的 EASTL 标准库结合使用。
> - **特性**：
>   - 适用于实时游戏引擎中的内存分配。
>   - EASTL 是标准 C++ STL 的一个高效实现，专门为游戏和嵌入式环境设计。
> 
> - **相关产品**：EA 的游戏引擎和相关游戏开发工具链中使用。
> 
> ### 3. **特殊用途的内存管理器**
> 
> #### 3.1. **TLSF (Two-Level Segregated Fit)**
> - **用途**：专门为嵌入式系统和实时系统设计，确保分配和释放操作的时间复杂度为 O(1)。
> - **特性**：
>   - 时间复杂度为 O(1)，非常适合实时系统和嵌入式系统。
>   - 碎片率低，保证了在长时间运行后的性能稳定性。
> 
> - **项目地址**：[TLSF](https://github.com/mattconte/tlsf)
> 
> #### 3.2. **SNmalloc**
> - **用途**：一个基于消息传递的内存分配器，适合基于对象的语言，具有高并发性和高效的内存分配特性。
> - **特性**：
>   - 线程私有缓存，减少线程间锁竞争。
>   - 优化了多线程环境下的性能。
> 
> - **项目地址**：[SNmalloc](https://github.com/microsoft/snmalloc)
> 
> ### 总结
> 
> 不同的内存管理器针对不同的需求和应用场景进行了优化。对于高并发、低延迟的应用，像 `jemalloc` 和 `tcmalloc` 这样的管理器非常适合。而在嵌入式系统和实时系统中，`TLSF` 等分配器提供了确定性的内存管理方案。在游戏开发和专用环境下，像 `RPMalloc` 和 `EASTL Allocator` 提供了针对性优化。根据应用场景选择合适的内存管理器可以显著提升系统的性能和稳定性。


## Adhere to convention when writing new and delete

> 在C++中，`operator new` 的自定义实现可以用于为对象分配内存，通常默认的 `operator new` 是使用标准库的 `malloc` 或类似的低级内存分配函数进行实现的。而自定义的 `operator new` 可以通过以下方式进行修改，以满足特定需求，如处理请求分配大小为 0 和重复调用 `new_handler`（处理内存分配失败的机制）。
> 
> ### 自定义 `operator new` 实现
> 你可以重载全局的 `operator new` 或者为特定类重载 `operator new`，以下是一个自定义的 `operator new` 示例：
> 
> ```cpp
> #include <new>        // For std::bad_alloc, std::set_new_handler
> #include <cstdlib>    // For std::malloc, std::free
> #include <iostream>
> 
> // 定义一个全局的 new_handler
> void my_new_handler() {
>     std::cerr << "Custom new_handler: Memory allocation failed!" << std::endl;
>     std::abort();
> }
> 
> // 自定义 operator new
> void* operator new(std::size_t size) noexcept(false) {
>     std::cout << "Custom operator new called. Size requested: " << size << std::endl;
> 
>     // 如果请求大小为 0，设置为最小分配单位
>     if (size == 0) {
>         size = 1;  // 有些系统不允许分配 0 字节，可以设置为 1 字节
>     }
> 
>     // 尝试分配内存
>     void* ptr = nullptr;
>     while (true) {
>         ptr = std::malloc(size);
>         if (ptr) {
>             return ptr;  // 成功分配内存
>         }
> 
>         // 如果分配失败，调用 new_handler
>         //没有直接的get_handler，只能先调用set_new_handler得到handler之后再重新设置回原来的情况
>         std::new_handler global_handler = std::set_new_handler(0);
>         std::set_new_handler(globalHandler);
>         if (global_handler) {
>             global_handler();  // 调用当前设置的 new_handler
>         } else {
>             throw std::bad_alloc();  // 如果没有设置 handler，则抛出 std::bad_alloc 异常
>         }
>     }
> }
> 
> // 自定义 operator delete
> void operator delete(void* ptr) noexcept {
>     std::cout << "Custom operator delete called." << std::endl;
>     std::free(ptr);
> }
> 
> int main() {
>     // 设置自定义的 new_handler
>     std::set_new_handler(my_new_handler);
> 
>     try {
>         // 尝试分配内存
>         int* p = new int[1000000000000];  // 请求大内存，触发失败处理
>     } catch (const std::bad_alloc&) {
>         std::cerr << "Caught std::bad_alloc exception!" << std::endl;
>     }
> 
>     return 0;
> }
> ```
> 
> ### 代码解释：
> 1. **自定义的 `operator new`**：
>    - 在这个实现中，我们重载了全局的 `operator new`。当请求的分配大小为 0 时，将其设置为 1，以防止某些系统不支持 0 字节分配。
>    - 使用 `malloc` 来实际分配内存，如果分配失败，则调用 `new_handler`。
>    
> 2. **`new_handler` 的处理**：
>    - `new_handler` 是一个函数指针，指向当 `new` 操作符内存分配失败时调用的处理函数。可以通过 `std::set_new_handler` 来设置它。如果 `malloc` 失败，会调用这个处理函数，处理失败的情况。
>    - 如果没有设置 `new_handler`，将抛出 `std::bad_alloc` 异常。
> 
> 3. **重复调用 `new_handler`**：
>    - 如果内存分配失败，`operator new` 将反复调用已设置的 `new_handler`，直到内存分配成功或 `new_handler` 自行中止程序（例如通过调用 `std::abort`）。
>    
> 4. **自定义 `operator delete`**：
>    - 同时，自定义的 `operator delete` 用于释放内存，以便配合 `operator new` 一起使用。
> 
> ### 特别注意：
> - **分配大小为 0**：一些系统不允许分配 0 字节，因此通常的做法是将请求大小为 0 的分配调整为至少 1 字节。
> - **new_handler 的作用**：`new_handler` 通常用于处理内存分配失败的场景，重复调用它可以让程序尝试多次分配内存或记录日志等。
> 
> ### 默认 `operator new` 行为：
> 默认的 `operator new` 行为是在请求的内存大小为 0 时，不保证返回有效指针（可能会返回 `nullptr` 或者其他行为），并且它默认只会调用一次 `new_handler`。通过自定义它，我们可以为这种特殊场景添加更精细的处理逻辑。

> 在 C++ 中，如果基类定义了自定义的 `operator new`，但子类继承并调用了这个自定义的 `operator new`，确实可能引发一些性能和内存管理问题，尤其是在子类需要额外的内存布局或特殊的初始化过程时。因为基类的 `operator new` 是根据基类的需求优化的，而子类可能有不同的内存分配需求，比如额外的成员变量或更复杂的构造过程，导致子类使用基类的 `operator new` 可能不是最佳选择。
> 
> ### 可能导致子类性能问题的原因：
> 1. **内存分配不适合子类**：基类的 `operator new` 可能分配的内存空间大小或方式是根据基类的需求优化的，而子类的内存布局可能更复杂，需要更多或不同的内存分配策略。
>    
> 2. **没有考虑子类的特殊需求**：基类的 `operator new` 可能忽略了子类在初始化时需要的额外内存或资源分配需求，导致子类的构造或运行时性能受影响。
> 
> ### 解决方法：
> 1. **在子类中重新定义 `operator new`**：如果子类有不同的内存需求，可以在子类中重新定义 `operator new` 和 `operator delete`，以便根据子类的特点进行内存分配。例如：
> 
>    ```cpp
>    class Base {
>    public:
>        void* operator new(size_t size) {
>            std::cout << "Base class operator new\n";
>            return ::operator new(size);
>        }
>    };
> 
>    class Derived : public Base {
>    public:
>        void* operator new(size_t size) {
>            std::cout << "Derived class operator new\n";
>            return ::operator new(size);
>        }
>    };
>    ```
> 
> 2. **通过 RTTI 或类型信息进行适配**：如果无法或不希望在子类中重新定义 `operator new`，可以在基类的 `operator new` 中添加对子类的类型检查，并动态适配分配的内存空间。例如使用 `typeid` 或其他机制来识别类型。
> 
> 3. **避免在基类中定义通用的 `operator new`**：如果基类和子类的内存需求差异很大，可以考虑避免在基类中定义 `operator new`，让每个类自行管理内存分配，从而减少由于继承带来的问题。
> 
> ### 小结：
> 基类自定义 `operator new` 的优化可能不适用于子类，因为子类可能有不同的内存布局需求。如果这种优化确实影响到了子类的性能，最佳的解决方案是根据需要在子类中重新定义 `operator new` 或调整内存分配策略。

> 自定 operator new[] 有更多需要考虑的内容，例如难以确定具体有多大空间需要分配，因为子类也能调用基类实现的array new，并且空间中还需要记录数组中元素的数量

> 在 C++ 中，`operator delete` 函数通常用于释放通过 `operator new` 分配的内存，并且允许delete null，在自定义实现时需要注意。标准的 `operator delete` 有多种版本，其中一个可以接受两个参数：指针和大小。这个功能在 C++17 中引入，允许自定义的 `delete` 操作更灵活，尤其是在某些优化场景中，编译器可以传递内存块的大小。在某些情况下，new申请的大小可能是错的，比如类只需要4B，但new返回了6B，这时delete必须释放6B,用ptr参数，而不是size参数。
> 
> 这个函数的签名通常是这样的：
> 
> ```cpp
> void operator delete(void* ptr, std::size_t size) noexcept;
> ```
> 
> ### 参数说明：
> - `ptr`：指向需要释放的内存的指针。
> - `size`：表示被释放内存块的大小。
> 
> ### 使用场景：
> 1. 自定义分配器：当你实现自定义内存分配器时，可以利用 `size` 来更高效地管理内存。例如，你可能会根据大小选择不同的内存池来释放内存。
> 2. 调试和优化：通过 `size`，可以进行内存使用的分析、日志记录或做出一些优化决策。
> 3. 与 `operator new` 相匹配：如果你自定义了带有大小参数的 `operator new`，那么通常也要相应地自定义 `operator delete`，以确保内存分配和释放的一致性。
> 
> ### 例子：
> 
> ```cpp
> #include <iostream>
> #include <cstdlib>
> 
> class MyClass {
> public:
>     static void* operator new(std::size_t size) {
>         std::cout << "Allocating " << size << " bytes\n";
>         return std::malloc(size);
>     }
> 
>     static void operator delete(void* ptr, std::size_t size) {
>         std::cout << "Deallocating " << size << " bytes\n";
>         std::free(ptr);
>     }
> };
> 
> int main() {
>     MyClass* obj = new MyClass;
>     delete obj;
>     return 0;
> }
> ```
> 
> ### 输出：
> ```
> Allocating 1 bytes
> Deallocating 1 bytes
> ```
> 
> 在上面的例子中，自定义了 `operator new` 和 `operator delete`，并且 `delete` 操作收到了分配时的大小信息。


## Item 52: Write placement if you write placement new

> **`placement new` 是指除了size_t 之外有额外的参数的 operator new，例如使用一个指针作为额外参数是它的典型用法**。其作用是指定对象应该被构造在给定的内存位置上。
> 
> ### 使用 `placement new` 的场景
> 
> 在容器（如 `std::vector`）扩容时，通常需要将现有元素复制到新的、更大的内存块中，这时 `placement new` 就可以派上用场。例如，当 `std::vector` 的容量不足时，首先会分配一块新的、更大的内存空间，然后使用 `placement new` 在这块新内存中构造原有元素，最后释放旧内存。
> 
> ### 语法示例
> 
> ```cpp
> #include <new>  // for std::nothrow
> #include <iostream>
> 
> int main() {
>     // 预分配一块足够的内存
>     char* buffer = new char[sizeof(int)];
> 
>     // 在 buffer 所指的内存位置上构造一个 int 对象，值为42
>     int* p = new (buffer) int(42);
> 
>     // 输出对象的值
>     std::cout << *p << std::endl;
> 
>     // 需要手动调用析构函数，因为内存没有通过正常的 new 分配
>     p->~int();
> 
>     // 手动释放之前分配的内存
>     delete[] buffer;
> 
>     return 0;
> }
> ```
> 
> 一般而言人们讨论中提到的 placement new 大多数指的是 vector 中实现的这个特定版本的 placement new，而不是上面提到的更广泛的定义；少部分时候是指上面那个有额外参数的定义。具体指哪个需要联系上下文看具体含义
> 
> ### 注意事项
> 
> - 使用 `placement new` 构造对象后，需要手动调用析构函数来销毁对象，因为普通的 `delete` 不会被调用。
> - 需要手动管理内存的分配与释放，错误处理起来可能比较复杂，容易引发内存泄漏或其他未定义行为。
> 
> 这种方式在需要高度优化和控制内存分配的场景中非常有用，比如在容器的实现中使用以避免频繁的内存分配和释放操作。

> c++中new的具体执行流程是operator new，static_cast 修改内存type，然后调用构造函数。如果你定义了带有额外参数的 `operator new`即 placement new，那么为了避免潜在的内存泄漏，你也必须定义相应带有相同参数签名的 `operator delete` 和 普通版本的 operator delete 即不带参数的版本，因为大多数时候 new 没抛出异常，就需要调用普通版本的 delete，这时候也需要处理有额外参数new造成的额外开支。这是因为：
> 
> - 当使用 `new` 分配内存时，`operator new` 被调用来分配内存。
> - 如果在分配完内存后，构造函数抛出了异常，C++ 会自动调用相应的 `operator delete` 来释放之前分配的内存。
> 
> ### 详细解释：
> 
> 1. **自定义 `operator new` 和 `operator delete`：**
>    你可以为某个类重载 `operator new` 和 `operator delete`，并且可以为 `operator new` 提供额外的参数。
> 
>    ```cpp
>    class MyClass {
>    public:
>        void* operator new(size_t size, int extraParam) {
>            std::cout << "Allocating memory with extra parameter: " << extraParam << std::endl;
>            return ::operator new(size);
>        }
>    
>        void operator delete(void* ptr, int extraParam) {
>            std::cout << "Deallocating memory with extra parameter: " << extraParam << std::endl;
>            ::operator delete(ptr);
>        }
>    };
>    ```
> 
> 2. **异常处理中的配对问题：**
>    当你使用 `new` 分配对象时，通常会先调用 `operator new` 来分配内存，然后调用构造函数初始化对象。如果构造函数抛出异常，分配的内存需要通过调用 `operator delete` 进行释放。对于自定义的 `operator new`，其调用时传递的参数在异常处理时也需要传递给 `operator delete`，否则可能会导致内存泄漏。
> 
>    例如：
> 
>    ```cpp
>    try {
>        MyClass* obj = new (42) MyClass();
>    } catch (...) {
>        // 如果构造函数抛出异常，带参数的 operator delete 会被调用来释放内存
>    }
>    ```
> 
>    这里 `42` 是传给 `operator new` 的额外参数，如果构造函数抛出异常，C++ 需要用相同的额外参数（42）调用 `operator delete` 来释放内存。
> 
> 3. **没有配对的风险：**
>    **如果你只定义了带有额外参数的 `operator new`，但没有定义匹配的 `operator delete`，那么在构造函数抛出异常时，C++ 的runtime system会找不到合适的 `operator delete` 来释放内存，从而导致内存泄漏**。
> 
> 因此，在自定义带参数的 `operator new` 时，务必定义具有相同参数签名的 `operator delete`和普通版本的 `operator delete`，以确保异常安全性。

> 在实现类的 placement operator new 的时候要注意不要用名字覆盖全局正常版本的 operator new 或基类声明的 operator new，覆盖见Item 33

> 在 C++ 中，`operator new` 是用于动态内存分配的操作符。它有多个全局版本，可以根据不同的需求进行内存分配。以下是标准库中提供的几个全局 `operator new` 版本：
> 
> ### 1. `void* operator new(std::size_t size);`
> - 这个是最常见的 `new` 操作符，它分配大小为 `size` 的内存，并返回一个指向该内存块的指针。如果分配失败，会抛出 `std::bad_alloc` 异常。
> 
> ### 2. `void* operator new(std::size_t size, const std::nothrow_t&) noexcept;`
> - 这是一个不抛出异常的版本。如果内存分配失败，它返回 `nullptr` 而不是抛出异常。
> 
> ### 3. `void* operator new[](std::size_t size);`
> - 用于分配数组类型的内存，与第一个版本类似，只不过它用于分配多个元素的连续内存。
> 
> ### 4. `void* operator new[](std::size_t size, const std::nothrow_t&) noexcept;`
> - 不抛出异常的数组版本。如果分配失败，返回 `nullptr`。
> 
> ### 5. Placement new: `void* operator new(std::size_t size, void* ptr) noexcept;`
> - 这个版本的 `new` 是“定位 new”（Placement new），不会分配新的内存，而是直接在给定的内存地址 `ptr` 处构造对象。这种用法常见于自定义内存管理。
> 
> ### 6. `void* operator new[](std::size_t size, void* ptr) noexcept;`
> - 这是定位 new 的数组版本，用于在指定地址 `ptr` 上构造数组对象。
> 
> ### 7. Aligned allocation (C++17 引入的对齐分配版本):
>   - `void* operator new(std::size_t size, std::align_val_t alignment);`
>   - `void* operator new(std::size_t size, std::align_val_t alignment, const std::nothrow_t&) noexcept;`
>   - `void* operator new[](std::size_t size, std::align_val_t alignment);`
>   - `void* operator new[](std::size_t size, std::align_val_t alignment, const std::nothrow_t&) noexcept;`
> 
>   这些版本允许指定分配的内存块的对齐方式，以便支持对齐要求较高的数据类型。
> 
> ### 8. 自定义 `operator new`
> - 程序员还可以自定义 `new` 操作符，以实现自己的内存管理逻辑。
> 
> 这些不同版本的 `operator new` 提供了灵活的内存分配机制，适应不同的应用场景。当在类内部自定义了任何版本的 operator new，都将会覆盖这些标准形式正常版本的 operator new。一个简单的不覆盖的方式是声明一个StandardNewDeleteForms类，并且实现各个版本的new和相对应的delete，让自定义类继承这个类，并用using 声明

![实现free和delete](52-1.png)
![自定义类继承](52-2.png)

# 9.Miscellany
## Item 53: Pay attention to compiler warnings

- Take compiler warnings seriously, and strive to compile warning-free at the maximum warning level supported by yor compilers.
- Don't become depend on compiler warnings, because different compilers warn about different things. Porting to a new compilers may eliminate warning messages you've come to rely on.



## Item 54: Familiarize yourself with the standard library including TR1

> 在C++中，输入输出操作通过标准库中的一组预定义的对象来处理。最常用的三个输入输出流对象是：
> 
> 1. **`cin`**（标准输入流）：
>    - `cin` 是一个标准输入流对象，用于从键盘获取输入。
>    - `cin` 使用的是输入运算符（`>>`），会根据用户输入的格式将数据读入变量中。输入时，`cin` 会忽略空格、换行符和制表符，直到遇到非空白字符开始读取。
>    - 示例：
>      ```cpp
>      #include <iostream>
>      using namespace std;
> 
>      int main() {
>          int number;
>          cout << "Enter a number: ";
>          cin >> number;  // 从用户输入读取数据
>          cout << "You entered: " << number << endl;
>          return 0;
>      }
>      ```
> 
> 2. **`cout`**（标准输出流）：
>    - `cout` 是一个标准输出流对象，用于向控制台输出数据。
>    - `cout` 使用的是输出运算符（`<<`），可以输出各种数据类型并自动格式化。
>    - 示例：
>      ```cpp
>      #include <iostream>
>      using namespace std;
> 
>      int main() {
>          cout << "Hello, World!" << endl;  // 向控制台输出信息
>          return 0;
>      }
>      ```
> 
> 3. **`cerr`**（标准错误流）：
>    - `cerr` 是标准错误流对象，专门用于输出错误信息。
>    - 与 `cout` 类似，`cerr` 也使用输出运算符（`<<`）来输出数据，但 `cerr` 的输出通常是未经缓冲的（unbuffered），意味着信息会立即输出到控制台，常用于错误或调试信息的输出。
>    - 示例：
>      ```cpp
>      #include <iostream>
>      using namespace std;
> 
>      int main() {
>          cerr << "Error: Something went wrong!" << endl;  // 输出错误信息
>          return 0;
>      }
>      ```
> 
> 4. **`clog`**（标准日志流）：
>    - `clog` 也是一个输出流对象，专门用于输出日志信息。
>    - 与 `cerr` 不同的是，`clog` 是缓冲的（buffered），这意味着它的输出可能会稍微延迟，直到缓冲区被填满或程序显式刷新输出缓冲。
>    - 示例：
>      ```cpp
>      #include <iostream>
>      using namespace std;
> 
>      int main() {
>          clog << "Log: This is a log message." << endl;  // 输出日志信息
>          return 0;
>      }
>      ```
> 
> ### 总结：
> - **`cin`**：从标准输入设备（通常是键盘）读取输入。
> - **`cout`**：向标准输出设备（通常是屏幕）输出数据。
> - **`cerr`**：向标准错误设备（通常是屏幕）输出错误信息（无缓冲）。
> - **`clog`**：向标准日志设备（通常是屏幕）输出日志信息（有缓冲）。
> 
> 这些对象都定义在头文件 `<iostream>` 中，并且通常与 C++ 标准流 `istream`、`ostream`、`iostream` 的类功能相关。
 
> C++ 的异常处理机制（Exception Handling）允许程序在运行时捕获并处理错误或异常情况，而不会导致程序崩溃。C++ 的异常处理系统由关键字 `try`、`throw` 和 `catch` 实现，通过抛出异常和捕获异常来处理程序中的错误。异常处理系统中的异常类型组织成一个层次结构，允许程序员定义和使用标准异常类以及自定义异常类。
> 
> ### C++ 异常系统的层次结构
> 
> C++ 提供了一组标准异常类，它们都继承自标准库中的 `std::exception` 类，形成了一种类层次结构。`std::exception` 是所有异常类的基类，其他异常类则从它派生出来。以下是 C++ 中常见的标准异常类及其层次结构：
> 
> #### 1. `std::exception`（基类）
>    - `std::exception` 是所有 C++ 标准异常的基类，位于头文件 `<exception>` 中。
>    - 该类定义了一个虚拟成员函数 `what()`，用于返回异常的描述信息（通常为字符指针 `const char*`）。
>    - 示例：
>      ```cpp
>      #include <iostream>
>      #include <exception>
> 
>      int main() {
>          try {
>              throw std::exception();
>          } catch (const std::exception& e) {
>              std::cout << "Caught exception: " << e.what() << std::endl;
>          }
>          return 0;
>      }
>      ```
> 
> #### 2. `std::bad_alloc`（内存分配错误）
>    - 继承自 `std::exception`，用于表示在内存分配失败时抛出的异常（如 `new` 操作符分配内存失败）。
>    - 头文件 `<new>` 中定义。
>    - 示例：
>      ```cpp
>      #include <iostream>
>      #include <new>
> 
>      int main() {
>          try {
>              int* arr = new int[100000000000];  // 可能分配失败
>          } catch (const std::bad_alloc& e) {
>              std::cout << "Memory allocation failed: " << e.what() << std::endl;
>          }
>          return 0;
>      }
>      ```
> 
> #### 3. `std::bad_cast`（类型转换失败）
>    - 继承自 `std::exception`，用于表示动态类型转换失败（`dynamic_cast`）时抛出的异常。
>    - 头文件 `<typeinfo>` 中定义。
>    - 示例：
>      ```cpp
>      #include <iostream>
>      #include <typeinfo>
> 
>      class Base { virtual void foo() {} };
>      class Derived : public Base {};
> 
>      int main() {
>          try {
>              Base* base = new Base;
>              Derived* derived = dynamic_cast<Derived*>(base);  // 转换失败
>              if (!derived) throw std::bad_cast();
>          } catch (const std::bad_cast& e) {
>              std::cout << "Bad cast: " << e.what() << std::endl;
>          }
>          return 0;
>      }
>      ```
> 
> #### 4. `std::bad_typeid`（类型信息获取失败）
>    - 继承自 `std::exception`，在通过 `typeid` 操作获取空指针的类型信息时抛出异常。
>    - 头文件 `<typeinfo>` 中定义。
>    - 示例：
>      ```cpp
>      #include <iostream>
>      #include <typeinfo>
> 
>      class Base { virtual void foo() {} };
> 
>      int main() {
>          try {
>              Base* base = nullptr;
>              std::cout << typeid(*base).name() << std::endl;  // 会抛出 std::bad_typeid 异常
>          } catch (const std::bad_typeid& e) {
>              std::cout << "Bad typeid: " << e.what() << std::endl;
>          }
>          return 0;
>      }
>      ```
> 
> #### 5. `std::bad_function_call`（无效的函数调用）
>    - 继承自 `std::exception`，表示无效的函数调用（如调用空 `std::function` 对象）时抛出的异常。
>    - 头文件 `<functional>` 中定义。
> 
> #### 6. `std::logic_error`（逻辑错误异常类）
>    - `std::logic_error` 是从 `std::exception` 派生的，用于表示程序逻辑上的错误，这些错误通常由代码中的设计缺陷引起，不应该通过运行时来捕获，而是应该在编写程序时避免。
>    - 这个类主要用于捕获那些在编译时能预测到的错误。
>    - 它的派生类有：
>      - `std::invalid_argument`：表示函数接收到无效的参数。
>      - `std::domain_error`：表示数学函数的输入超出了有效范围。
>      - `std::length_error`：表示尝试创建超出最大长度的对象。
>      - `std::out_of_range`：表示数组或容器访问超出了有效范围。
> 
> #### 7. `std::runtime_error`（运行时错误异常类）
>    - `std::runtime_error` 也是从 `std::exception` 派生的，用于表示程序运行时出现的错误，这些错误是程序无法避免的异常情况（如文件读取失败、数学错误等）。
>    - 它的派生类有：
>      - `std::range_error`：表示算术运算产生超出有效范围的结果。
>      - `std::overflow_error`：表示算术运算导致溢出。
>      - `std::underflow_error`：表示算术运算导致下溢。
> 
> ### C++ 异常处理的基本流程
> 
> 1. **`throw`** 关键字：
>    - 用于抛出异常，当程序检测到错误时，通过 `throw` 抛出异常对象。
>    - 可以抛出标准异常对象，也可以抛出用户自定义的异常对象。
>    - 示例：
>      ```cpp
>      throw std::runtime_error("Error occurred");
>      ```
> 
> 2. **`try` 块**：
>    - 用于包裹可能会抛出异常的代码，异常会在 `try` 块中抛出。
>    - 示例：
>      ```cpp
>      try {
>          // 可能抛出异常的代码
>      }
>      ```
> 
> 3. **`catch` 块**：
>    - 用于捕获异常，`catch` 块后紧跟一个参数，表示捕获的异常类型。
>    - 一个 `try` 块可以有多个 `catch` 块，每个 `catch` 块捕获不同类型的异常。
>    - 示例：
>      ```cpp
>      try {
>          // 可能抛出异常的代码
>      } catch (const std::exception& e) {
>          std::cout << "Caught exception: " << e.what() << std::endl;
>      }
>      ```
> 
> ### 自定义异常类
> 
> C++ 允许程序员定义自己的异常类，可以继承自 `std::exception` 或其他标准异常类，重写 `what()` 方法提供更具体的异常描述。
> 
> 示例：
> ```cpp
> #include <iostream>
> #include <exception>
> 
> class MyException : public std::exception {
> public:
>     const char* what() const noexcept override {
>         return "Custom exception occurred";
>     }
> };
> 
> int main() {
>     try {
>         throw MyException();
>     } catch (const MyException& e) {
>         std::cout << e.what() << std::endl;
>     }
>     return 0;
> }
> ```
> 
> ### 总结
> 
> C++ 的异常处理系统层次结构以 `std::exception` 为基类，派生出各种标准异常类，如 `std::runtime_error` 和 `std::logic_error`，这些类进一步派生出更多具体的异常类型。开发者可以捕获这些异常，或定义自定义异常来处理特定的错误情况。

> **TR1** 是一个技术规范，它定义了一系列特性和库接口，但它本身并不提供具体的实现。TR1 的具体实现是由不同的编译器和标准库供应商提供的，类似于 C++ 标准库的其他部分。
> 
> ### TR1 的实现与依赖
> 
> 不同编译器和库供应商（如 GCC、Clang、Microsoft Visual C++ 等）通常会根据 TR1 规范提供具体的实现。这些实现可能存在于标准库的不同命名空间中，也可能通过额外的库来提供支持。在某些编译器中，TR1 的特性需要通过特定的标志或库链接来使用。
> 
> #### 不同编译器的 TR1 实现
> 1. **GCC（GNU Compiler Collection）**
>    GCC 提供了对 TR1 的支持，通常通过 `libstdc++` 标准库实现。在 GCC 中，TR1 的功能可以在 `<tr1/...>` 头文件中找到，TR1 的内容通常位于 `std::tr1` 命名空间中。比如：
>    ```cpp
>    #include <tr1/memory>
>    std::tr1::shared_ptr<int> p(new int(10));
>    ```
> 
> 2. **Clang**
>    Clang 与 GCC 一样，使用 `libstdc++` 或 `libc++` 提供 TR1 支持。`libc++` 作为一个独立的标准库实现，也提供了与 TR1 兼容的功能，默认支持 C++11 和更新版本。
> 
> 3. **Microsoft Visual C++ (MSVC)**
>    在 Visual Studio 中，TR1 特性曾经被包含在 `tr1` 命名空间中。早期的 Visual C++ 版本通过 `std::tr1` 提供 TR1 特性。但自从 C++11 发布后，MSVC 逐渐将 TR1 特性移入标准库的 `std` 命名空间，并淘汰了对 `std::tr1` 的支持。例如，在 Visual Studio 2010 及之前的版本中，你可以看到 TR1 的支持，而在更新的版本中则直接使用 C++11 特性。
> 
> 4. **其他编译器和库**
>    一些嵌入式编译器或专用平台的编译器也可能实现了 TR1 特性，不过这些实现可能并不完整，或者需要依赖第三方库（如 Boost）来提供部分功能。
> 
> ### 依赖 TR1 实现的库
> 在 TR1 规范发布之前和之后，很多开发者使用第三方库来提供这些功能。最著名的就是 **Boost 库**，它是一个广泛使用的 C++ 第三方库，许多 TR1 特性最初就是从 Boost 库中提取和标准化的。
> 
> - **Boost 和 TR1**
>   Boost 提供了很多与 TR1 类似的功能，比如 `boost::shared_ptr`、`boost::tuple` 和 `boost::function` 等，这些组件后来成为 TR1 和 C++11 标准库的一部分。因此，很多在使用 TR1 特性时，也会考虑使用 Boost 库，尤其是在编译器对 TR1 支持不完整或不一致的情况下。
> 
> ### 总结
> 
> - **TR1 是标准规范**：它定义了一系列新特性和库组件，但具体实现依赖于不同的编译器和标准库供应商。
> - **编译器实现 TR1**：不同的编译器可能有不同的实现方式，并且 TR1 的实现通常在命名空间 `std::tr1` 中，而在 C++11 标准中这些特性被迁移到了 `std` 命名空间。
> - **第三方库如 Boost**：在 TR1 和 C++11 发布之前，Boost 库提供了许多类似的功能，开发者可以依赖 Boost 来获得类似于 TR1 的功能。 
>总的来说，TR1 作为过渡期的规范起到了连接传统 C++ 和现代 C++ 的桥梁作用，**而在现代编译器中，TR1 的大部分功能已经被整合到 C++11 标准库中，因此更推荐使用标准库的实现**

> 在C++中，`TR1`（Technical Report 1）是C++标准库的一个扩展，它提供了一些新特性和组件，旨在改进标准库的功能和增强对现代编程需求的支持。TR1是由ISO/IEC的一个技术小组开发的，并在C++0x标准（即C++11）之前推出，很多TR1中的功能最终被纳入了C++11标准中。
> 
> ### TR1中的主要组件
> TR1 引入了一些新特性，虽然在C++11及后续标准中它们被直接整合，但在使用早期编译器时，仍有可能看到对TR1的引用。以下是TR1中引入的一些主要组件：
> 
> 1. **智能指针 (`smart pointers`)**
>    TR1 引入了 `shared_ptr` 和 `weak_ptr`，用于更好地管理动态内存。
>    - `shared_ptr`: 引用计数的智能指针，多个指针可以共享同一个对象，当最后一个引用被销毁时，自动释放内存。
>    - `weak_ptr`: 辅助 `shared_ptr`，防止循环引用。
> 
> 2. **正则表达式 (`regex`)**
>    TR1 中引入了正则表达式的库，允许更高效地匹配字符串模式，C++11正式将它们作为标准的一部分。
>    - `std::regex`: 用于定义正则表达式的类。
>    - `std::smatch` 和 `std::cmatch`: 用于存储匹配结果的容器类。
> 
> 3. **元组 (`tuple`)**
>    TR1引入了 `std::tuple`，允许将多个类型不同的对象组合在一起形成一个元组对象，这在函数返回多个值时特别有用。
>    - `std::tuple`: 可以存储不同类型的多个值的集合。
>    - `std::get`: 用于从元组中获取值。
> 
> 4. **哈希表 (`hash table`)**
>    TR1 中的哈希表提供了 `std::unordered_map` 和 `std::unordered_set`，它们使用哈希算法来进行快速查找。C++11中正式引入了这些容器。
>    - `std::unordered_map`: 基于哈希表实现的映射容器。
>    - `std::unordered_set`: 不重复的无序集合。
> 
> 5. **随机数生成 (`random number generation`)**
>    TR1引入了一个灵活的随机数生成框架。C++11对其进行了扩展和标准化。
>    - `std::random_device`: 提供了一个硬件生成的随机数。
>    - `std::mt19937`: 梅森旋转算法，用于生成伪随机数。
> 
> 6. **数学特殊函数 (`mathematical special functions`)**
>    TR1 提供了一些高级的数学函数，比如贝塞尔函数、伽马函数、误差函数等。这些功能对一些科学计算应用特别有用。
> 
> 7. **函数对象 (`function objects`)**
>    TR1引入了 `std::function`，允许存储和调用任意的可调用对象（函数、lambda表达式、函数指针等），使函数更加灵活。
>    - `std::function`: 用于存储和调用可调用对象。
>    - `std::bind`: 用于绑定函数参数。
> 
> 8. **类型特征 (`type traits`)**
>    TR1 引入了类型特征，用于在编译期检测类型的各种特性（如是否为指针类型、是否为整型等），这在模板元编程中特别有用。
> 
> ### TR1 和 C++11 的关系
> TR1 最初是作为C++标准库的扩展引入的，但它并不是正式的标准。随着C++11的发布，TR1的绝大多数组件被纳入了C++标准库，并得到了进一步改进和扩展。因此，使用现代C++时，开发者通常直接使用C++11中的对应功能，而无需再显式依赖TR1。
> 
> ### 使用TR1的场景
> - 在旧版本的编译器（如不支持C++11的编译器）中，TR1可以作为引入上述特性的方式。
> - 对于使用较老的C++代码库，可能仍然会看到对TR1的引用，尤其是使用了特定库或特性的代码。
> 
> 总结来说，TR1在C++演进过程中起到了重要的过渡作用，提供了在正式标准化之前的一些重要功能。不过，随着C++11及后续标准的引入，大多数TR1特性已经成为现代C++的一部分。

## Item 55: Familiarize yourself with Boost

Boost offers implementations of many TR1 components, but it also offers many other libraries, too.


