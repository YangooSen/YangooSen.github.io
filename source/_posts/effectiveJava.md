---
title: effectiveJava
katex: true
date: 2024-06-11 15:31:08
excerpt: 《Effective Java》笔记
tag: java
---

# 第一章 引言

本书中大多数规则都是源于少数几条基本的规则
- 组件（即任何可重用的软件元素，例如单个方法或整个框架）的用户永远也不应该被其行为所迷惑
- 组件要尽可能小但又不能太小
- 代码应该被重用而不是被拷贝
- 组件之间的依赖性应该尽可能降低到最小
- 错误应该尽早被检测出来，最好是在编译阶段就被发现并解决

> Java 支持以下几种**基本数据类型**：
> 
> 1. **整数类型**：
>    - `byte`：占用 1 字节，范围为 -128 到 127。
>    - `short`：占用 2 字节，范围为 -32,768 到 32,767。
>    - `int`：占用 4 字节，范围为 -2^31 到 2^31-1。
>    - `long`：占用 8 字节，范围为 -2^63 到 2^63-1。
> 
> 2. **浮点数类型**：
>    - `float`：占用 4 字节，单精度浮点数，精度为 6-7 位小数。
>    - `double`：占用 8 字节，双精度浮点数，精度为 15-16 位小数。
> 
> 3. **字符类型**：
>    - `char`：占用 2 字节，用于表示单个 Unicode 字符，范围为 0 到 65,535。
> 
> 4. **布尔类型**：
>    - `boolean`：用于表示布尔值，取值范围为 `true` 或 `false`。
> 
> **引用数据类型**：
> 1. **类 (Class)**：类是对象的模板，可以定义复杂的数据结构和行为。
> 2. **接口 (Interface)**：接口定义类可以实现的一组方法，没有具体实现。
> 3. **数组 (Array)**：存储相同类型的元素的集合，支持多维数组。
> 4. **枚举 (Enum)**：定义常量集合的类型。
> 
> Java 中所有对象和数组类型都是引用类型，基本数据类型存储的是值，而引用类型存储的是对象的内存地址。

> 在 Java 中需要区分**对象**和**对象变量**。
> 
> 1. **对象**（Object）：
>    - 对象是类的一个实例，它占用内存空间并包含实际的数据和行为。通过 `new` 关键字创建。
>    - 比如：`new String("Hello")` 是一个 `String` 对象，它在堆内存中占据一块空间。
> 
> 2. **对象变量**（Object Reference or Object Variable）：
>    - 对象变量是一个引用类型的变量，它保存的是对象在内存中的地址（引用），而不是对象本身。
>    - 例如，`String str = new String("Hello");` 中的 `str` 是对象变量，它指向内存中的 `String` 对象。
> 
> ### 举个例子：
> ```java
> String str0;
> String str1 = new String("Hello"); // 创建了一个对象，str1 是对象变量
> String str2 = str1;                // str2 也是对象变量，它与 str1 指向同一个对象
> ```
> 
> 在上面的代码中：
> - `str0` 仅仅是一个对象变量，而不是一个对象，只有当`new`了之后对象变量才会**引用**一个对象（而**不是包含**一个对象）
> - `new String("Hello")` 创建了一个 `String` 对象。
> - `str1` 是指向这个对象的引用变量。
> - 当 `str2 = str1` 时，`str2` 也指向相同的对象，所以两个引用变量指向同一个内存地址。
> 
> ### 重要点：
> - Java中的对象变量（引用数据类型）就是Cpp中的对象指针，Java中的引用数据类型引用的对象，全部存储在堆中
> - 如果两个对象变量指向同一个对象，修改其中一个对象的属性可能会影响另一个对象变量看到的结果（如果是可变对象）引用数据类型就是指针，这个很好理解。
> - 引用类型变量的默认值是 `null`，这意味着变量没有指向任何对象。

> Java 方法传参时是按值传递的。对于基本数据类型，传递的是值的拷贝，函数内部对形参的修改不会影响到外部的实参。
> 
> 但是，对于引用数据类型（如对象或数组），传递的也是“值”，不过这个值是对象的引用（即对象的地址）。因此，虽然传递的是引用的副本，但通过这个引用可以访问到对象本身，所以函数内部对对象的修改将影响到外部的实参对象本身。不过，如果在函数内部让引用指向一个新的对象，外部的引用并不会受到影响。只要留意到对象引用（对象变量）和对象的区别，并坚定地认为是按值传递就不会出错
> 
> 简单来说：
> - **如果函数修改的是引用指向的对象的内容，外部会受到影响。**
> - **如果函数修改的是引用本身（指向新的对象），外部不会受到影响。**
> 
> 例如
> 
> ```java
> 
> public class functionTest {
>     static class test{
>         private int data;
>         test(){
>             data=0;
>         }
>     }
>     static void modify(test t){
>         //发生了 test t=a,这样的按值传递,t指向的是和参数指向的同一个对象
>         //指向同一个对象，修改会发生
>         t.data=1;
>         //修改了t这个新对象引用的情况，参数a并不受影响，a还是指向之前的对象
>         t=new test();
>     }
>     public static void main(String[] args) {
>         test a = new test();
>         test b=a;
>         System.out.println(a.data);  //0a
>         modify(a);
>         System.out.println(a.data);   //1
>         System.out.println(b==a); //true
>     }
> }
> 
> 
> ```

> 在 Java 中，**包级私有**（Package-Private，也叫默认访问控制或缺省访问控制）是一种访问修饰符。它的特点是：如果不显式声明为 `public`、`protected` 或 `private`，**则默认情况下，类、变量或方法的访问权限是包级私有**。这意味着该类、变量或方法只能在**同一个包**内被访问，不能被其他包中的类访问。
> 
> ### 包级私有的特点
> - **访问权限**：仅限于同一包中的类访问，包外的类无法访问。
> - **修饰符**：没有任何修饰符。例如：
>   
>   ```java
>   class MyClass {
>       // 这个类是包级私有的
>       int myField;  // 这个字段是包级私有的
>       void myMethod() {  // 这个方法是包级私有的
>           // 一些逻辑
>       }
>   }
>   ```
> 
> 在上面的代码中，`MyClass` 类、`myField` 字段和 `myMethod` 方法都是包级私有的，它们只能被位于相同包中的其他类访问，不能被其他包中的类访问。
> 
> ### 示例
> 假设有以下两个包：
> - `com.example.package1`
> - `com.example.package2`
> 
> `MyClass` 在 `com.example.package1` 包中定义：
> 
> ```java
> package com.example.package1;
> 
> class MyClass {
>     int myField;  // 包级私有字段
>     void myMethod() {  // 包级私有方法
>         System.out.println("This is a package-private method.");
>     }
> }
> ```
> 
> 在同一个包 (`com.example.package1`) 中的另一个类可以访问 `MyClass` 的字段和方法：
> 
> ```java
> package com.example.package1;
> 
> public class Test {
>     public static void main(String[] args) {
>         MyClass obj = new MyClass();
>         obj.myField = 10;  // 访问包级私有字段
>         obj.myMethod();  // 调用包级私有方法
>     }
> }
> ```
> 
> 但是，如果在另一个包 (`com.example.package2`) 中尝试访问 `MyClass`，将会导致编译错误：
> 
> ```java
> package com.example.package2;
> 
> import com.example.package1.MyClass;
> 
> public class Test {
>     public static void main(String[] args) {
>         MyClass obj = new MyClass();  // 编译错误，MyClass 对外包不可见
>         obj.myField = 10;  // 编译错误，无法访问包级私有字段
>         obj.myMethod();  // 编译错误，无法调用包级私有方法
>     }
> }
> ```
> 
> ### 总结
> - **包级私有**是一种没有显式修饰符的访问级别。
> - 只能在**同一个包**内访问，包外无法访问。


# 第二章 创建和销毁对象

## 第一条：用静态工厂方法代替构造器

> 注意区分：`Boolean` 是引用数据类型（也称为包装类型），而 `boolean` 是值类型（基本数据类型）。
> 
> 在 Java 中：
> 
> - **`boolean`** 是一种**基本数据类型**，它只表示两种可能的值：`true` 或 `false`。它的值是存储在栈上的实际布尔值，因此它是一个简单的、轻量的、不可为 `null` 的类型。
> 
> - **`Boolean`** 是一个**引用数据类型**，也叫做包装类（Wrapper Class）。它是一个对象，可以表示 `true` 或 `false`，并且可以为 `null`。包装类允许我们在需要对象的地方使用基本数据类型。`Boolean` 包含了一些实用方法，比如 `parseBoolean()` 和 `valueOf()`。
> 
> 两者的区别主要体现在以下几点：
> 
> 1. **存储位置**：
>    - `boolean` 存储在栈中，属于值类型。
>    - `Boolean` 是一个对象，存储在堆中，通过引用访问。
> 
> 2. **性能**：
>    - `boolean` 是基本类型，直接在内存中存储值，性能较好。
>    - `Boolean` 是对象，涉及到内存的分配和引用管理，因此会稍微慢一些。
> 
> 3. **`null` 值**：
>    - `boolean` 不能为 `null`，只能是 `true` 或 `false`。
>    - `Boolean` 可以为 `null`，适合用于一些情况下需要表示 "未知" 或 "未定义" 状态。
> 
> 4. **自动装箱/拆箱**：
>    - Java 支持自动装箱（autoboxing）和自动拆箱（unboxing），即可以自动将 `boolean` 转换为 `Boolean`，反之亦然。例如：
>      ```java
>      Boolean boolObj = true;  // 自动装箱，等同于 Boolean boolObj = Boolean.valueOf(true);
>      boolean boolVal = boolObj; // 自动拆箱，等同于 boolVal = boolObj.booleanValue();
>      ```
> 
> 因此，`boolean` 适用于简单、高效的逻辑操作，而 `Boolean` 则适用于需要对象引用的情况，比如在集合类（如 `List`）中使用布尔值时。


> **静态工厂方法**是一种用于创建对象的设计模式。在Java中，它是指用一个静态方法来返回类的实例，而不是使用类的构造器来直接创建对象。该方法可以位于类的内部或外部，返回的可以是新创建的对象，也可以是现有的对象。静态工厂方法和设计模式中的工厂方法不同
> 
> ### 静态工厂方法的特点
> 
> 1. **静态方法**：静态工厂方法是用`static`修饰的方法，属于类而不是实例。
> 2. **返回类型**：静态工厂方法返回类的实例，但可以灵活返回子类的对象或其他缓存的实例。
> 3. **不必每次都创建新对象**：与构造器不同，静态工厂方法可以复用现有对象，如单例模式中只返回一个实例。
> 4. **可以有更具描述性的方法名**：不像构造器只能使用类名，静态工厂方法可以根据其用途有更具描述性的名称。
> 
> ### 静态工厂方法的优点
> 
> 1. **具有描述性的名称**：相比构造器只能使用类名，静态工厂方法允许开发者为方法命名，使代码更易读。例如：
>    ```java
>    LocalDate date = LocalDate.of(2023, 10, 1);  // 更具意义的创建日期方法
>    ```
> 
> 2. **可以控制实例的创建**：静态工厂方法可以根据需要返回已经存在的实例，避免不必要的对象创建。例如，单例模式：
>    ```java
>    public class Singleton {
>        private static final Singleton instance = new Singleton();
> 
>        private Singleton() {}  // 私有化构造器，防止外部直接实例化
> 
>        public static Singleton getInstance() {
>            return instance;  // 只返回唯一的实例
>        }
>    }
>    ```
> 
> 3. **可以返回子类对象**：静态工厂方法可以返回类的子类对象，具体取决于条件。这在某些场景下比直接使用构造器更具灵活性。
>    ```java
>    public class ShapeFactory {
>        public static Shape createShape(String type) {
>            if (type.equals("circle")) {
>                return new Circle();
>            } else if (type.equals("square")) {
>                return new Square();
>            }
>            return null;
>        }
>    }
>    ```
> 
> 4. **可以进行额外的逻辑处理**：静态工厂方法可以包含额外的创建逻辑，比如验证参数、初始化默认值等。
> 
> ### 常见的静态工厂方法模式
> - `valueOf()`：返回已有对象或根据给定值创建对象。
> - `of()`：简便工厂方法，常用于不可变类型。
> - `getInstance()`：获取单例实例。
> - `newInstance()`：创建新的对象实例。
> - `getType()`：获取特定类型的实例。
> 
> ### 示例
> 
> ```java
> public class Car {
>     private String model;
>     private String color;
> 
>     private Car(String model, String color) {
>         this.model = model;
>         this.color = color;
>     }
> 
>     // 静态工厂方法，创建Car实例
>     public static Car createCar(String model, String color) {
>         return new Car(model, color);
>     }
> }
> 
> // 使用静态工厂方法代替构造器
> Car car = Car.createCar("Tesla", "Red");
> ```
> 
> ### 什么时候使用静态工厂方法
> - 当你想要更灵活的对象创建控制时。
> - 当对象的创建逻辑复杂，不希望直接通过构造器创建时。
> - 当你需要返回缓存实例或复用实例时（如单例模式）。
> - 当你想隐藏具体类的构造细节，只暴露公共接口时。

优势：
- 有名称
- 可以复用对象（享元模式）
- 可以返回任何子类对象
- 返回的对象的类可以根据参数变化（简单工厂）
- 返回的对象的类，在编写这个静态工厂方法的时候可以不存在（只返回接口类，而具体实现类可以后续再动态加载）
缺点：
- 类必须有一个public或protected的构造器，如果是private的，子类将无法继承这个类，因为子类会调用父类构造器
- 类的静态工厂方法不是必须写的，API文档也没有明确标识，因此程序员很难发现有这个方法。不过有一些惯用名称
在Java中，静态工厂方法常常使用一些约定俗成的命名模式，以提高代码的可读性和一致性。常见的惯用名称包括：

> 1. **from**  
>    用于描述将一个类型转换为当前类型的工厂方法。  
>    示例：`Date.from(Instant instant)`
> 
> 2. **of**  
>    用于根据多个元素构造当前类型的实例，通常是直接根据参数生成对象的简洁方法。  
>    示例：`ArrayList<String> arrayList = new ArrayList<>(List.of("a", "b", "c"));`
> 
> 3. **valueOf**  
>    用于将某个值转换为当前类型的实例，常见于枚举和包装类。  
>    示例：`Integer.valueOf(String s)`
> 
> 4. **instance** / **getInstance**  
>    用于返回类的一个实例，可能是一个单例或其他共享实例。有时可能通过方法参数描述 
>    示例：`Calendar.getInstance()`
> 
> 5. **create/newInstance**  
>    用于显式地创建一个**新**实例，通常用于某些工厂类。  
>    示例：`ThreadFactory.create()`
> 
> 7. **getType**  
>    用于返回当前类中的某个特定类型实例，`Type`通常是一个具体的子类或接口。  
>    示例：`NumberFormat.getCurrencyInstance()`
> 
> 8. **newType**  
>    类似于`getType`，但更明确地表达了创建一个新实例。  
>    示例：`Executor.newCachedThreadPool()`
> 
> 这些命名惯用法能够帮助开发者迅速理解工厂方法的意图，从而提高代码的可读性和易用性。

## 第二条：遇到多个构造器参数时要考虑构建器

当类的构造器参数很多的时候，考虑使用建造者模式。建造者模式分离出了**创造产品的过程**。先得到一个builder对象，然后在builder对象上调用类似setter的方法设置具体的很多参数，最后build返回出具体的产品（例如StringBuilder）。builder的优势在于可以有多个可变参数，并且还可以多次调用同一个setter，灵活性更高。但是在对性能要求高的场景下，为了创建builder可能会花费额外的很多时间，因此在构造器参数很多时再使用

> 在Java中，Java并没有像一些其他编程语言（如Python）那样明确的`self`或`this`类型。然而，Java通过其他方式实现类似的功能，通常通过泛型（generics）来模拟一种"self"类型。我们常常需要这种"self"类型来表示某个类中的方法返回的是当前实例类型的对象，尤其是在链式调用的设计中。下面详细解释这一点。
> 
> ### 1. 问题背景
> Java中的类方法中可以使用`this`关键字引用当前对象，但`this`的类型是当前类的具体类型。而在面向对象编程中，我们有时需要在基类中定义一个返回当前对象的泛型方法，并希望子类调用时也能返回子类的实例。如果直接返回`this`，在继承层次结构中可能导致子类调用时返回父类的实例，而不是子类的实例，这就导致了问题。
> 
> ### 2. 解决方案：使用递归泛型，即泛型T是递归的
> 为了在基类中返回当前的具体子类类型（模拟“self”类型的效果），我们可以使用递归泛型（Curiously Recurring Template Pattern，CRTP）。递归泛型允许我们在定义一个类时，**将子类的类型作为泛型参数传递给基类**。这样基类的某些方法可以返回子类的类型。
> 
> 如果在父类中直接返回`this`，在子类中调用该方法时，返回的类型将是父类的类型，而不是子类的类型。这可能导致方法链不再返回子类的实例，从而影响代码的灵活性和可扩展性。以下是一个示例来说明这一点：
> 
> #### 示例代码
> 
> ```java
> class Parent {
>     // 直接返回this，类型为Parent
>     public Parent getThis() {
>         return this;
>     }
> }
> 
> class Child extends Parent {
>     // 子类特有的方法
>     public void childMethod() {
>         System.out.println("This is a method in Child.");
>     }
> }
> 
> public class Main {
>     public static void main(String[] args) {
>         Child child = new Child();
> 
>         // 调用父类的getThis方法
>         Parent parentRef = child.getThis(); // 返回的是Parent类型
> 
>         // 试图调用Child的方法
>         // parentRef.childMethod(); // 这将导致编译错误，parentRef是Parent类型
> 
>         // 可以调用Parent的方法
>         System.out.println("Returned instance type: " + parentRef.getClass().getSimpleName());
>     }
> }
> ```
> 
> #### 输出结果
> ```
> Returned instance type: Child
> ```
> 
> #### 解释
> 1. **`Parent`类**：定义了一个方法`getThis()`，直接返回`this`，但返回类型是`Parent`。
> 2. **`Child`类**：继承自`Parent`，并增加了一个方法`childMethod()`。
> 3. **`Main`类**：在`main`方法中创建了`Child`的实例`child`，调用了`getThis()`，并将结果赋值给`Parent`类型的引用`parentRef`。
> 
> #### 问题
> - 当调用`child.getThis()`时，返回的是`Parent`类型的引用`parentRef`。虽然`parentRef`实际上指向的是`Child`的实例，但由于返回类型是`Parent`，所以编译器不允许我们调用`Child`特有的方法`childMethod()`，这就限制了代码的灵活性。
> - 如果我们需要在`parentRef`上调用`childMethod()`，就会出现编译错误，因为`parentRef`被视为`Parent`类型。
> 
> #### 解决方案
> 为了在返回类型上保留子类的信息，我们可以使用递归泛型：
> 
> ```java
> //这里泛型T是递归的
> class Parent<T extends Parent<T>> {
>    //也可以父类不实现这个方法，只提供接口，由每个继承的子类重写这个方法，他们的方法体直接return this即可，不需要强制类型转换
>     public T getThis() {
>         return (T) this; // 强制转换为子类类型
>     }
> }
> 
> class Child extends Parent<Child> {
>     public void childMethod() {
>         System.out.println("This is a method in Child.");
>     }
> }
> 
> public class Main {
>     public static void main(String[] args) {
>         Child child = new Child();
> 
>         // 调用父类的getThis方法，返回的是Child类型
>         Child childRef = child.getThis(); // 返回Child类型
> 
>         // 现在可以调用Child的方法
>         childRef.childMethod(); // 这将成功调用Child的方法
>     }
> }
> ```
> 
> 在这个修改后的版本中，`getThis()`方法返回的是具体子类的类型，使得代码的灵活性得到了提升。通过使用递归泛型，我们确保了方法调用的正确性和类型安全性。
> 
> 
> 
> 
> ### 3. 递归泛型的工作原理
> - **Base类**：定义了一个泛型类，`Base<T>`，其中`T`代表具体的子类类型。`self()`方法返回当前实例，并将其转换为`T`类型。
> - **Derived类**：通过继承`Base<Derived>`，将自身`Derived`类型传递给基类的泛型参数，这样基类中的`self()`方法在`Derived`类中调用时会返回`Derived`类型的实例。
> 
> ### 4. 优势
> - **链式调用**：递归泛型允许在基类中定义方法，返回子类的类型，因此可以在子类中使用链式调用，避免每次返回父类的类型而还需要转换类型，可以直接方法链接。
> - **灵活性和可扩展性**：可以轻松扩展到更复杂的继承结构，每个子类都可以继承并复用基类的逻辑，同时保持方法调用的正确类型。
> 
> ### 5. 限制
> 这种模式在Java中虽然常见，但它依赖于强制类型转换（如上面的`(T) this`），这虽然在大多数情况下是安全的，但强制转换始终带有一定的风险。如果滥用，可能在某些情况下会导致`ClassCastException`。
> 
> 通过递归泛型，我们有效地模拟了"self"类型，使得返回值能够根据具体的子类自动调整类型，在面向对象设计中实现了更灵活和类型安全的接口设计。


> 在Java中，协变返回类型（Covariant Return Type）指的是子类方法重写父类方法时，可以返回父类方法返回类型的子类类型。这种特性允许在继承中保持更具体的返回类型，从而增强了灵活性和可读性。
> 
> ### 示例
> 
> 考虑以下代码示例：
> 
> ```java
> class Animal {
>     public Animal makeSound() {
>         System.out.println("Some sound");
>         return new Animal();
>     }
> }
> 
> class Dog extends Animal {
>     @Override
>     public Dog makeSound() {
>         System.out.println("Bark");
>         return new Dog(); // 返回类型为Dog，这是Animal的子类
>     }
> }
> ```
> 
> 在这个例子中：
> 
> - `Animal`类有一个`makeSound()`方法，返回类型为`Animal`。
> - `Dog`类重写了`makeSound()`方法，返回类型为`Dog`，这使得`makeSound()`方法的返回类型从`Animal`变为`Dog`，即实现了协变返回类型。
> 
> ### 优点
> 
> 1. **类型安全**：子类可以返回更具体的类型，避免了类型转换的麻烦。
> 2. **增强可读性**：代码更加直观，能够清晰地表明方法的具体返回类型。
> 
> ### 注意事项
> 
> - 协变返回类型只能在方法重写中使用。
> - 返回类型必须是父类返回类型的子类或相同类型。
> 
> 这种特性在面向对象编程中是一个很有用的特性，尤其是在涉及多态性时，可以使代码更具灵活性。
## 第三条：用私有构造器或者枚举类强化Signgleton属性

下面是单例的实现方法：

### 饿汉静态常量
```java
//饿汉式(静态常量)
class Singleton {
    //    1.构造器私有化
    private Singleton() {

    }

    //2.本类内部创建对象实例
    private final static Singleton instance = new Singleton();

    //提供一个公共静态方法，返回实例对象
    public static Singleton getInstance() {
        return instance;
    }
}
```
另一种考虑是直接将成员instance设为public，这样就不需要通过getInstance来获得，除非有能想到的扩展优势，一般用这样更简单的方法。饿汉式如果从未使用就会造成内存浪费，但也要注意拥有特权的用户可以借助AccessibleObject.setAccessible方法，通过反射调用私有构造器

### 饿汉静态代码块

```java
//饿汉式(静态代码块)
class Singleton1 {
    //    1.构造器私有化
    private Singleton1() {

    }

    //2.本类内部创建对象实例
    private final static Singleton1 instance;/*final修饰的属性必须显式赋值*/

    static {
        instance = new Singleton1();
    }

    //提供一个公共静态方法，返回实例对象
    public static Singleton1 getInstance() {
        return instance;
    }
}

```
> 在Java中，类的生命周期包括**加载（Loading）、链接（Linking）、初始化（Initialization）、使用（Using）**和**卸载（Unloading）**五个主要阶段。以下是对每个阶段的详细描述，尤其是代码块的调用顺序和时机：
> 
> #### 1. 类的加载（Loading）
> 当类第一次被使用时，Java类加载器会将类文件从磁盘或其他存储设备中加载到内存中。加载过程主要包括：
> 
> - **类加载器（ClassLoader）**找到对应的`.class`文件，并将其加载到内存中形成**字节码**。
> - 类加载完成后，会生成**Class对象**，它表示该类在JVM中的结构和元数据。
> 
> > 类的加载是由类加载器（ClassLoader）完成的，类加载器有很多种，包括**启动类加载器（Bootstrap ClassLoader）**、**扩展类加载器（Extension ClassLoader）**、**应用类加载器（Application ClassLoader）**等。
> 
> #### 2. 类的链接（Linking）
> 链接阶段将类的字节码进行验证、准备和解析，确保类可以正确执行。这一阶段包括三个步骤：
> 
> - **验证（Verification）**：确保类的字节码格式正确，并且不会违反JVM的安全规则（如类型安全性）。
> - **准备（Preparation）**：**为类的静态变量分配内存**，并初始化为默认值（如`int`类型为0，`boolean`类型为`false`等）。
> - **解析（Resolution）**：将类的符号引用（如类、方法、字段的名字）解析为直接引用，指向具体的内存地址。
> 
> #### 3. 类的初始化（Initialization）
> 在链接之后，类的初始化阶段会执行类中的**静态代码块**、**静态变量赋值**等操作。这是类生命周期中最关键的阶段，以下是初始化过程中的代码执行顺序：
> 
> 1. **静态变量和静态代码块**：
>     - **静态变量**：类的静态变量按在类中定义的顺序依次初始化。例如：
>       ```java
>       public static int a = 10;
>       ```
>     - **静态代码块**：静态代码块会在类加载时执行。多个静态代码块按顺序执行。例如：
>       ```java
>       static {
>           System.out.println("Static block 1");
>       }
>       static {
>           System.out.println("Static block 2");
>       }
>       ```
>     - 以上代码在类加载时会按顺序输出`Static block 1`和`Static block 2`。
>   
> 2. **构造代码块**：
>     - 构造代码块是在每次创建对象时执行的代码块。构造代码块的执行顺序在**构造方法之前**。例如：
>       ```java
>       {
>           System.out.println("Instance block");
>       }
>       ```
>       每次创建对象时，这段代码都会在构造方法之前执行。
> 
> 3. **构造方法**：
>     - 构造方法是每次创建对象时执行的，用于初始化对象的实例变量。例如：
>       ```java
>       public ClassName() {
>           System.out.println("Constructor");
>       }
>       ```
>       构造方法的执行顺序是在所有的静态代码块和构造代码块之后。
> 
> #### 4. 类的使用（Using）
> 在类初始化完成后，该类即可用于创建对象或调用其静态方法。静态方法的调用只会触发类的加载和初始化，而实例方法则需要在类初始化后创建对象，然后调用。
> 
> 例如：
> ```java
> ClassName obj = new ClassName(); // 调用构造函数
> ClassName.staticMethod();        // 调用静态方法
> ```
> 
> #### 5. 类的卸载（Unloading）
> 当类不再被使用，且没有任何类的实例或引用存在时，JVM会在**垃圾回收**时将该类从内存中卸载。卸载通常发生在没有引用指向类的情况，且类加载器也没有再持有该类的引用时。
> 
> 卸载的过程由JVM自动管理，程序员不需要主动干预。类的卸载可以回收类所占用的内存空间。
> 
> ---
> 
> ### 调用顺序总结
> 1. **类加载**时，JVM会根据需要加载类的字节码。
> 2. **类链接**阶段包括验证、准备和解析。
> 3. **类初始化**时：
>    - 静态变量和静态代码块按照它们在代码中出现的顺序执行。
>    - 每次创建对象时，构造代码块会先于构造方法执行。
> 4. **类使用**时，静态方法或实例方法会在初始化之后调用。
> 5. 最后，类在**卸载**时从内存中移除。
### 懒汉线程不安全

```java
class Singleton {
    private static Singleton instance;

    private Singleton() {

    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

```
多线程同时进入if判断会生成多个实例
### 懒汉线程安全
```java

class Singleton {
    private static Singleton instance;

    private Singleton() {

    }

    //加入同步处理的代码，解决线程安全问题。但每个线程每次都要同步，实际上这个方法只需执行一次实例化代码，后面都是直接return的。这样效率太低
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

```
每次调用getInstance时都只能有一个线程调用，效率低，注意不能只把synchronized加载if里面，那样还是会有多个线程进入if

> 在 Java 中，`synchronized` 是一种关键字，用于实现线程同步，确保多线程环境下对共享资源的安全访问。它通过锁机制来防止多个线程同时执行某些代码块，从而避免数据不一致或竞态条件。以下是对 `synchronized` 的详细解释：
> 
> 1. `synchronized` 的基本概念
> 在多线程环境中，如果多个线程同时访问和修改共享资源（如变量、对象等），可能会导致数据不一致。`synchronized` 用来确保同一时刻只能有一个线程访问某个特定的代码块或方法，其他线程必须等待，直到该线程执行完毕释放锁。
> 
> 2. `synchronized` 的用法
> `synchronized` 主要可以用于两种场景：
> 
> - **同步方法**：将整个方法设为同步方法，保证在同一时刻只有一个线程能够执行这个方法。
> 
>   ```java
>   public synchronized void someMethod() {
>       // 同步的代码
>   }
>   ```
> 
> - **同步代码块**：可以在方法中指定某段代码块为同步代码块，锁定某个对象，只有获取到该对象的锁后才能执行这段代码。
> 
>   ```java
>   public void someMethod() {
>       synchronized (this) {  // 锁定当前对象
>           // 同步的代码
>       }
>   }
>   ```
> 
>   或者：
> 
>   ```java
>   public void someMethod(Object lock) {
>       synchronized (lock) {  // 锁定某个对象
>           // 同步的代码
>       }
>   }
>   ```
> 
> 3. `synchronized` 的锁机制
> 每个对象都有一个与之相关联的**内部锁（monitor）**。当一个线程进入 `synchronized` 修饰的方法或代码块时，它必须先获得该对象的锁。一旦锁被线程占有，其他线程就不能再进入同一对象的 `synchronized` 方法或代码块，必须等待锁被释放。
> 
> - **实例方法的锁**：当 `synchronized` 修饰实例方法时，锁的是当前对象实例（即 `this`），只有一个线程可以访问该对象的同步实例方法。
> 
> - **静态方法的锁**：当 `synchronized` 修饰静态方法时，锁的是该类的 Class 对象，因为Java 中的 Class 对象是 JVM 中用来描述类的元数据（例如类的名称、方法、字段等），它是一个运行时的对象表示，每个加载的类在 JVM 中都会对应唯一的一个 Class 对象，并且该类的所有实例共享这个 Class 对象。所有该类的实例都共享这个锁。
> 
> - **代码块的锁**：使用 `synchronized` 修饰代码块时，锁的是指定的对象。可以通过锁不同的对象来实现更细粒度的同步控制。
> 
> 
> 4. `synchronized` 和重入锁（Reentrant Lock）
> `synchronized` 是一种简单的同步机制，但在一些复杂的场景中，Java 提供了 `java.util.concurrent.locks.ReentrantLock` 来替代 `synchronized`，它提供了更灵活的同步机制，允许显式获取和释放锁，并支持更高级的特性如公平锁和条件变量。


### 双重检查

```java
class Singleton {
    //    volatile:使变量一有修改立马提交到主存里面去
    private static volatile Singleton instance;

    private Singleton() {

    }

    //加入双重检查代码，解决线程安全问题，同时解决懒加载问题
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }

            }

        }
        return instance;
    }
}

```
只有第一次使用（instance==null）的时候才用同步控制线程，非空直接返回。第一次if是为了效率，可以直接返回；第二次if是为了确保同步的实现，如果没有第二个if可能会：第一次使用时两个线程同时进入if，第一个线程获得锁创建实例后归还锁，然后第二个线程得到锁之后又创建实例。但是必须注意volatile防止指令重排，确保写操作在读操作之前完成的作用。JVM创建对象的时候先分配空间赋予域初始值，然后才调用构造器方法写内存。如果某线程分配空间之后，还没有构造赋值就被另一个线程抢占，那么返回的对象域值将只是初始值，加了volatile之后，一旦调用new，JVM会保证对象完全初始化结束（写）之后才允许其他线程访问（读），防止半初始化的对象

![volatile](v_s.jpg)

> JVM 创建对象时，确实会先分配内存并为对象的实例变量（成员变量）赋予默认初始值，然后再调用构造器方法执行进一步的初始化。这是对象创建过程中的标准步骤。
> 
> 具体来说，对象的创建过程大致可以分为以下几个阶段：
> 
> #### 1. **内存分配（Memory Allocation）**
> - 当使用 `new` 关键字创建一个对象时，JVM 首先会在堆内存中为该对象分配内存。
> - 分配的内存大小取决于该对象所属的类的定义，包括所有实例变量所需的空间。
> - 此时，JVM 会为每个实例变量分配空间，并按照其类型设置默认初始值（而不是显式赋值）。
>   - **基本数据类型**的默认初始值：
>     - `int`、`short`、`byte`、`long`：默认为 `0`
>     - `float`、`double`：默认为 `0.0`
>     - `char`：默认为 `'\u0000'`（null 字符）
>     - `boolean`：默认为 `false`
>   - **引用类型**的默认初始值：所有引用类型（对象、数组等）的默认值是 `null`。
> 
> #### 2. **执行构造方法前的默认初始化**
> - 在内存分配和默认值赋值完成后，对象的内存空间已经分配好了，并且所有实例变量都被设置为默认值。但此时对象尚未完成构造。
>   
> ### 3. **调用构造器方法（Constructor Invocation）**
> - 接下来，JVM 调用对象的构造方法（`constructor`），该方法可以进一步对对象进行初始化操作。
>   - **在构造器中**，你可以为实例变量赋值或者进行更复杂的初始化操作（如调用其他方法、进行逻辑运算等）。
>   - 如果构造器中没有对某个实例变量赋值，那么该变量将保留前一步中的默认初始值。
> 
> #### 4. **对象完成初始化**
> - 当构造器方法执行完毕，内存中的对象才算真正完成初始化。此时，对象的引用将指向分配的内存，可以返回给调用者，且可以被使用。
> 
> 
> #### **总结**
> 是的，JVM 在创建对象时会按照以下顺序操作：
> 1. **分配内存**：为对象在堆中分配空间。
> 2. **默认初始化**：为对象的所有实例变量赋予默认初始值。
> 3. **调用构造器**：通过构造器方法进行显式初始化，覆盖默认初始值或进行进一步的初始化。
> 4. **完成初始化**：构造器执行完毕后，内存中的对象初始化完成。

> 对于对象的创建，使用 volatile 可以确保对象的引用被分配并指向内存区域时，对象已经完全初始化，从而避免其他线程看到“半初始化”的对象。



[懒汉中volatile的关键作用](https://zhuanlan.zhihu.com/p/111691225)


> 在Java中，`volatile` 关键字用于修饰变量，以确保该变量在多线程环境中可见，并防止指令重排序。它是一种轻量级的同步机制，主要用于解决多线程下的可见性问题，而不是原子性问题。下面详细解释一下 `volatile` 的核心概念：
> 
> 1. **内存可见性（Memory Visibility）**
> 在Java中，线程之间的通信是通过主内存（Main Memory）完成的。每个线程都有自己的工作内存（Thread Local Memory），其中保存着从主内存中读取到的变量副本。如果一个线程对某个变量进行了修改，其他线程可能并不能立即看到这个修改，除非这个修改被刷新回主内存，而其他线程从主内存重新读取该变量。
> 
> `volatile` 修饰的变量可以保证一个线程对该变量的修改会立刻被刷新到主内存中，而其他线程读取时会从主内存中读取最新的值。这样可以确保多线程对该变量的修改是可见的。确保写操作一定在读操作之前。
> 
> 2. **禁止指令重排序（Instruction Reordering）**
> 编译器和CPU在执行代码时，可能为了优化性能，会对指令进行重排序。但这种重排序有时会导致多线程程序中的并发问题。
> 
> `volatile` 关键字可以防止指令重排序。在使用 `volatile` 修饰的变量时，所有对这个变量的写操作都不会与之前的读写操作重排序；所有对这个变量的读操作也不会与之后的读写操作重排序。
> 
> 3. **示例**
> ```java
> class MyClass {
>     private volatile boolean flag = false;
> 
>     public void setFlagTrue() {
>         flag = true; // 写操作
>     }
> 
>     public void doSomething() {
>         while (!flag) {
>             // 线程会不断检查flag的值
>         }
>         System.out.println("Flag is true, exit loop");
>     }
> }
> ```
> 在这个例子中，两个线程分别执行 `setFlagTrue()` 和 `doSomething()`。如果 `flag` 没有被 `volatile` 修饰，那么一个线程对 `flag` 的修改可能不会及时被另一个线程看到，导致 `doSomething()` 方法中的循环无法退出。但使用 `volatile` 后，`flag` 的修改会立即对其他线程可见，确保程序按预期执行。
> 
> 4. **`volatile`的局限性**
> - **不保证原子性**：`volatile` 仅保证可见性和有序性，但它不能保证操作的原子性。例如，对 `volatile` 修饰的 `int` 变量执行 `i++` 操作时，它仍然不是原子的，因为 `i++` 实际上是三步操作：读取、加1、写回。尽管读取和写回都能确保可见性，但这三步之间可能被其他线程打断，导致竞态条件。
> 
> - **轻量级同步**：相比于 `synchronized`，`volatile` 不会造成线程的阻塞。因此，它适用于某些轻量级的同步场景，但不能替代 `synchronized` 实现复杂的线程同步。
> 
> ### 5. **适用场景**
> `volatile` 适用于以下场景：
> - 变量在多个线程之间共享，且被频繁读写。
> - 不需要通过 `volatile` 变量实现复杂的同步逻辑，仅仅是确保线程间的可见性。
> - 例如标志位、状态标志等简单的同步机制。
> 
> 总的来说，`volatile` 提供了一种在多线程环境下保证变量可见性和顺序性的方法，但它不能替代锁机制来解决原子性问题。在并发编程中，`volatile` 适合用来修饰一些轻量级、无需复杂同步的共享状态变量。


### 静态内部类

```java
class Singleton {

    private static Singleton instance;

    private Singleton() {

    }

    //    静态内部类,该类中有一个静态属性Singleton
    private static class SingletonInstance {
        private static final Singleton INSTANCE = new Singleton();
    }

    //直接返回SingletonInstance.INSTANCE
//    注意：上面的静态内部类不会随着主类加载就加载，而是会在下面的方法执行时才加载。
    public static Singleton getInstance() {

        return SingletonInstance.INSTANCE;
    }
}

```

只有 getInstance 被调用时，静态内部类才会第一次被JVM加载，且以线程安全的模式初始化好静态内部类的静态域

> 在 Java 中，内部类和静态内部类的加载时机有所不同，它们的加载时机由 JVM 控制，具体取决于它们的定义方式和使用场景。以下是对两者的详细描述：
> 
> #### 1. **内部类（Non-static Inner Class）的加载时机**
> 内部类是与外部类关联的类，它不能独立存在，必须通过外部类的实例来创建和访问。其加载时机如下：
> 
> - **依赖外部类的实例：** 内部类的实例是依赖外部类的实例而存在的。换句话说，只有在外部类的实例被创建后，内部类才可能被创建。内部类会在它被实例化时进行加载，而不会在外部类加载时一同加载。
> - **懒加载机制：** 内部类只有在被实际使用时才会加载，JVM 不会在外部类加载时就加载内部类。这意味着，外部类的加载和内部类的加载是相互独立的，只有当内部类的代码被引用时，它才会被加载到内存中。
> 
> **示例：**
> ```java
> public class OuterClass {
>     class InnerClass {
>         public void display() {
>             System.out.println("Inner class method");
>         }
>     }
> 
>     public static void main(String[] args) {
>         OuterClass outer = new OuterClass();  // 创建外部类实例
>         OuterClass.InnerClass inner = outer.new InnerClass();  // 创建内部类实例
>         inner.display();
>     }
> }
> ```
> 在上面的代码中，`InnerClass` 只有在 `main` 方法中通过外部类实例 `outer` 调用时才会被加载和实例化。
> 
> #### 2. **静态内部类（Static Nested Class）的加载时机**
> 静态内部类（有时称为嵌套类）与普通的内部类不同，它不依赖于外部类的实例，类似于静态变量和静态方法。静态内部类的加载时机如下：
> 
> - **与外部类的实例无关：** 静态内部类与外部类的实例没有任何依赖关系。它可以独立于外部类的实例进行创建。即使外部类的实例不存在，也可以创建静态内部类的实例。
> - **外部类加载时不加载静态内部类：** 静态内部类在外部类加载时不会一同加载。只有在首次引用静态内部类时，JVM 才会对其进行加载。这与普通的静态变量和方法的加载机制类似，只有在需要使用时才会进行加载。
> 
> **示例：**
> ```java
> public class OuterClass {
>     static class StaticNestedClass {
>         public void display() {
>             System.out.println("Static nested class method");
>         }
>     }
> 
>     public static void main(String[] args) {
>         OuterClass.StaticNestedClass nested = new OuterClass.StaticNestedClass();  // 创建静态内部类实例
>         nested.display();
>     }
> }
> ```
> 在这个例子中，`StaticNestedClass` 只有在 `main` 方法中被首次引用时才会被加载，而不会在 `OuterClass` 加载时被加载。
> 
> #### 3. **两者的对比总结**
> - **实例依赖：** 普通内部类依赖外部类的实例，而静态内部类则不依赖外部类的实例。
> - **加载时机：** 普通内部类只有在被实际使用时才会加载，而静态内部类也是在首次使用时才加载，但不依赖外部类的实例。
> - **内存占用：** 普通内部类可能会占用更多的内存，因为它需要与外部类的实例关联。而静态内部类由于不依赖外部类的实例，通常占用较少的内存。
> 
> #### JVM 加载机制补充
> JVM 在类的加载过程中，一般会按照以下步骤进行：
> - **加载（Loading）：** JVM 读取类文件的二进制数据，将其加载到内存中。
> - **链接（Linking）：** 包括验证、准备（为静态变量分配内存并初始化默认值）和解析（将符号引用转化为直接引用）。
> - **初始化（Initialization）：** 执行类的初始化，包括静态变量的赋值和静态代码块的执行。
> 
> 静态内部类的静态变量和静态代码块也会在首次使用时进行初始化，而普通内部类则不会涉及静态代码块和静态变量的初始化。

### 枚举类

```java
//枚举，推荐使用
enum Singleton {
    INSTANCE;/*属性*/
}

```
更加简洁，且提供了反序列化，即使面对复杂的序列化和反射也能保证单例。唯一的缺点是枚举类已经默认有了继承，这样实现单例无法让他继承任何父类

> Java 中的枚举类（`enum`）是一种特殊的类，用来定义一组固定的常量。**枚举的每个成员都是该类的一个单实例**，通常用于表示一组有限的可能值，比如一周的天数、交通信号灯的颜色等。枚举不仅限于简单的常量，它们还可以包含字段、方法、构造器，使其具备更强大的功能。
> 
> 1. 枚举的定义和基本用法
> 定义枚举类非常简单，可以像这样定义：
> 
> ```java
> public enum Day {
>     MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
> }
> ```
> 
> 上面定义了一个表示星期几的枚举类型 `Day`。每个大写字母表示的成员即为常量。使用时，可以通过以下方式引用这些常量：
> 
> ```java
> Day day = Day.MONDAY;
> ```
> 
> 2. 枚举的特点
> - **类型安全**：枚举类使代码更具可读性，并避免使用像 `int` 或 `String` 这样的通用类型来表示有限集合。
> - **有序性**：每个枚举值都有一个隐式的 `ordinal()` 方法，返回其定义时的顺序，从 0 开始。例如，`Day.MONDAY.ordinal()` 会返回 0，`Day.TUESDAY.ordinal()` 会返回 1。
> - **值比较**：枚举值可以使用 `==` 比较，因为它们是常量且是单例模式（每个值只有一个实例）。
> 
> 3. 带字段和构造器的枚举
> 枚举不仅限于简单的常量值，还可以像类一样添加字段、构造方法和方法。以下是一个更复杂的枚举示例：
> 
> ```java
> public enum TrafficLight {
>     RED(30), GREEN(60), YELLOW(10);
>     
>     private final int duration;  // 秒数
> 
>     // 枚举的构造器是私有的
>     TrafficLight(int duration) {
>         this.duration = duration;
>     }
> 
>     public int getDuration() {
>         return duration;
>     }
> }
> ```
> 
> 这里 `TrafficLight` 枚举包含了一个字段 `duration`，表示交通信号灯持续的时间。每个枚举值 `RED`, `GREEN`, `YELLOW` 都传递了不同的持续时间。可以通过方法 `getDuration()` 获取每个信号灯的时间：
> 
> ```java
> TrafficLight light = TrafficLight.RED;
> System.out.println(light.getDuration());  // 输出：30
> ```
> 
> 4. 枚举中的方法
> 除了可以定义自己的方法，所有枚举类型还隐式继承了 `java.lang.Enum` 类中的一些方法：
> 
> - `name()`：返回枚举常量的名字，形式是字符串。
> - `ordinal()`：返回枚举常量在枚举类中的顺序，从 0 开始。
> - `values()`：返回包含所有枚举常量的数组。
> - `valueOf(String name)`：根据名称返回相应的枚举常量。
> 
> 例如：
> 
> ```java
> for (TrafficLight light : TrafficLight.values()) {
>     System.out.println(light + " lasts for " + light.getDuration() + " seconds.");
> }
> ```
> 
jdk中Runtime类利用第一种饿汉式实现了单例模式

## 第四条：通过私有构造器强化不可实例化的能力

以s结尾的静态工具类不希望被实例化，由于当类不存在显式构造器的时候编译器会生成默认的构造器，因此需要声明一个private的构造器，然后在构造器内部抛出异常，防止类内部不小心调用构造器。确定是这个类将不能再被继承，因为类构造器都会调用超类的构造器

## 第五条：优先考虑依赖注入来引用资源

在 Java 中，**依赖注入（Dependency Injection, DI）** 是一种设计模式，用来减少类之间的耦合度。其核心思想是将对象的依赖关系从代码中硬编码的形式剥离出来，通过外部机制来动态提供依赖对象。这通常通过框架来实现，比如 Spring 框架。

依赖注入有三种常见的方式：
1. **构造器注入**：通过类的构造方法将依赖传递给对象。
   ```java
   public class UserService {
       private UserRepository userRepository;
       
       // 依赖通过构造器传入
       public UserService(UserRepository userRepository) {
           this.userRepository = userRepository;
       }
   }
   ```

2. **Setter 注入**：通过 setter 方法来注入依赖。
   ```java
   public class UserService {
       private UserRepository userRepository;
       
       // 依赖通过 setter 方法注入
       public void setUserRepository(UserRepository userRepository) {
           this.userRepository = userRepository;
       }
   }
   ```

3. **接口注入**（相对较少使用）：通过实现某个接口，框架会调用实现类的方法来注入依赖。

### 为什么使用依赖注入？

1. **降低耦合性**：类与类之间的依赖关系由外部注入，避免了直接实例化依赖对象，便于后期的扩展和维护。
2. **提高可测试性**：依赖注入允许你更轻松地对单个类进行单元测试，因为你可以在测试中传入 mock 或者 stub 对象。
3. **简化代码**：特别是在 Spring 这样的框架中，DI 管理了对象的生命周期和依赖关系，你只需专注于业务逻辑。

Spring 是最常见的依赖注入框架，通过其 IoC 容器（Inversion of Control，控制反转容器）来实现依赖注入。

## 第六条：避免创建不必要的对象


### 字符串字面量

在 Java 中，`String` 对象的确是不可变的（immutable），并且会存储在常量池（String Pool）中。

1. **不可变性**：`String` 类被设计为不可变的对象，即一旦创建，字符串的内容就无法更改。任何修改操作（例如拼接、替换等）都会创建一个新的字符串对象，而不会改变原有对象的值。这种不可变性确保了线程安全，使字符串可以在多线程环境中安全共享。

2. **字符串常量池**：Java 将所有直接用双引号创建的**字符串字面量**储存在常量池中（例如 `"hello"`）。当程序中使用相同内容的字符串时，Java 会在常量池中查找是否已有相同内容的字符串，如果有，则直接返回引用，不会创建新的对象。这样可以减少内存的使用，尤其是在大量重复字符串的情况下。 

3. **new 关键字创建的字符串**：使用 `new String("example")` 会在堆内存中创建一个新的 `String` 对象，而不会使用常量池中的字符串。

在 Java 中，`String a = "a";` 和 `String b = new String("b");` 是创建字符串对象的两种不同方式，区别主要体现在以下几点：

1. **内存分配**：
   - `String a = "a";`：字符串 `"a"` 是一个字面量，直接保存在**字符串常量池**中。如果常量池中已有 `"a"`，则 `a` 会直接引用该对象，不会创建新的对象。这种方式更节省内存，因为相同的字符串字面量只会在常量池中存在一份。
   - `String b = new String("b");`：使用 `new` 关键字会强制在**堆内存**中创建一个新的 `String` 对象，即使 `"b"` 已存在于字符串常量池中。这样一来，即使堆中和常量池中的字符串内容相同，它们也会是不同的对象。

2. **对象数量**：
   - `String a = "a";`：当使用这种方式时，如果常量池中已经有 `"a"`，不会创建新对象；如果没有，才会在常量池中添加 `"a"`。
   - `String b = new String("b");`：这行代码实际上会在常量池和堆内存中分别创建或引用一个 `"b"`，即可能会涉及到两个不同的对象。堆中的 `b` 是一个新的对象，而常量池中的 `"b"` 是之前就存在的。

3. **== 和 equals 比较**：
   - `==`：`==` 比较的是两个对象的**引用（即地址）**是否相同。对于 `String a = "a";` 和 `String b = new String("b");`，即使它们内容相同，`a == b` 也会返回 `false`，因为它们引用的对象在不同的内存区域。
   - `equals()`：`equals` 方法比较的是两个字符串的**内容**是否相同。无论是 `a.equals("a")` 还是 `b.equals("b")`，只要内容相同，`equals()` 都会返回 `true`。

总结来说：

- `String a = "a";` 使用的是字符串常量池，具有内存优势。
- `String b = new String("b");` 在堆内存中创建了新的对象，即使内容相同，它和常量池中的 `"b"` 是不同的对象。如果常量池中原本不存在字面量对象，会先创建字面量，再用创建堆中的String对象:w

```java
String s1 = new String("hello");
String s2 = "hello";
System.out.println(s1 == s2);// false
System.out.println(s1.equals(s2));// true
```

堆中的 `String` 对象并不会直接“指向”常量池中的对象，而是**复制了常量池中字符串的字符内容**（也就是字符数组）来初始化自己。所以，它在内存中是一个独立的对象，而不是对常量池中字符串的引用。

当你使用 new String("a") 创建一个字符串时，Java在堆中创建一个新的 String 对象。这一对象的内部表示会直接包含字符串的内容，比如字符数组（char[]）等。堆中的String对象是直接存储其实际内容，而不是存储一个指向常量池中字符串对象的地址

具体解释如下：

- 当执行 `new String("b")` 时，`"b"` 字符串字面量会先在常量池中查找，若不存在，则会在常量池中创建。
- 然后，`new String("b")` 会在堆中创建一个新的 `String` 对象，并将常量池中字符串的字符内容（字符数组）**复制**到堆中的对象中。

**因此，堆中的 `String` 对象拥有自己独立的字符内容（虽然内容相同），但它并不直接引用常量池中的 `String` 对象或字符数组。**

也就是说，堆中的 `String` 对象内容和常量池中相同，但两者是独立的。

![6-1](6-1.png)

```java
String s1 = "hello";
String s2 = "world";
String s3 = "helloworld";
System.out.println(s3 == s1 + s2); //false
System.out.println(s3.equals((s1 + s2)));//true
System.out.println(s3 == "hello" + "world");//true
System.out.println(s3.equals("hello" + "world"));//true

```
![6-2](6-2.png)
![6-3](6-3.png)
> 注意对象和对象变量的区别和牢记"按值传递"

### 字符串常量池具体位置

在Java的不同版本中，字符串常量池的位置确实有所变化。具体如下：

1. **Java 6及之前**：字符串常量池位于**方法区**，并且是全局的。每次创建字符串字面量，都会检查常量池中是否已有相同的字符串，如果有，则返回已有的字符串引用；否则，将新字符串添加到常量池中。

2. **Java 7**：字符串常量池被移到了**堆内存**中。这是因为字符串常量池在方法区占用大量内存，将其移至堆可以有效减少方法区内存的使用。此改动使得字符串常量池在堆中与其他对象共存，改善了内存管理效率。

3. **Java 8及之后**：字符串常量池依然位于**堆内存**中。Java 8还移除了永久代（PermGen），将其内容划分至元空间（Metaspace），但这不影响字符串常量池的位置。

总结来说，自Java 7起，字符串常量池一直位于堆内存中，而在Java 6及之前则位于方法区。

![java8以后字符串常量池的位置实验](https://blog.csdn.net/qq_41813208/article/details/110849158)


### 底层结构
在Java的不同版本中，`String`类的底层数据结构确实有所不同：

1. **Java 6及之前**：`String`类内部使用**`char[]`字符数组**来存储字符串内容，每个字符占用2个字节。字符串内容存储在`char`数组中，同时`String`对象是不可变的（immutable），所以一旦创建，`String`对象中的字符内容就无法修改。

2. **Java 7到Java 8**：`String`类依然使用`char[]`字符数组作为底层存储结构。不过，Java 7引入了**字符串去重机制**，允许多个相同的字符串字面量共享同一个`char[]`数组。

3. **Java 9及之后**：Java 9开始对`String`类做了优化，改用**`byte[]`数组**来存储字符串内容，同时引入了一个名为`coder`的字段，用于指示字符串的编码方式。这个字段只有两个值：
   - `0`表示Latin-1（ISO-8859-1）编码，每个字符占用1个字节；
   - `1`表示UTF-16编码，每个字符占用2个字节。

   这样，如果字符串中只有Latin-1字符（即单字节字符），`String`可以通过`byte[]`来节省内存。这种设计被称为**Compact Strings**，可以显著减少内存占用。

因此，Java 9及之后的`String`类的底层实现结构由`char[]`改为`byte[]`，通过`coder`字段实现对字符编码的判断，以更高效地管理字符串存储。


在Java中，`Map`接口的`keySet()`方法返回的是一个`Set`视图，代表了映射中所有键的集合。这个视图是“动态”的，意味着它是对原始映射的一个视图，因此任何在`keySet`上所做的修改都会影响原始映射，反之亦然，每次返回的都是同一个对象，避免了创建不必要的对象。具体来说：

1. **重用视图**：`keySet()`返回的`Set`视图是直接引用原始映射中的键，因此对该`Set`的修改（如添加或删除键）会反映在原始的`Map`中。举个例子：

   ```java
   Map<String, Integer> map = new HashMap<>();
   map.put("one", 1);
   map.put("two", 2);

   Set<String> keySet = map.keySet();
   keySet.remove("one");  // 从keySet中移除"one"

   System.out.println(map); // 输出: {two=2}
   ```

2. **一致性**：由于`keySet()`视图与原始`Map`保持一致，任何对`Map`的修改（如插入新键）也会反映在`keySet`中。例如，如果你在`Map`中添加了一个新键，`keySet`的内容会自动更新。

3. **不支持的操作**：虽然`keySet()`返回的`Set`支持大部分常见操作，但它并不支持`add`操作，因为你不能直接向映射中添加一个新的键。你必须通过`put`方法来添加键值对。

总结来说，`Map`接口的`keySet()`方法返回的`Set`视图是重用的，与原始映射紧密关联，确保了操作的一致性。

为了避免创建不必要的对象，还要尽量避免使用装箱基本类型，尽量使用基本类型，当心无意识的自动装箱


### 第七条：消除过期对象的引用

利用java实现一个简单的栈，底层是数组结构，在需要增大时扩充容量。如果栈先是增长（实现中是`array[size++]=e`），然后再收缩，那么从栈中pop出来的对象（实现中是`return array[--size]`）将不会被当做垃圾回收，因为栈内部还维护着这些对象的过期引用，即永远不会再被解除的引用。非活动部分即下标大于size的那些元素都是过期对象，但不会自动进行垃圾回收，需要在pop前手动设置为null
```java
public Object pop(){
    if (size=0) throws new Exception();
    Object result=array[--size];
    array[size]=null;   //手动清除
    return result;
}

```
**一般来说只要类是自己管理内存，就应该警惕内存泄漏问题**。当然也不需要过分小心，清空对象引用应该是一种例外，而不是规范行为，只有当类自己处理存储池（array）中的元素才需要注意。

在支持垃圾回收（GC）的语言（如Java、Python等）中，内存泄漏通常不会像在手动管理内存的语言（如C/C++）中那样明显。然而，即便有垃圾回收机制，仍然可能出现隐蔽的内存泄漏，这种情况通常由**无意识的对象保持（unintentional object retention）**导致。无意识的对象保持指的是某些对象不再需要使用，但由于程序不小心保留了对这些对象的引用，垃圾回收器无法回收它们，从而导致内存占用不断增加。

以下是无意识对象保持导致隐蔽内存泄漏的常见场景及其原因：

### 1. 静态集合或全局集合的使用
在很多应用程序中，会使用全局静态集合（如`List`、`Map`）来保存数据。由于静态集合的生命周期与程序相同，只要有元素引用存在，垃圾回收器就无法释放其中的对象。如果将不再需要的对象放入静态集合并且忘记清除它们，便会导致这些对象无法被回收。

**示例**：
```java
public class MemoryLeakExample {
    private static final List<Object> staticList = new ArrayList<>();

    public static void addItem(Object item) {
        staticList.add(item); // 添加对象到静态集合
    }
}
```
在这个例子中，任何添加到`staticList`中的对象都会在应用程序的整个生命周期内被保留，除非手动清理。

### 2. 缓存导致的内存泄漏
缓存机制经常会导致无意识的对象保持。为了提高性能，程序通常会缓存一些数据对象，避免重复创建。但如果没有使用合适的缓存策略（如定期清理或限制大小的缓存），这些缓存数据会在不需要时仍然被保留，无法被垃圾回收器回收。

**解决方案**：可以使用`WeakReference`或类似的弱引用机制，允许垃圾回收器在内存紧张时清除缓存数据。

### 3. 监听器和回调未正确移除
当一个对象注册了事件监听器或回调（callback）时，这些监听器会保存对该对象的引用。如果对象在事件源中未正确移除，GC将无法回收它们，导致内存泄漏。

**示例**：
```java
public class EventSource {
    private final List<EventListener> listeners = new ArrayList<>();

    public void addListener(EventListener listener) {
        listeners.add(listener); // 添加监听器
    }
}
```
在这个例子中，`EventListener`实例会保持对注册对象的引用，必须在不再需要时手动移除监听器。

### 4. 自定义类加载器导致的内存泄漏
在一些Java应用服务器中，会加载并卸载多种不同的应用，而每种应用使用自己的类加载器。类加载器和类之间有强引用关系，如果类加载器没有正确卸载，可能会导致整个应用的对象无法被回收。

### 5. 内部类或匿名类导致的内存泄漏
在使用内部类或匿名类时，内部类会自动保存外部类的引用。如果不小心创建了生命周期较长的内部类对象，那么它们会保持对外部类的引用，导致外部类对象也无法被回收。

**示例**：
```java
public class OuterClass {
    private class InnerClass {
        // 内部类对外部类的引用
    }
}
```
在这个例子中，`InnerClass`的实例会持有`OuterClass`的引用。如果`InnerClass`的实例在其他地方被持久化保存了，它会间接导致`OuterClass`无法被回收。

### 6. ThreadLocal变量导致的内存泄漏
`ThreadLocal`变量为每个线程提供独立的变量副本。然而，线程池中的线程生命周期较长，若`ThreadLocal`变量未及时清理，就会导致其保留的对象无法被回收。



### 解决这些问题的建议
- **使用弱引用**：对于缓存对象、监听器和回调等场景，可以使用`WeakReference`或`WeakHashMap`等弱引用机制。
- **定期清理集合**：对长生命周期集合的数据进行定期清理。
- **谨慎使用静态变量和集合**：确保静态变量和集合中的对象确实需要被长时间引用。
- **移除不再需要的监听器**：在对象销毁或不再需要监听器时，及时移除注册的监听器或回调。
- **尽量避免ThreadLocal在长时间运行的线程中**：对于线程池的使用，注意及时清理`ThreadLocal`变量。

通过这些措施，可以尽量避免无意识的对象保持导致的内存泄漏问题，让垃圾回收器更有效地回收不再需要的内存。

`java.lang.ref`包中的引用类型（特别是`WeakReference`和`SoftReference`）可以帮助管理缓存数据，避免无意识的对象保持，进而减少内存泄漏的风险。

### 1. `WeakReference` - 弱引用
弱引用是为了让GC更积极地回收对象的引用类型。只要某个对象只被弱引用持有，GC会优先回收它。因此，`WeakReference`非常适合缓存场景，尤其是那些可以随时重新生成的数据。例如，如果一个缓存对象仅被弱引用持有，那么当内存不足时，GC可以回收这些弱引用的对象，释放内存。

#### 使用示例
```java
import java.lang.ref.WeakReference;
import java.util.Map;
import java.util.WeakHashMap;

public class CacheExample {
    private final Map<String, WeakReference<Object>> cache = new WeakHashMap<>();

    public void put(String key, Object value) {
        cache.put(key, new WeakReference<>(value));
    }

    public Object get(String key) {
        WeakReference<Object> ref = cache.get(key);
        return ref != null ? ref.get() : null;
    }
}
```

在这个例子中，我们使用`WeakHashMap`作为缓存容器。当内存紧张时，GC会自动回收缓存中不再强引用的对象。

### 2. `SoftReference` - 软引用
`SoftReference`是另一种引用类型，适合用来缓存可能需要的、但可以在内存不足时释放的对象。软引用比弱引用更“坚强”，GC只会在内存不足时回收这些对象。通常情况下，可以用软引用来缓存占用内存较大的对象，例如图像或大型数据。

#### 使用示例
```java
import java.lang.ref.SoftReference;
import java.util.HashMap;
import java.util.Map;

public class ImageCache {
    private final Map<String, SoftReference<byte[]>> imageCache = new HashMap<>();

    public void put(String key, byte[] imageData) {
        imageCache.put(key, new SoftReference<>(imageData));
    }

    public byte[] get(String key) {
        SoftReference<byte[]> ref = imageCache.get(key);
        return (ref != null) ? ref.get() : null;
    }
}
```

在这个例子中，当内存不足时，GC会自动回收缓存中使用`SoftReference`保存的图像数据。

### 3. `PhantomReference` - 虚引用
`PhantomReference`主要用于跟踪对象的最终化状态。虽然通常不会用在缓存中，但在需要精确控制资源释放的场景下，它可以用于执行一些清理操作，确保对象内存被彻底释放。

### 小结
使用`java.lang.ref`包中的引用类型，可以让缓存机制更智能，在内存紧张时自动清理不再使用的对象。这种方法不仅能优化内存使用，还能降低内存泄漏的风险，是Java缓存管理中的一项重要工具。

## 第八条 避免使用终结方法和清除方法
在 Java 中，终结方法（Finalizer）和清除方法（Cleaner）都是用于在对象被垃圾回收之前执行一些**资源释放操作**的机制，但它们的使用方式和效率有所不同。注意是资源的管理，例如文件描述符等

### 1. 终结方法（Finalizer）

终结方法是 Java 中用于清理对象的旧机制，它通过覆盖 `Object` 类的 `finalize()` 方法来实现。当垃圾回收器发现某个对象没有引用时，会调用这个对象的 `finalize()` 方法，以便在对象被回收之前完成清理工作。

**特点**：
- `finalize()` 只会在垃圾回收之前调用一次，之后该对象的 `finalize()` 方法不会再被调用。
- 因为垃圾回收的时间不确定，所以 `finalize()` 的执行时间也是不确定的，可能导致资源得不到及时释放。
- Java 9 开始，`finalize()` 方法被标记为过时（deprecated），不推荐使用。

**示例**：
```java
@Override
protected void finalize() throws Throwable {
    try {
        // 清理资源
    } finally {
        super.finalize();
    }
}
```

**缺点**：
- 效率低，可能延迟垃圾回收，甚至导致内存泄漏。
- 终结方法在使用时没有可靠的执行顺序。
- 在使用错误时可能导致应用程序崩溃等问题。

### 2. 清除方法（Cleaner）

`Cleaner` 是从 Java 9 引入的一种替代 `finalize()` 的机制，用于更加高效和灵活的清理工作。`Cleaner` 通过注册一个清理任务（清理回调）来完成对象的清理工作，它不会影响垃圾回收的效率。

**特点**：
- `Cleaner` 是由 `java.lang.ref.Cleaner` 类实现的，它允许注册一个清理任务，在对象没有引用时自动执行。
- `Cleaner` 是非阻塞的，不会拖慢垃圾回收的速度。
- 相比于 `finalize()`，`Cleaner` 更加安全、高效，且可以控制清理任务的执行。

**示例**：
```java
import java.lang.ref.Cleaner;

public class ResourceHolder {
    private static final Cleaner cleaner = Cleaner.create();
    private final Cleaner.Cleanable cleanable;

    public ResourceHolder() {
        this.cleanable = cleaner.register(this, () -> {
            // 资源清理代码
            System.out.println("Resource cleaned up.");
        });
    }

    // 其他方法
}
```

**优点**：
- 更加高效，不会影响垃圾回收性能。
- 清理过程更加灵活，可以独立定义清理任务。
- 避免了 `finalize()` 的性能开销和不确定性。

### 总结

1. **终结方法** (`finalize()`): 旧方法，效率低，Java 9 开始被弃用。
2. **清除方法** (`Cleaner`): 新方法，推荐使用，性能更高，资源清理更可靠。

在实际编程中，建议尽量避免使用 `finalize()`，转而使用 `Cleaner` 或者手动清理资源。实际应该尽量少得使用终结方法和清除方法，但是它们也有好处：
- 当资源的所有者忘记调用close方法时，它们可以充当安全网，迟一点释放资源总比永不释放资源好
- 对象的本地对等体不是一个普通对象，所以垃圾回收期不会知道他，如果本地对等体拥有资源，并且性能可以被接受的话就应该使用终结方法或清除方法

> 在 Java 中，`Cleaner` 提供了一种比 `finalize` 更高效和可靠的方式来执行对象的清理操作。`Cleaner` 通过注册清理任务（也称清理回调）来帮助我们在对象不再被引用时进行清理。下面是 `Cleaner` 的基本使用方法和要点。
> 
> ### `Cleaner` 的使用步骤
> 
> 1. **创建 `Cleaner` 实例**：使用 `Cleaner.create()` 创建一个 `Cleaner` 对象。
> 2. **注册清理任务**：在构造函数或初始化方法中，将对象注册到 `Cleaner` 中，同时定义一个清理任务。这个任务将在对象被垃圾回收前执行。
> 3. **实现清理任务**：清理任务通常是一个实现 `Runnable` 接口的实例，包含资源释放的逻辑。
> 
> ### 示例：使用 `Cleaner` 来管理资源
> 
> 假设我们有一个 `ResourceHolder` 类来管理某种资源（如文件句柄或数据库连接），我们希望在对象被回收时自动释放资源。
> 
> ```java
> import java.lang.ref.Cleaner;
> 
> public class ResourceHolder {
> 
>     // 创建一个 Cleaner 实例（可以在多个对象间共享）
>     private static final Cleaner cleaner = Cleaner.create();
> 
>     // 内部类，用于实现清理任务
>     private static class State implements Runnable {
>         private boolean resourceInUse; // 资源的状态
> 
>         State() {
>             this.resourceInUse = true;
>             System.out.println("Resource acquired.");
>         }
> 
>         // 定义清理任务，用于在垃圾回收时释放资源
>         @Override
>         public void run() {
>             if (resourceInUse) {
>                 resourceInUse = false;
>                 System.out.println("Resource released.");
>             }
>         }
>     }
> 
>     // 实例字段，表示对象的状态
>     private final State state;
>     // Cleanable 对象，用于管理清理任务
>     private final Cleaner.Cleanable cleanable;
> 
>     public ResourceHolder() {
>         this.state = new State();
>         // 注册清理任务，并在对象的 state 不再被引用时执行 state 的 run 方法
>         this.cleanable = cleaner.register(this, state);
>     }
> 
>     // 其他业务逻辑
> }
> ```
> 
> ### 代码说明
> 
> - **`Cleaner cleaner = Cleaner.create();`**：创建一个 `Cleaner` 实例，可以在多个对象之间共享。
> - **`State` 类**：这是一个实现 `Runnable` 接口的内部类，定义了资源释放的逻辑。
> - **`cleaner.register(this, state);`**：将 `ResourceHolder` 对象注册到 `Cleaner` 中，并将 `state` 设置为清理任务。这样，垃圾回收器发现 `ResourceHolder` 不再被引用时，将会调用 `state.run()` 来释放资源。
> 
> ### 使用 `ResourceHolder` 类的示例
> 
> ```java
> public class Main {
>     public static void main(String[] args) {
>         ResourceHolder resource = new ResourceHolder();
>         
>         // 资源使用代码
>         // ...
> 
>         // 通过显式地将对象设置为 null，可以提示垃圾回收器回收
>         resource = null;
> 
>         // 主动调用垃圾回收器（仅作为示例，垃圾回收的时机是不确定的）
>         System.gc();
> 
>         // 等待清理任务完成（仅用于演示）
>         try {
>             Thread.sleep(1000); // 等待一会儿以确保清理任务输出
>         } catch (InterruptedException e) {
>             e.printStackTrace();
>         }
>     }
> }
> ```
> 
> ### 注意事项
> 
> 1. **不能替代显式资源管理**：`Cleaner` 虽然提供了自动清理功能，但仍不如 `try-with-resources` 或显式调用 `close()` 方法可靠。
> 2. **性能和资源消耗**：`Cleaner` 的清理任务会在后台线程中执行，这在性能敏感的场景中可能会有额外开销。
> 3. **非实时清理**：清理任务的执行时间依赖于垃圾回收，因此，`Cleaner` 不适合用于实时性要求高的资源释放场景。
> 
> ### 总结
> 
> `Cleaner` 是 `finalize` 的改进，但仍然不如显式资源管理可靠。通常建议仅在对象的生命周期复杂且不易管理的情况下使用 `Cleaner`。 
> 在 Java 中，`Cleaner.register` 的方法签名如下：
> 
> ```java
> public Cleanable register(Object obj, Runnable action)
> ```
> 
> 该方法用于注册一个清理任务，当指定的对象 `obj` 被垃圾回收时，将自动执行该清理任务 `action`。
> 
> ### 参数说明
> 
> 1. **`obj`**（`Object` 类型）：
>    - `register` 方法中的第一个参数 `obj` 是需要进行清理的对象，即当这个对象变得不可达时，清理任务 `action` 会被触发。
>    - 该对象通常是含有需要清理的资源（如文件、数据库连接等）的类实例。`Cleaner` 会监控这个对象的状态，一旦它不再被引用且符合垃圾回收条件，就会触发 `action`。
> 
> 2. **`action`**（`Runnable` 类型）：
>    - `action` 是一个实现了 `Runnable` 接口的实例，用于定义当 `obj` 不再被引用且垃圾回收时要执行的清理任务。
>    - 清理任务通常用于释放资源，比如关闭文件、断开数据库连接等。
>    - `Runnable` 的 `run()` 方法中包含具体的清理逻辑。这个清理任务会在 `obj` 被垃圾回收时自动调用。
> 
> ### 返回值
> 
> - `register` 方法返回一个 `Cleaner.Cleanable` 对象。通过调用 `Cleanable.clean()` 方法，用户可以手动执行清理任务，而不必等待垃圾回收器触发它。这在需要更及时地释放资源时非常有用。例如在实现了AutoCloseable的类中，将close方法定义为调用Cleanable.clean()


在 Java 中，确实建议尽量避免使用终结方法 (`finalize`) 和清除方法 (`Cleaner`)，这是因为它们在性能、可靠性、和可控性方面都存在一些限制和风险，尤其是在特定场景中。主要原因如下：

### 1. **终结方法的性能和不确定性问题**

- **终结方法的调用时间不确定**：终结方法依赖垃圾回收（GC）来触发，但垃圾回收的时间无法预测，这意味着终结方法的执行时间也是不确定的。

- **效率低下**：终结方法的实现通常会影响垃圾回收的性能，因为垃圾回收器需要多做一些检查和管理工作。启用终结方法的对象会被放入一个队列中等待回收，这可能会导致对象在内存中停留更长时间，延迟内存释放，从而占用资源。

- **多线程安全性**：终结方法是在垃圾回收器的线程中调用的，处理不当会引发线程安全问题，尤其是当清理的资源与其他线程共享时。

- **不保证执行**：终结方法线程的优先级很低，有可能使得对象进入队列的速度超过对象的终结速度，这时就无法执行终结任务。当一个程序终止的时候，某些已经无法访问的对象上的终结方法完全有可能根本没有被执行。如果异常发生在终结方法中，不会使得线程终止，连警告都不会打出来

### 2. **清除方法的改进与局限**

- **尽管 `Cleaner` 提高了效率**，它也面临类似的问题，例如，它的执行仍然依赖于垃圾回收，尽管比 `finalize` 效率高，但清理的时间和顺序依然不完全可控。这样，对于资源敏感型应用，依赖 `Cleaner` 进行清理仍然存在风险。

- **在高性能场景中代价高**：`Cleaner` 需要额外的线程和资源管理，可能对系统资源造成额外负担。这在频繁创建、销毁对象的环境中会影响性能。

### 3. **推荐的替代方案**

- **使用 `try-with-resources` 或显式的 `close()` 方法**：Java 7 引入的 `try-with-resources` 结构提供了一种更加可靠的资源管理方式。实现了 `AutoCloseable` 或 `Closeable` 接口的对象可以在 `try` 代码块结束时自动调用 `close()` 方法，以确保资源被及时释放，无需依赖垃圾回收。

- **显式地管理资源**：对于一些重要资源，如文件、数据库连接和网络连接，最好由开发者在合适的地方显式释放资源。通过显式地调用关闭方法，可以明确清理的时间和顺序。

### 4. **总结：终结方法和清除方法的局限性**

终结方法和清除方法在自动清理方面确实提供了一定的便利，但由于其执行时间和顺序的不确定性，以及潜在的性能影响，它们并不是理想的资源管理工具。更好的方法是使用 `try-with-resources` 或显式管理资源，这样可以保证资源的及时释放、提高性能，并避免潜在的内存泄漏和线程安全问题。

> `AutoCloseable` 是 Java 1.7 引入的接口，用于实现资源自动关闭，通常在涉及资源的代码中使用，比如文件、数据库连接、网络资源等。实现了 `AutoCloseable` 的类可以在 `try-with-resources` 语句中自动关闭资源，而不需要显式地在 `finally` 块中调用 `close()` 方法。
> 
> ### 基本用法
> 
> 以下是 `AutoCloseable` 的简单示例，假设我们有一个类 `MyResource` 实现了 `AutoCloseable`：
> 
> ```java
> class MyResource implements AutoCloseable {
>     public void doSomething() {
>         System.out.println("Doing something...");
>     }
>     
>     @Override
>     public void close() throws Exception {
>         System.out.println("Resource closed.");
>     }
> }
> 
> public class Main {
>     public static void main(String[] args) {
>         try (MyResource resource = new MyResource()) {
>             resource.doSomething();
>         } catch (Exception e) {
>             e.printStackTrace();
>         }
>     }
> }
> ```
> 
> ### 解释
> 
> 1. `try-with-resources` 语句在 `try` 块执行完毕后会自动调用 `AutoCloseable` 资源的 `close()` 方法。
> 2. 当 `MyResource` 实现了 `AutoCloseable`，它可以在 `try` 语句中初始化，并在结束时自动关闭。
> 3. 如果有多个资源，可以在 `try` 后加多个资源，用分号分隔，所有资源都会在块结束后依次关闭。
> 
> ### 使用场景
> 
> `AutoCloseable` 常用于 `InputStream`、`OutputStream`、`Reader`、`Writer`、数据库连接等实现了 `AutoCloseable` 接口的类。

## 第九条：try-with-resources 优先于 try-finally

```java
import org.junit.jupiter.api.Test;

import java.io.*;

public class MyTest {

    @Test
    public void test() throws IOException {
        InputStream inputStream = new FileInputStream("/home/yangsen/in.txt");
        try{
            OutputStream outputStream = new FileOutputStream("/home/yangsen/out.txt");
            int a=1/0;
            try{
                System.out.println("do sth");
            }finally{
                System.out.println("out close");
                outputStream.close();
            }
        }finally {
            System.out.println("in close");
            inputStream.close();
        }
    }

}

```
运行上面的程序，会使得发生异常时只有inputStream关闭了，而完全跨过了outputStream的finally块，导致outputStream无法关闭，使用try-resources就可以避免

```java
可以使用 `try-with-resources` 来简化资源管理代码，从而自动关闭资源，避免手动关闭文件流。以下是改进后的代码：

```java
import org.junit.jupiter.api.Test;
import java.io.*;

public class MyTest {

    @Test
    public void test() throws IOException {
        try (InputStream inputStream = new FileInputStream("/home/yangsen/in.txt");
             OutputStream outputStream = new FileOutputStream("/home/yangsen/out.txt")) {
            
            int a = 1 / 0; // 模拟异常
            
            System.out.println("do sth");

        } catch (ArithmeticException e) {
            System.out.println("ArithmeticException caught: " + e.getMessage());
        } finally {
            System.out.println("Resources closed automatically.");
        }
    }
}
```

在改进后的代码中：

- `try-with-resources` 自动管理 `inputStream` 和 `outputStream`，不再需要在 `finally` 块中手动关闭它们。
- 当发生异常（如 `ArithmeticException`），资源仍然会自动关闭。
- 移除了嵌套 `try-finally`，代码结构更加简洁易读
```


# 第三章 对于所有对象都通用的方法

> 在 Java 中，`final` 关键字有以下三种主要用途：
> 
> 1. **修饰变量**：表示该变量只能被赋值一次。一旦赋值，值不能再被修改（对于对象引用，引用不能被改变，但对象的内容仍然可以改变）。
>    ```java
>    final int age = 25;
>    age = 30; // 这会导致编译错误，因为age已经是final的。
>    ```
> 
> 2. **修饰方法**：表示该方法不能在子类中被重写（override）。这在设计不可更改的逻辑时很有用。
>    ```java
>    class Parent {
>        public final void display() {
>            System.out.println("This is a final method.");
>        }
>    }
> 
>    class Child extends Parent {
>        // 这里不能重写display()方法，否则会导致编译错误
>    }
>    ```
> 
> 3. **修饰类**：表示该类不能被继承。如果你不希望某个类被扩展，可以将其声明为 `final`。
>    ```java
>    public final class Car {
>        // 类的内容
>    }
> 
>    class ElectricCar extends Car {
>        // 这会导致编译错误，因为Car类是final的，不能被继承。
>    }
>    ```
> 
> 总结来说，`final` 关键字用于确保值、方法或类的不可更改性，从而提供一定程度的安全性和设计控制。




## 第十条：覆盖 equals 时清遵守通用约定

类有自己独有的"逻辑相等"概念，而不是对象等同，例如表示值的类，如Integer，而不是想了解是否是同一个对象时，需要覆盖 equals
```java
public boolean equals(Object obj) {
    return (this == obj);
}
```
覆盖时需要注意，equals 实现了 "等价关系"，满足：
- 自反性
- 对称性，在两个相似但不同的类进行互操作时要注意，x.equals(y)时，y.equals(x)必须返回一样的值
- 传递性，在遇到难以同时符合 equals 约定的继承类设计时，可以试试 复合优先于继承 的原则。因为 instanceof 能将子类判定为父类，当父类对象和子类对象调用equals方法的时候很可能会出错。
> 在 Java 中，`java.sql.Timestamp` 类的 `equals()` 方法确实存在违反对称性的情况。具体来说，`java.sql.Timestamp` 是 `java.util.Date` 的子类，但它们的 `equals()` 方法的行为有所不同。
> 
> ### 违反对称性的原因
> 
> - `java.util.Date` 的 `equals()` 方法只比较毫秒级的时间戳，不考虑纳秒部分。
> - 而 `java.sql.Timestamp` 的 `equals()` 方法除了比较毫秒级的时间戳外，还会比较纳秒级的时间戳。
> 
> 因此，出现了以下违反对称性的情况：
> 
> ```java
> java.util.Date date = new java.util.Date();
> java.sql.Timestamp timestamp = new java.sql.Timestamp(date.getTime());
> 
> // timestamp.equals(date) 返回 true
> // date.equals(timestamp) 返回 false
> ```
> 
> 在这个例子中：
> - `timestamp.equals(date)` 会返回 `true`，因为 `Timestamp` 的 `equals()` 方法会将 `date` 转换为 `Timestamp` 进行比较，忽略纳秒部分。
> - 但 `date.equals(timestamp)` 会返回 `false`，因为 `Date` 的 `equals()` 方法只比较毫秒部分，而不考虑 `Timestamp` 的纳秒部分。
> 
> ### 解决方案
> 
> 为了避免这种违反对称性的情况，通常建议在比较 `Timestamp` 和 `Date` 时：
> - 使用 `getTime()` 方法比较它们的毫秒时间戳。
> - 或者使用 `java.time` 包下的类，如 `Instant`、`LocalDateTime` 等，它们更为现代化和一致，避免了类似的设计问题。
> 
> 例如，可以这样进行时间戳的比较：
> 
> ```java
> if (date.getTime() == timestamp.getTime()) {
>     // 处理逻辑
> }
> ``` 
> 
> 或者，使用 Java 8 及以上版本中的 `Instant`：
> 
> ```java
> Instant instant1 = date.toInstant();
> Instant instant2 = timestamp.toInstant();
> 
> if (instant1.equals(instant2)) {
>     // 处理逻辑
> }
> ```

- 一致性
> `java.net.URL` 类的 `equals()` 方法也存在一些不符合标准 `equals()` 约定的问题，具体来说是违反了一致性原则。这个问题主要源于 `URL` 类的 `equals()` 方法如何执行两个 `URL` 对象的比较。
> 
> ### 一致性问题的原因
> 
> 在 `java.net.URL` 中，`equals()` 方法不仅仅比较 URL 的字符串表示形式，还会解析和比较实际的网络资源。这个过程依赖于 DNS（域名系统）解析，具体来说，两个 `URL` 对象的 `equals()` 方法不仅会比较它们的协议、主机、端口和路径，还会尝试通过 DNS 解析它们的主机名，以判断它们是否指向同一个 IP 地址。
> 
> 由于 DNS 解析是一个外部依赖，这就导致了 `equals()` 方法的结果在不同的时间或网络环境下可能会发生变化。例如：
> - 如果在第一次比较时 DNS 服务器没有响应，而在第二次比较时 DNS 响应了，`equals()` 的结果就会不同。
> - DNS 配置或网络状况的改变可能导致两个 URL 本来是相等的，但在不同的时间被认为不等，或者反过来。
> 
> 这就违反了 `equals()` 的 **一致性** 原则，按照该原则，如果两个对象在某一时刻相等，那么它们在未来的任意时刻都应该保持相等（前提是没有修改对象的状态）。
> 
> ### 示例
> 
> ```java
> URL url1 = new URL("http://example.com");
> URL url2 = new URL("http://example.com");
> 
> System.out.println(url1.equals(url2)); // 结果可能根据 DNS 解析的不同而改变
> ```
> 
> 在上述例子中，即便两个 URL 字面上是相同的，`equals()` 的结果也可能由于网络状况或 DNS 配置而变化。
> 
> ### 解决方案
> 
> 为了避免这种违反一致性的问题，推荐使用 `java.net.URI` 类而不是 `URL`，因为 `URI` 的 `equals()` 方法只会比较 URL 的字符串表示形式，而不会进行 DNS 解析。因此它能保证 `equals()` 的一致性。
> 
> ```java
> URI uri1 = new URI("http://example.com");
> URI uri2 = new URI("http://example.com");
> 
> System.out.println(uri1.equals(uri2)); // 始终返回 true
> ```
> 
> ### 总结
> 
> - `java.net.URL` 的 `equals()` 方法依赖于 DNS 解析，可能导致在不同的时间或环境下比较结果不一致，从而违反一致性原则。
> - 解决这个问题的最佳方法是使用 `java.net.URI` 类进行 URL 比较，因为它的 `equals()` 方法不会依赖外部网络状态。


- 对于非null的x，x.equals(null)返回false，而不是异常。通常使用了 instanceof 就已经避免null的问题了，不需要再显示判断是否为null


实现正确 equals 的诀窍：
- 使用 == 判断是否引用了同一个对象
- instanceof 判断是否是正确类型
- 把参数强转成正确类型
- 对每个关键域进行判断，优先判断那些很可能不一样的域，这样加快速度：
    -  float类型调用 Float.compare
    -  double类型调用 Double.compare
    -  其他基本类型直接 ==
    -  对象引用调用 equals


一些告诫：
- 覆盖 equals 时一定要覆盖 hashcode
- 不要想着让 equals 覆盖范围太广，只需要判断基本的相等即可，不要想着兼容很多类，不然会出错
- equals 的参数是 Object，这样才能覆盖 override，而不是重载 overload

## 第十一条：覆盖 equals 时总要覆盖 hashCode

在 Java 中，`equals` 和 `hashCode` 方法之间存在密切的关系。它们的正确实现对于对象在集合类（如 `HashMap`、`HashSet` 等）中的正确使用非常重要。

### 1. `equals()` 的作用
`equals()` 用于比较两个对象是否“逻辑上相等”。默认情况下（即在 `Object` 类中），它是比较对象的引用是否相同（即是否是同一个对象）。但是，在自定义类中通常需要覆盖这个方法，以比较对象的内容。

### 2. `hashCode()` 的作用
`hashCode()` 方法返回对象的哈希码，哈希码是用于支持哈希表的数据结构的整数值。Java 集合类中的哈希表（如 `HashMap`、`HashSet`）会根据对象的哈希码来确定其存储位置。

### 3. `equals()` 与 `hashCode()` 的关系
为了确保对象能够在哈希表中正常工作，必须遵守以下约定：
- **如果两个对象通过 `equals()` 方法相等，那么它们的 `hashCode()` 返回值必须相同。如果`equals()`不相等，`hashCode()`不是必须不同，但如果不同的话，散列表的性能更高**
- **如果两个对象的 `hashCode()` 返回值相同，它们通过 `equals()` 方法不一定相等。`hashCode()`返回值都不同了，那么`equals()` 也肯定不同**

**简单来说，`hashCode()` 的作用是将对象快速分组，只有在 `hashCode()` 值相等的情况下，集合才会调用 `equals()` 方法进一步检查对象是否相等。** equals 的要求更高，hashCode 只是用来分组

> 在 Java 的 `Objects` 类中，`hash()` 方法用于生成对象的哈希码。这个方法的主要目的是简化和标准化对象的哈希码计算，尤其是在处理多个字段时。
> 
> ### `hash()` 方法的重载
> 
> `Objects` 类提供了多个重载版本的 `hash()` 方法，以支持不同数量的参数。以下是一些常见的重载方法：
> 
> 1. **单个参数：**
>    ```java
>    public static int hash(Object o);
>    ```
>    这个方法会返回给定对象的哈希码。如果对象为 `null`，则返回 `0`。
> 
> 2. **多个参数：**
>    ```java
>    public static int hash(Object... values);
>    ```
>    这个方法接受任意数量的对象，并计算它们的哈希码。它会返回这些对象的哈希码的合成值。如果输入的数组为 `null`，则返回 `0`。如果其中某个对象为 `null`，会为其返回 `0`。
> 
> ### 示例
> 
> 以下是使用 `Objects.hash()` 方法的一个简单示例：
> 
> ```java
> import java.util.Objects;
> 
> public class Example {
>     private String name;
>     private int age;
> 
>     public Example(String name, int age) {
>         this.name = name;
>         this.age = age;
>     }
> 
>     @Override
>     public int hashCode() {
>         return Objects.hash(name, age);
>     }
> 
>     // 其他方法
> }
> ```
> 
> 在这个例子中，`hashCode()` 方法通过调用 `Objects.hash(name, age)` 来计算哈希码。这样可以简化哈希码的实现，同时避免处理 `null` 的复杂性。
> 
> ### 小结
> 
> 使用 `Objects.hash()` 方法可以让哈希码的计算更简洁和安全，尤其是在需要考虑多个字段的情况下。


![接口层次](11-interface.png)
![类层次](11-class.png)

> 在 Java 中，以 `Abstract` 开头的类（例如 `AbstractList`, `AbstractSet`, `AbstractMap` 等）是抽象类（`abstract` classes），它们提供了一些通用的基本功能和框架，帮助开发者实现具体的集合类。这些 `Abstract` 类的主要作用是简化子类的实现，提供默认的实现或部分实现，减少重复的代码工作。
> 
> ### 作用和特点
> 
> 1. **基础实现和模板**：
>    - 这些类提供了一些集合操作的默认实现（例如添加、删除、迭代等），让子类可以继承并只需实现特定的操作逻辑，而不必从头开始定义所有方法。
>    - 例如，`AbstractList` 提供了 `size()`, `get()` 等基本方法，但没有实现像 `add()` 这样的可修改方法，具体的子类只需实现这部分逻辑即可。
> 
> 2. **减少代码重复**：
>    - 如果你直接实现某个接口（比如 `List`, `Set`, `Map` 等），需要实现接口中的所有方法。而通过继承 `Abstract` 类，你只需要关注那些需要自定义的部分。很多标准操作已经由这些抽象类默认提供了。
> 
> 3. **部分抽象，部分具体实现**：
>    - `Abstract` 类是抽象类，它们不能直接实例化。类中有些方法是抽象的（必须由子类实现），而有些方法是具体的（提供了默认实现）。
>    - 例如，在 `AbstractList` 中，`get(int index)` 是一个抽象方法，子类必须实现它；而 `iterator()` 则是具体方法，它可以基于 `get()` 方法提供一个默认的迭代器。
> 
> 4. **增强可扩展性**：
>    - 开发者可以通过继承 `Abstract` 类轻松创建自定义的集合类型。这些类充当了模板，确保了自定义集合符合 Java 集合框架的设计模式和约定。
> 
> ### 常见的 Abstract 类
> 
> 1. **AbstractCollection**：
>    - 这是所有集合类型（`Collection`）的基础类，提供了 `size()`, `iterator()` 等基础操作的默认实现。
> 
> 2. **AbstractList**：
>    - 实现了 `List` 接口的大部分功能，适合用来创建序列化的集合类型，尤其是基于索引访问的集合。
> 
> 3. **AbstractSet**：
>    - 实现了 `Set` 接口，提供了一些针对无序集合的默认实现，比如 `equals()`, `hashCode()`。
> 
> 4. **AbstractQueue**：
>    - 实现了 `Queue` 接口的大部分功能，适用于创建队列（FIFO）相关的集合类型。
> 
> 5. **AbstractMap**：
>    - 为 `Map` 接口提供基础框架，帮助开发者实现键值对集合。
> 
> ### 示例：AbstractList 的作用
> ```java
> public abstract class AbstractList<E> extends AbstractCollection<E> implements List<E> {
>     // 默认实现了 List 接口的部分方法
>     public boolean add(E e) {
>         add(size(), e); // 基于 size() 和 add(int, E) 实现
>         return true;
>     }
> 
>     // 抽象方法，子类必须实现
>     public abstract E get(int index);
>     
>     // 可选地，子类可以覆盖此方法
>     public E remove(int index) {
>         throw new UnsupportedOperationException();
>     }
> }
> ```
> 
> 在这个示例中，`AbstractList` 已经实现了 `List` 接口中的部分方法（如 `add()`），而将具体的 `get()` 和 `remove()` 等方法留给子类实现。这样，子类可以通过继承 `AbstractList`，快速实现一个具体的 `List` 类。
> 
> ### 总结
> 以 `Abstract` 开头的类在 Java 集合框架中扮演了一个模板和部分实现的角色，它们帮助开发者简化具体集合类的实现，并确保新的集合类遵循集合框架的设计原则。


在 Java 中，集合框架（Collection Framework）是用来存储和操作一组对象的。其主要包含两个基本接口：**Collection** 和 **Map**。它们有各自的常见实现层次。以下是这两个接口的描述及其常见实现类的层次结构：

### 1. **Collection 接口**
**Collection** 是集合框架的根接口，它表示一组元素（对象），集合中的元素可以进行迭代。常见的子接口包括 **List**、**Set** 和 **Queue**，而它们有各自不同的实现。

Collection接口继承了Iterator接口，对象方法有next和hasNext和remove，注意和Collection接口的remove区分，另外remove的返回值是void，和cpp中的方法也不一样

#### 1.1 **List 接口**
**List** 接口表示一个存取有序（与插入顺序一致而不是经过了排序）的集合，允许重复元素。常见的实现类有（下面都是直接实现类，而不是像TreeSet和TreeMap一样通过子接口再实现）：

![内部类](innerClass.jpg)


> `java.util.Arrays$ArrayList` 是 Java 中的一个内部类，表示 `java.util.Arrays` 类中的一个静态内部类。它由 `Arrays.asList(…)` 方法返回。
> 
> 在 Java 中，符号 `$` 用于表示内部类的层级结构。所以，`java.util.Arrays$ArrayList` 表示的是 `Arrays` 类的内部类 `ArrayList`，它是 `java.util.Arrays` 类中定义的一个特殊的 `List` 实现。
> 
> 与 `java.util.ArrayList` 不同，`java.util.Arrays$ArrayList` 的几个关键特性是：
> 
> 1. **固定大小**: `java.util.Arrays$ArrayList` 并不是普通的 `ArrayList`，它是基于一个固定大小的数组创建的。你不能添加或删除元素，尝试这样做会抛出 `UnsupportedOperationException`。
>    
> 2. **共享底层数组**: `java.util.Arrays$ArrayList` 直接包装传入的数组，数组和 `List` 对象共享相同的内存区域。这意味着修改 `List` 中的元素会影响原始数组，反之亦然。
> 
> 3. **性能优化**: 这个类是轻量级的包装类，主要用于在 `List` 接口下快速操作数组内容，不会创建新的数组副本，因此效率较高，但它的功能有限。
> 
> ### 例子：
> ```java
> String[] array = {"A", "B", "C"};
> List<String> list = Arrays.asList(array);
> 
> // 修改 List
> list.set(0, "X");
> System.out.println(Arrays.toString(array));  // 输出: [X, B, C]
> 
> // 修改原始数组
> array[1] = "Y";
> System.out.println(list);  // 输出: [X, Y, C]
> 
> // 尝试添加或删除元素
> list.add("D");  // 抛出 UnsupportedOperationException
> ```
> 
> 在这种情况下，`list` 的实际类型是 `java.util.Arrays$ArrayList`，它是一个封装了 `array` 的不可变大小的 `List`，并且和数组共享数据存储。



##### ArrayList

基于数组实现的动态数组，查询快（O(1)），插入和删除效率相对低（O(n)）。

ArrayList和Vector的底层物理结构都是数组，       
- ArrayList是新版的动态数组，线程不安全，效率高，Vector是旧版的动态数组，线程安全，效率低。         
- 动态数组的扩容机制不同，ArrayList默认扩容为原来的1.5倍，Vector默认扩容增加为原来的2倍。       
- 数组的初始化容量，如果在构建ArrayList与Vector的集合对象时，没有显式指定初始化容量，那>么Vector的内部数组的初始容量默认为10，而ArrayList在JDK 6.0 及之前的版本也是10，JDK8.0 之后的版本ArrayList初始化为长度为0的空数组，之后在添加第一个元素时，再创建长度为10的数组。原因：
- 用的时候，再创建数组，避免浪费。因为很多
方法的返回值是ArrayList类型，需要返回一个Arr
ayList的对象，例如：后期从数据库查询对象的方
法，返回值很多就是ArrayList。有可能你要查询>
的数据不存在，要么返回null，要么返回一个没有
元素的ArrayList对象。




##### LinkedList

基于双向链表实现，适合频繁插入和删除的场景（O(1)），但查询效率低（O(n)）。

##### Vector

古老的集合，类似于 ArrayList，但它是线程安全的（性能稍低）。

##### Stack

继承自 Vector，遵循“后进先出”（LIFO）的原则。

#### 1.2 **Set 接口**

**Set** 接口表示一个不允许重复元素的集合。常见的实现类有（常用的直接实现类只有HashSet）：

##### HashSet

基于哈希表实现的集合，元素无序且不重复（不重复指的是不equals，equals比hashCode强，元素可以是null），查询和插入性能优秀（O(1)）。必须要实现hashCode和equals


HashSet中添加元素的过程：
- 第1步：当向 HashSet 集合中存入一个元素时，HashSet 会调用该对象的 hashCode() 方法得到该对象的 hashCode值，然后根据 hashCode值，通过某个散列函数决定该对>象在 HashSet 底层数组中的存储位置。
- 第2步：如果要在数组中存储的位置上没有元素，则直接添加成功
- 第3步：如果要在数组中存储的位置上有元素，则继续比较：     
  - 如果两个元素的hashCode值不相等，则添加成功；     
  - 如果两个元素的hashCode()值相等，则会继续调用equals()方法：
    - 如果equals()方法结果为false，则添加成功。
    - 如果equals()方法结果为true，则添加失败。

> 第2步添加成功，元素会保存在底层数组中。
>
> 第3步两种添加成功的操作，由于该底层数组的位置已经有元素了，则会通过`链表`的方式继续链接，存储。
> 注意，当两个元素的equals返回true，但只要hashCode不同，它们仍然会添加成功，要避免这种情况

> 在 Java 中，当我们在类中重写 `hashCode()` 方法时，通常使用的是一种基于 **对象字段** 的散列算法。常见的做法是利用类中的字段，计算出一个整型的散列值，确保相等的对象有相同的 `hashCode()` 值，同时避免不同对象有相同的 `hashCode()` 值，以减少哈希冲突。
> 
> 最常见的算法通常包括以下步骤：
> 
> 1. 选择一个非零的常量（例如 `31`），作为乘法因子。
> 2. 根据类的每个字段逐步计算出一个哈希值。
> 3. 对于每个字段，将当前的哈希值乘以乘法因子（通常是31），然后加上该字段的哈希值。
>     - 如果字段是基本类型（如 `int`、`long`），可以直接使用字段值。
>     - 如果字段是对象类型（如 `String`、`Integer`），可以调用该字段的 `hashCode()` 方法。
>     - 如果字段是数组，可以遍历数组元素计算 `hashCode()`，或者使用 `Arrays.hashCode()`。
> 
> 以下是一个典型的 `hashCode()` 方法的重写示例：
> 
> ```java
> @Override
> public int hashCode() {
>     int result = 17; // 初始值，可以是任意非零常数
>     result = 31 * result + (field1 != null ? field1.hashCode() : 0); // field1 是一个对象
>     result = 31 * result + field2; // field2 是一个基本类型（如 int）
>     result = 31 * result + (field3 != null ? field3.hashCode() : 0); // field3 是另一个对象
>     return result;
> }
> ```
> 
> 在这个例子中，`31` 被用作乘法因子，因为它是一个奇数素数，选择素数可以减少哈希冲突。而 `17` 是一个初始值，通常是非零的常数。
> 使用31的原因：
> 首先，选择系数的时候要选择尽量大的系数。因为如果计算出来的hash地址越大，所谓的“冲突”就越少，查找起来效率也会提高。（减少冲突）
> 其次，31只占用5bits,相乘造成数据溢出的概率较小。
> 再次，31可以 由i*31== (i<<5)-1来表示,现在很多虚拟机里面都有做相关优化。（提高算法效率）
> 最后，31是一个素数，素数作用就是如果我用一个数字来乘以这个素数，那么最终出来的结果只能被素数本身和被乘数还有1来整除！(减少冲突)
> 在很多 IDE（如 IntelliJ IDEA）中，生成 `hashCode()` 方法时，使用的也是类似的策略：逐个字段计算哈希值，并通过乘以常数累加来合成最终的哈希值。


###### LinkedHashSet

HashSet 的子类，使用双向链表维护插入顺序，但实际还是hashtable存储的，插入性能略低，但是迭代访问性能更好

##### 1.2.1 **SortedMap接口**
###### 1.2.2 **NavigableMap接口**


###### TreeSet

实现的是子接口SortedMap的子接口NavigableMap，基于红黑树实现，集合中的元素按自然顺序排序，查询和插入时间复杂度为 O(log n)。

对于 TreeSet 集合而言，**它判断两个对象是否相等的唯一标准是**：两个对象通过 compareTo(Object obj) 或compare(Object o1,Object o2)方法比较返回值。返回值为0，则认为两个对象相等。

#### 1.3 **Queue 接口**

**Queue** 接口表示一个队列，通常用于按顺序处理元素。常见的实现类有：
##### PriorityQueue

基于堆实现，支持优先级排序的队列。

##### LinkedList

也实现了Queue接口支持队列的基本操作

### 2. **Map 接口**

**Map** 接口表示一个键值对（key-value）映射表，它不继承自 Collection，因为 Map 是基于键值映射而非单个元素的集合。常见的子接口和实现类有（常用的直接实现类只有HashMap）：

> 在Java的`Map`接口中，对`key`和`value`的类有一些基本要求：
> 
> ### 对于`key`类的要求：
> 1. **`equals()` 和 `hashCode()` 方法**：如果你使用的`Map`实现基于哈希表（例如`HashMap`、`LinkedHashMap`），`key`类必须正确实现`equals()`和`hashCode()`方法。相同的`key`对象必须返回相同的`hashCode`值，并且在比较时通过`equals()`方法被认为是相等的。
>    - `equals()`用于比较两个键是否相等。
>    - `hashCode()`用于确定键的哈希值，决定它在哈希表中的存储位置。
>    
>    如果没有正确实现这些方法，可能会导致`Map`无法正确判断键是否相等，导致查找或存储操作出现异常行为。
> 
> 2. **不可变性（推荐）**：为了确保`Map`的行为一致，建议`key`对象是不可变的（例如`String`、包装类`Integer`等）。如果`key`的状态在存储后发生改变，可能导致`Map`无法正确查找键，因为哈希值或者比较规则可能发生变化。
> 
> 3. **实现`Comparable`接口（可选）**：如果你使用的`Map`是基于有序的（例如`TreeMap`），那么`key`类通常需要实现`Comparable`接口，或者你需要提供一个`Comparator`来定义键的排序规则。
> 
> ### 对于`value`类的要求：
> 1. **没有特殊要求**：对于`value`，Java的`Map`接口没有特别的要求。它可以是任何对象类型，可以为空（`null`），取决于具体的`Map`实现。
>    - `HashMap`、`TreeMap`允许`value`为`null`，但`Hashtable`不允许`value`为`null`。
> 
> 总结来说，`key`类在哈希表实现的`Map`中需要正确实现`equals()`和`hashCode()`方法，而对于`value`类没有特殊要求。

Map提供了元视图：
- Set keySet()
- Collection values()
- Set entrySet()

#### 2.1 **SortedMap接口**
继承自 `Map`，它保持键的自然顺序或按比较器指定的顺序。
#### 2.2 **NavigableMap接口**

继承自 `SortedMap`，提供了一些导航方法，允许对键进行更复杂的查找。

##### TreeMap

实现的是 NavigableMap接口，和TreeSet类似。基于红黑树实现，存储键的排序版本，支持 `SortedMap` 和 `NavigableMap` 的功能。

底层使用红黑树存储，判断相等的标准不是equals而是两个key通过compareTo()方法或者compare()方法返回0。

#### HashMap

直接实现Map接口，基于哈希表实现，允许空键和空值，查找速度快。

在Java中，`HashMap` 的底层主要使用 **数组 + 链表 + 红黑树** 来存储数据。

具体工作原理如下：

1. **数组**：`HashMap` 底层维护了一个数组（称为 **哈希桶**）。数组的每个位置存储的是一个链表或树的引用。

2. **链表**：当多个键的哈希值经过取模计算后落在同一个位置时，就会发生**哈希冲突**。在较早版本的 `HashMap` 中，这些冲突的键值对会以链表的形式存储在同一个桶里。每个链表节点包含了键值对和指向下一个节点的指针。

3. **红黑树**：Java7中使用的是类似HashSet的结构，没有用红黑树。从Java 8开始，为了优化查询性能，当某个桶中的链表长度超过阈值（默认为 8）时，链表会转换成**红黑树**。红黑树是一种自平衡的二叉查找树，能够将最差情况下的查找时间复杂度从 `O(n)` 降低到 `O(log n)`。

简要过程：
- 当你往 `HashMap` 中插入键值对时，`HashMap` 会对键的哈希值进行处理，找到对应的数组索引。
- 如果这个索引位置没有元素，直接插入；如果已经有元素，则采用链表或红黑树来解决冲突。
- 在查找时，根据键的哈希值定位到数组中的位置，再在链表或红黑树中进行进一步的匹配。

通过这种设计，`HashMap` 在一般情况下提供了接近 `O(1)` 的时间复杂度，用来快速插入和查找数据。


##### LinkedHashMap

继承自 `HashMap`，保持插入顺序或访问顺序。
使用了一对双向链表标记了添加元素的先后顺序



##### Hashtable

- Hashtable是Map接口的`古老实现类`，JDK1.0就提供了。不同于HashMap，Hashtable是线程安全的

- Hashtable实现原理和HashMap相同，功能相同。底层都使用哈希表结构（数组+单向链表），查询速度快。
- Hashtable判断两个key相等、两个value相等的标准，与HashMap一致。
- 与HashMap不同，Hashtable 不允许使用 null 作为 key 或 value。

面试题：Hashtable和HashMap的区别

```
HashMap:底层是一个哈希表（jdk7:数组+链表;jdk
8:数组+链表+红黑树）,是一个线程不安全的集合,
执行效率高
Hashtable:底层也是一个哈希表（数组+链表）,是
一个线程安全的集合,执行效率低

HashMap集合:可以存储null的键、null的值
Hashtable集合,不能存储null的键、null的值

Hashtable和Vector集合一样,在jdk1.2版本之后被
更先进的集合(HashMap,ArrayList)取代了。所以H
ashMap是Map的主要实现类，Hashtable是Map的古>
老实现类。

Hashtable的子类Properties（配置文件）依然活>
跃在历史舞台
Properties集合是一个唯一和IO流相结合的集合
```
###### Properties

Properties是Hashtable的子类，要求key和value都是字符串类型，存取数据的时候推荐使用setProperty和getProperty

在 `Properties` 类中推荐使用 `setProperty` 而不是 `put` 等 `Map` 接口中的通用方法，主要有以下几个原因：

1. **类型安全**：
   - `setProperty` 方法的参数类型是 `String`，而 `put` 方法的参数类型是 `Object`。`Properties` 类的设计初衷是用于存储字符串键值对，因此使用 `setProperty` 可以保证键和值都是 `String` 类型，从而避免类型转换的错误或运行时异常。
   - 如果使用 `put`，虽然键值可以是任意类型的 `Object`，但这有可能导致程序中的类型不一致问题，增加调试和维护的复杂性。

2. **兼容性**：
   - `Properties` 类是 `Hashtable` 的子类，而 `Hashtable` 实现了 `Map` 接口。由于 `Hashtable` 可以存储任意类型的键值对，因此 `Properties` 类继承了 `put` 方法。然而，`Properties` 类本质上是为处理字符串配置而设计的，使用 `setProperty` 更符合它的设计意图。
   - 如果使用 `put` 方法，虽然可以存储非字符串类型的键值对，但在 `store()` 方法（将 `Properties` 对象写入输出流）等功能上可能会引发问题，因为这些方法假设键和值都是 `String` 类型。

3. **方法行为差异**：
   - `setProperty` 方法不仅会将键值对存储到内部的 `Hashtable` 中，还会同时将键值对保存在 `Properties` 专用的字符串键值对集合中。如果直接使用 `put`，该特性可能不会得到正确实现。

因此，尽管 `Properties` 类可以使用 `put` 方法来存取数据，但更推荐使用 `setProperty` 来确保类型安全，并且保持与类的设计初衷和功能行为一致。



> 在 Java 中，`Set` 和 `Queue` 是直接继承自 `Collection` 接口，而 `List` 是通过 `SequencedCollection` 间接继承于 `Collection`，这是从 Java 21 开始引入的新变化。
> 
> 以下是更详细的解释：
> 
> 1. **Set 和 Queue**:
>    - 这两个接口仍然直接继承自 `Collection`，即它们从 `Collection` 接口中继承了基本的集合操作方法（如 `add()`, `remove()`, `contains()` 等）。
> 
> 2. **List**:
>    - 从 Java 21 开始，`List` 接口不再直接继承自 `Collection`，而是通过新的 `SequencedCollection` 接口间接继承。`SequencedCollection` 是一个新的接口，专门用来处理有序的集合类型。
>    - `SequencedCollection` 继承了 `Collection`，并添加了一些顺序相关的方法，比如 `getFirst()`, `getLast()`, `reverseIterator()` 等。
>    - 因此，`List` 作为一个有序集合，依赖于 `SequencedCollection` 来提供顺序相关的操作，同时仍然保留了所有 `Collection` 的通用方法。
> 
> ### Java 21 的这种设计变化
> - 引入 `SequencedCollection` 的目的是为了更加清晰地区分有序和无序的集合。
> - 它不仅简化了有序集合的操作，还为开发者提供了更好的 API 设计，让顺序相关的集合操作更加直观和统一。
> 
> 总结来说，在 Java 21 之前，`List` 是直接继承自 `Collection`，而在 Java 21 中，它是通过 `SequencedCollection` 间接继承。


### List接口分析
#### List接口特点
- List集合所有的元素是以一种`线性方式`进行存储的，例如，存元素的顺序是11、22、33。那么集合中，元素的存储就是按照11、22、33的顺序完成的）。
- 它是一个`带有索引`的集合，通过索引就可以精确的操作集合中的元素（与数组的索引是一个道理）。
   - ArrayList：动态数组
   - LinkedList：双向链表
* 动态数组的扩容机制不同，ArrayList默认扩容为原来的1.5倍，Vector默认扩容增加为原来的2倍。
  * 用的时候，再创建数组，避免浪费。因为很多方法的返回值是ArrayList类型，需要返回一个ArrayList的对象，例如：后期从数据库查询对象的方法，返回值很多就是ArrayList。有可能你要查询的数据不存在，要么返回null，要么返回一个没有元素的ArrayList对象。
#### ArrayList部分源码分析
**JDK1.7.0_07中：**
```java
//属性
private transient Object[] elementData; //存储底层数组元素
private int size; //记录数组中存储的元素的个数

//构造器
public ArrayList() {
    this(10); //指定初始容量为10
}

public ArrayList(int initialCapacity) {
    super();
    //检查初始容量的合法性
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal Capacity: "+ initialCapacity);
    //数组初始化为长度为initialCapacity的数组
    this.elementData = new Object[initialCapacity]; 
}

//方法：add()相关方法
public boolean add(E e) {
    ensureCapacityInternal(size + 1);  //查看当前数组是否够多存一个元素
    elementData[size++] = e; //将元素e添加到elementData数组中
    return true;
}

private void ensureCapacityInternal(int minCapacity) {
    modCount++;
    // 如果if条件满足，则进行数组的扩容
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}

private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length; //当前数组容量
    int newCapacity = oldCapacity + (oldCapacity >> 1); //新数组容量是旧数组容量的1.5倍
    if (newCapacity - minCapacity < 0)  //判断旧数组的1.5倍是否够
        newCapacity = minCapacity;
    //判断旧数组的1.5倍是否超过最大数组限制
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    //复制一个新数组
    elementData = Arrays.copyOf(elementData, newCapacity);
}

//方法：remove()相关方法
public E remove(int index) {
    rangeCheck(index); //判断index是否在有效的范围内

    modCount++; //修改次数加1
    //取出[index]位置的元素，[index]位置的元素就是要被删除的元素，用于最后返回被删除的元素
    E oldValue = elementData(index); 

    int numMoved = size - index - 1; //确定要移动的次数
    //如果需要移动元素，就用System.arraycopy移动元素
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index, numMoved);
    //将elementData[size-1]位置置空，让GC回收空间，元素个数减少
    elementData[--size] = null; 

    return oldValue;
}

private void rangeCheck(int index) {
    if (index >= size) //index不合法的情况
        throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}

E elementData(int index) { //返回指定位置的元素
    return (E) elementData[index];
}

//方法：set()方法相关
public E set(int index, E element) {
    rangeCheck(index); //检验index是否合法
	
    //取出[index]位置的元素，[index]位置的元素就是要被替换的元素，用于最后返回被替换的元素
    E oldValue = elementData(index);
    //用element替换[index]位置的元素
    elementData[index] = element;
    return oldValue;
}

//方法：get()相关方法
public E get(int index) {
    rangeCheck(index); //检验index是否合法

    return elementData(index); //返回[index]位置的元素
}

//方法：indexOf()
public int indexOf(Object o) {
    //分为o是否为空两种情况
    if (o == null) {
        //从前往后找
        for (int i = 0; i < size; i++)
            if (elementData[i]==null)
                return i;
    } else {
        for (int i = 0; i < size; i++)
            if (o.equals(elementData[i]))
                return i;
    }
    return -1;
}

//方法：lastIndexOf()
public int lastIndexOf(Object o) {
    //分为o是否为空两种情况
    if (o == null) {
        //从后往前找
        for (int i = size-1; i >= 0; i--)
            if (elementData[i]==null)
                return i;
    } else {
        for (int i = size-1; i >= 0; i--)
            if (o.equals(elementData[i]))
                return i;
    }
    return -1;
}
```
**jdk1.8.0_271中：**
```java
//属性
transient Object[] elementData;
private int size;
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

//构造器
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;  //初始化为空数组
}

//方法:add()相关方法
public boolean add(E e) {
    //查看当前数组是否够多存一个元素
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    //存入新元素到[size]位置，然后size自增1
    elementData[size++] = e;
    return true;
}

private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

private static int calculateCapacity(Object[] elementData, int minCapacity) {
    //如果当前数组还是空数组
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        //那么minCapacity取DEFAULT_CAPACITY与minCapacity的最大值
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}

//查看是否需要扩容
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;  //修改次数加1

    //如果需要的最小容量比当前数组的长度大，即当前数组不够存，就扩容
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}

private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length; //当前数组容量
    int newCapacity = oldCapacity + (oldCapacity >> 1); //新数组容量是旧数组容量的1.5倍
    //看旧数组的1.5倍是否够
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    //看旧数组的1.5倍是否超过最大数组限制
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    //复制一个新数组
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```

#### Vector部分源码分析

```java
//属性
protected Object[] elementData;
protected int elementCount;

//构造器
public Vector() {
	this(10); //指定初始容量initialCapacity为10
}

public Vector(int initialCapacity) {
	this(initialCapacity, 0); //指定capacityIncrement增量为0
}

public Vector(int initialCapacity, int capacityIncrement) {
    super();
    //判断了形参初始容量initialCapacity的合法性
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal Capacity: "+ initialCapacity);
    //创建了一个Object[]类型的数组
    this.elementData = new Object[initialCapacity];
    //增量，默认是0，如果是0，后面就按照2倍增加，如果不是0，后面就按照你指定的增量进行增量
    this.capacityIncrement = capacityIncrement;
}

//方法：add()相关方法
//synchronized意味着线程安全的   
public synchronized boolean add(E e) {
    modCount++;
    //看是否需要扩容
    ensureCapacityHelper(elementCount + 1);
    //把新的元素存入[elementCount]，存入后，elementCount元素的个数增1
    elementData[elementCount++] = e;
    return true;
}

private void ensureCapacityHelper(int minCapacity) {
     //看是否超过了当前数组的容量
    if (minCapacity - elementData.length > 0)
        grow(minCapacity); //扩容
}

private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length; //获取目前数组的长度
    //如果capacityIncrement增量是0，新容量 = oldCapacity的2倍
    //如果capacityIncrement增量是不是0，新容量 = oldCapacity + capacityIncrement增量;
    int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                     capacityIncrement : oldCapacity);
    //如果按照上面计算的新容量还不够，就按照你指定的需要的最小容量来扩容minCapacity
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    //如果新容量超过了最大数组限制，那么单独处理
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    //把旧数组中的数据复制到新数组中，新数组的长度为newCapacity
    elementData = Arrays.copyOf(elementData, newCapacity);
}

//方法：remove()相关方法
public boolean remove(Object o) {
    return removeElement(o);
}
public synchronized boolean removeElement(Object obj) {
    modCount++;
    //查找obj在当前Vector中的下标
    int i = indexOf(obj);
    //如果i>=0，说明存在，删除[i]位置的元素
    if (i >= 0) {
        removeElementAt(i);
        return true;
    }
    return false;
}

//方法：indexOf()
public int indexOf(Object o) {
    return indexOf(o, 0);
}
public synchronized int indexOf(Object o, int index) {
    if (o == null) {//要查找的元素是null值
        for (int i = index ; i < elementCount ; i++)
            if (elementData[i]==null)//如果是null值，用==null判断
                return i;
    } else {//要查找的元素是非null值
        for (int i = index ; i < elementCount ; i++)
            if (o.equals(elementData[i]))//如果是非null值，用equals判断
                return i;
    }
    return -1;
}

//方法：removeElementAt()
public synchronized void removeElementAt(int index) {
    modCount++;
    //判断下标的合法性
    if (index >= elementCount) {
        throw new ArrayIndexOutOfBoundsException(index + " >= " +
                                                 elementCount);
    }
    else if (index < 0) {
        throw new ArrayIndexOutOfBoundsException(index);
    }

    //j是要移动的元素的个数
    int j = elementCount - index - 1;
    //如果需要移动元素，就调用System.arraycopy进行移动
    if (j > 0) {
        //把index+1位置以及后面的元素往前移动
        //index+1的位置的元素移动到index位置，依次类推
        //一共移动j个
        System.arraycopy(elementData, index + 1, elementData, index, j);
    }
    //元素的总个数减少
    elementCount--;
    //将elementData[elementCount]这个位置置空，用来添加新元素，位置的元素等着被GC回收
    elementData[elementCount] = null; /* to let gc do its work */
}
```

#### LinkedList部分源码分析

```java
//属性
transient Node<E> first; //记录第一个结点的位置
transient Node<E> last; //记录当前链表的尾元素
transient int size = 0; //记录最后一个结点的位置

//构造器
public LinkedList() {
}

//方法：add()相关方法
public boolean add(E e) {
    linkLast(e); //默认把新元素链接到链表尾部
    return true;
}

void linkLast(E e) {
    final Node<E> l = last; //用 l 记录原来的最后一个结点
    //创建新结点
    final Node<E> newNode = new Node<>(l, e, null);
    //现在的新结点是最后一个结点了
    last = newNode;
    //如果l==null，说明原来的链表是空的
    if (l == null)
        //那么新结点同时也是第一个结点
        first = newNode;
    else
        //否则把新结点链接到原来的最后一个结点的next中
        l.next = newNode;
    //元素个数增加
    size++;
    //修改次数增加
    modCount++;
}

//其中，Node类定义如下
private static class Node<E> {
    E item; //元素数据
    Node<E> next; //下一个结点
    Node<E> prev; //前一个结点

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
//方法：获取get()相关方法
public E get(int index) {
    checkElementIndex(index);
    return node(index).item;
} 

//方法：插入add()相关方法
public void add(int index, E element) {
    checkPositionIndex(index);//检查index范围

    if (index == size)//如果index==size，连接到当前链表的尾部
        linkLast(element);
    else
        linkBefore(element, node(index));
}

Node<E> node(int index) {
    // assert isElementIndex(index);
	/*
	index < (size >> 1)采用二分思想，先将index与长度size的一半比较，如果index<size/2，就只从位置0
	往后遍历到位置index处，而如果index>size/2，就只从位置size往前遍历到位置index处。这样可以减少一部
	分不必要的遍历。
	*/
    //如果index<size/2，就从前往后找目标结点
    if (index < (size >> 1)) {
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {//否则从后往前找目标结点
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}

//把新结点插入到[index]位置的结点succ前面
void linkBefore(E e, Node<E> succ) {//succ是[index]位置对应的结点
    // assert succ != null;
    final Node<E> pred = succ.prev; //[index]位置的前一个结点

    //新结点的prev是原来[index]位置的前一个结点
    //新结点的next是原来[index]位置的结点
    final Node<E> newNode = new Node<>(pred, e, succ);

    //[index]位置对应的结点的prev指向新结点
    succ.prev = newNode;

    //如果原来[index]位置对应的结点是第一个结点，那么现在新结点是第一个结点
    if (pred == null)
        first = newNode;
    else
        pred.next = newNode;//原来[index]位置的前一个结点的next指向新结点
    size++;
    modCount++;
}

//方法：remove()相关方法
public boolean remove(Object o) {
    //分o是否为空两种情况
    if (o == null) {
        //找到o对应的结点x
        for (Node<E> x = first; x != null; x = x.next) {
            if (x.item == null) {
                unlink(x);//删除x结点
                return true;
            }
        }
    } else {
        //找到o对应的结点x
        for (Node<E> x = first; x != null; x = x.next) {
            if (o.equals(x.item)) {
                unlink(x);//删除x结点
                return true;
            }
        }
    }
    return false;
}
E unlink(Node<E> x) {//x是要被删除的结点
    // assert x != null;
    final E element = x.item;//被删除结点的数据
    final Node<E> next = x.next;//被删除结点的下一个结点
    final Node<E> prev = x.prev;//被删除结点的上一个结点

    //如果被删除结点的前面没有结点，说明被删除结点是第一个结点
    if (prev == null) {
        //那么被删除结点的下一个结点变为第一个结点
        first = next;
    } else {//被删除结点不是第一个结点
        //被删除结点的上一个结点的next指向被删除结点的下一个结点
        prev.next = next;
        //断开被删除结点与上一个结点的链接
        x.prev = null;//使得GC回收
    }

    //如果被删除结点的后面没有结点，说明被删除结点是最后一个结点
    if (next == null) {
        //那么被删除结点的上一个结点变为最后一个结点
        last = prev;
    } else {//被删除结点不是最后一个结点
        //被删除结点的下一个结点的prev执行被删除结点的上一个结点
        next.prev = prev;
        //断开被删除结点与下一个结点的连接
        x.next = null;//使得GC回收
    }
    //把被删除结点的数据也置空，使得GC回收
    x.item = null;
    //元素个数减少
    size--;
    //修改次数增加
    modCount++;
    //返回被删除结点的数据
    return element;
}

public E remove(int index) { //index是要删除元素的索引位置
    checkElementIndex(index);
    return unlink(node(index));
}
```


![jdk7 ArrayList](jdk7ArrayList.jpg)
![jdk8 ArrayList](jdk8ArrayList.jpg)
![jdk8 Vector](jdk8Vector.jpg)



### Map接口分析


#### 哈希表的物理结构

HashMap和Hashtable底层都是哈希表（也称散列表），其中维护了一个长度为**2的幂次方**的Entry类型的数组table，数组的每一个索引位置被称为一个桶(bucket)，你添加的映射关系(key,value)最终都被封装为一个Map.Entry类型的对象，放到某个table[index]桶中。

使用数组的目的是查询和添加的效率高，可以根据索引直接定位到某个table[index]。


#### HashMap中数据添加过程

##### JDK7中过程分析
```java
// 在底层创建了长度为16的Entry[] table的数组
HashMap map = new HashMap(); 
```

```java
map.put(key1,value1);
/*
分析过程如下：

将(key1,value1)添加到当前hashmap的对象中。首先会调用key1所在类的hashCode()方法，计算key1的哈希值1，
此哈希值1再经过某种运算(hash())，得到哈希值2。此哈希值2再经过某种运算(indexFor())，确定在底层table数组中的索引位置i。
   （1）如果数组索引为i上的数据为空，则(key1,value1)直接添加成功   ------位置1
   （2）如果数组索引为i上的数据不为空，有(key2,value2)，则需要进一步判断：
       判断key1的哈希值2与key2的哈希值是否相同：
         （3） 如果哈希值不同，则(key1,value1)直接添加成功   ------位置2
              如果哈希值相同，则需要继续调用key1所在类的equals()方法，将key2放入equals()形参进行判断
                （4） equals方法返回false : 则(key1,value1)直接添加成功   ------位置3
                      equals方法返回true : 默认情况下，value1会覆盖value2。

位置1：直接将(key1,value1)以Entry对象的方式存放到table数组索引i的位置。
位置2、位置3：(key1,value1) 与现有的元素以链表的方式存储在table数组索引i的位置，新添加的元素指向旧添加的元素。

...
在不断的添加的情况下，满足如下条件的情况下，会进行扩容:
if ((size >= threshold) && (null != table[bucketIndex])) :
默认情况下，当要添加的元素个数超过12(即：数组的长度 * loadFactor得到的结果)时，就要考虑扩容。

补充：jdk7源码中定义的：
static class Entry<K,V> implements Map.Entry<K,V>
*/
```

```java
map.get(key1);
/*
① 计算key1的hash值，用这个方法hash(key1)

② 找index = table.length-1 & hash;

③ 如果table[index]不为空，那么就挨个比较哪个Entry的key与它相同，就返回它的value
*/

```

```java
map.remove(key1);
/*
① 计算key1的hash值，用这个方法hash(key1)

② 找index = table.length-1 & hash;

③ 如果table[index]不为空，那么就挨个比较哪个Entry的key与它相同，就删除它，把它前面的Entry的next的值修改为被删除Entry的next
*/
```



##### JDK8中过程分析

下面说明是JDK8相较于JDK7的不同之处：

```java
/*
①
使用HashMap()的构造器创建对象时，并没有在底层初始化长度为16的table数组。

②
jdk8中添加的key,value封装到了HashMap.Node类的对象中。而非jdk7中的HashMap.Entry。

③
jdk8中新增的元素所在的索引位置如果有其他元素。在经过一系列判断后，如果能添加，则是旧的元素指向新的元素。而非jdk7中的新的元素指向旧的元素。“七上八下”

④
jdk7时底层的数据结构是：数组+单向链表。 而jdk8时，底层的数据结构是：数组+单向链表+红黑树。
红黑树出现的时机：当某个索引位置i上的链表的长度达到8，且数组的长度超过64时，此索引位置上的元素要从单向链表改为红黑树。
如果索引i位置是红黑树的结构，当不断删除元素的情况下，当前索引i位置上的元素的个数低于6时，要从红黑树改为单向链表。

*/
```

#### HashMap源码剖析

##### JDK1.7.0_07中源码


###### **1、Entry**

key-value被封装为HashMap.Entry类型，而这个类型实现了Map.Entry接口。

```java
public class HashMap<K,V>{
    transient Entry<K,V>[] table;
    
    static class Entry<K,V> implements Map.Entry<K,V> {
        final K key;
        V value;
        Entry<K,V> next;
        int hash;

        /**
         * Creates new entry.
         */
        Entry(int h, K k, V v, Entry<K,V> n) {
            value = v;
            next = n;
            key = k;
            hash = h;
        }
        //略
    }
}
```

###### **2、属性**

```java
//table数组的默认初始化长度
static final int DEFAULT_INITIAL_CAPACITY = 16;
//哈希表
transient Entry<K,V>[] table;
//哈希表中key-value的个数
transient int size;
//临界值、阈值（扩容的临界值）
int threshold;
//加载因子
final float loadFactor;
//默认加载因子
static final float DEFAULT_LOAD_FACTOR = 0.75f;
```

###### **3、构造器**

```java
public HashMap() {
    //DEFAULT_INITIAL_CAPACITY：默认初始容量16
  	//DEFAULT_LOAD_FACTOR：默认加载因子0.75
    this(DEFAULT_INITIAL_CAPACITY, DEFAULT_LOAD_FACTOR);
}
```

```java
public HashMap(int initialCapacity, float loadFactor) {
    //校验initialCapacity合法性
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " + initialCapacity);
    //校验initialCapacity合法性 
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
    //校验loadFactor合法性
    if (loadFactor <= 0 || Float.isNaN(loadFactor))
        throw new IllegalArgumentException("Illegal load factor: " + loadFactor);

    //计算得到table数组的长度（保证capacity是2的整次幂）
    int capacity = 1;
    while (capacity < initialCapacity)
        capacity <<= 1;
	//加载因子，初始化为0.75
    this.loadFactor = loadFactor;
    // threshold 初始为默认容量
    threshold = (int)Math.min(capacity * loadFactor, MAXIMUM_CAPACITY + 1);
    //初始化table数组
    table = new Entry[capacity];
    useAltHashing = sun.misc.VM.isBooted() &&
                                       (capacity >= Holder.ALTERNATIVE_HASHING_THRESHOLD);
    init();
}
```

###### **4、put()方法**

```java
public V put(K key, V value) {
    //如果key是null，单独处理，存储到table[0]中，如果有另一个key为null，value覆盖
    if (key == null)
        return putForNullKey(value);
    //对key的hashCode进行干扰，算出一个hash值
    /*
      hashCode值        xxxxxxxxxx
      table.length-1    000001111
   
      hashCode值 xxxxxxxxxx  无符号右移几位和原来的hashCode值做^运算，使得hashCode高位二进制值参与计算，
                            也发挥作用，降低index冲突的概率。
    */
    int hash = hash(key);
    //计算新的映射关系应该存到table[i]位置，
    //i = hash & table.length-1，可以保证i在[0,table.length-1]范围内
    int i = indexFor(hash, table.length);
    //检查table[i]下面有没有key与我新的映射关系的key重复，如果重复替换value
    for (Entry<K,V> e = table[i]; e != null; e = e.next) {
        Object k;
        if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }

    modCount++;
    //添加新的映射关系
    addEntry(hash, key, value, i);
    return null;
}
```

其中，

```java
//如果key是null，直接存入[0]的位置
private V putForNullKey(V value) {
    //判断是否有重复的key，如果有重复的，就替换value
    for (Entry<K,V> e = table[0]; e != null; e = e.next) {
        if (e.key == null) {
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }
    modCount++;
    //把新的映射关系存入[0]的位置，而且key的hash值用0表示
    addEntry(0, null, value, 0);
    return null;
}
```

```java
final int hash(Object k) {
    int h = 0;
    if (useAltHashing) {
        if (k instanceof String) {
            return sun.misc.Hashing.stringHash32((String) k);
        }
        h = hashSeed;
    }

    h ^= k.hashCode();

    // This function ensures that hashCodes that differ only by
    // constant multiples at each bit position have a bounded
    // number of collisions (approximately 8 at default load factor).
    h ^= (h >>> 20) ^ (h >>> 12);
    return h ^ (h >>> 7) ^ (h >>> 4);
}
```

```java
static int indexFor(int h, int length) {
    return h & (length-1);
}
```

```java
void addEntry(int hash, K key, V value, int bucketIndex) {
    //判断是否需要库容
    //扩容：（1）size达到阈值（2）table[i]正好非空
    if ((size >= threshold) && (null != table[bucketIndex])) {
        //table扩容为原来的2倍，并且扩容后，会重新调整所有key-value的存储位置
        resize(2 * table.length); 
        //新的key-value的hash和index也会重新计算
        hash = (null != key) ? hash(key) : 0;
        bucketIndex = indexFor(hash, table.length);
    }
	//存入table中
    createEntry(hash, key, value, bucketIndex);
}
```

```java
void createEntry(int hash, K key, V value, int bucketIndex) {
    Entry<K,V> e = table[bucketIndex];
    //原来table[i]下面的映射关系作为新的映射关系next
    table[bucketIndex] = new Entry<>(hash, key, value, e);
    //个数增加
    size++; 
}
```

##### JDK1.8.0_271中源码

###### **1、Node**

key-value被封装为HashMap.Node类型或HashMap.TreeNode类型，它俩都直接或间接的实现了Map.Entry接口。

存储到table数组的可能是Node结点对象，也可能是TreeNode结点对象，它们也是Map.Entry接口的实现类。即table[index]下的映射关系可能串起来一个链表或一棵红黑树。


```java
public class HashMap<K,V>{
    transient Node<K,V>[] table;
    
    //Node类
    static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }
        // 其它结构：略
    }
    
    //TreeNode类
    static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
        TreeNode<K,V> parent;
        TreeNode<K,V> left;
        TreeNode<K,V> right;
        TreeNode<K,V> prev;
        boolean red; //是红结点还是黑结点
        TreeNode(int hash, K key, V val, Node<K,V> next) {
            super(hash, key, val, next);
        }
    }
    
    //....
}
```

###### **2、属性**

```java
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // 默认的初始容量 16
static final int MAXIMUM_CAPACITY = 1 << 30; //最大容量  1 << 30
static final float DEFAULT_LOAD_FACTOR = 0.75f;  //默认加载因子
static final int TREEIFY_THRESHOLD = 8; //默认树化阈值8，当链表的长度达到这个值后，要考虑树化
static final int UNTREEIFY_THRESHOLD = 6;//默认反树化阈值6，当树中结点的个数达到此阈值后，要考虑变为链表

//当单个的链表的结点个数达到8，并且table的长度达到64，才会树化。
//当单个的链表的结点个数达到8，但是table的长度未达到64，会先扩容
static final int MIN_TREEIFY_CAPACITY = 64; //最小树化容量64

transient Node<K,V>[] table; //数组
transient int size;  //记录有效映射关系的对数，也是Entry对象的个数
int threshold; //阈值，当size达到阈值时，考虑扩容
final float loadFactor; //加载因子，影响扩容的频率
```

###### **3、构造器**

```java
public HashMap() {
    this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted (其他字段都是默认值)
}
```

###### **4、put()方法**

```
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```

其中，

```java
static final int hash(Object key) {
    int h;
    //如果key是null，hash是0
	//如果key非null，用key的hashCode值 与 key的hashCode值高16进行异或
	//		即就是用key的hashCode值高16位与低16位进行了异或的干扰运算
		
	/*
	index = hash & table.length-1
	如果用key的原始的hashCode值  与 table.length-1 进行按位与，那么基本上高16没机会用上。
	这样就会增加冲突的概率，为了降低冲突的概率，把高16位加入到hash信息中。
	*/
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,boolean evict) {
    Node<K,V>[] tab; //数组
    Node<K,V> p;  //一个结点
    int n, i; //n是数组的长度   i是下标
    
    //tab和table等价
	//如果table是空的
    if ((tab = table) == null || (n = tab.length) == 0){
        n = (tab = resize()).length;
        /*
		tab = resize();
		n = tab.length;*/
		/*
		如果table是空的，resize()完成了①创建了一个长度为16的数组②threshold = 12
		n = 16
		*/
	}
    //i = (n - 1) & hash ，下标 = 数组长度-1 & hash
	//p = tab[i] 第1个结点
	//if(p==null) 条件满足的话说明 table[i]还没有元素
    if ((p = tab[i = (n - 1) & hash]) == null){
        //把新的映射关系直接放入table[i]
        tab[i] = newNode(hash, key, value, null);
        //newNode（）方法就创建了一个Node类型的新结点，新结点的next是null
    }else {
        Node<K,V> e; K k;
        //p是table[i]中第一个结点
		//if(table[i]的第一个结点与新的映射关系的key重复)
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;//用e记录这个table[i]的第一个结点
        else if (p instanceof TreeNode){ //如果table[i]第一个结点是一个树结点
            //单独处理树结点
            //如果树结点中，有key重复的，就返回那个重复的结点用e接收，即e!=null
            //如果树结点中，没有key重复的，就把新结点放到树中，并且返回null，即e=null
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        }else {
            //table[i]的第一个结点不是树结点，也与新的映射关系的key不重复
			//binCount记录了table[i]下面的结点的个数
            for (int binCount = 0; ; ++binCount) {
                //如果p的下一个结点是空的，说明当前的p是最后一个结点
                if ((e = p.next) == null) {
                    //把新的结点连接到table[i]的最后
                    p.next = newNode(hash, key, value, null);
                    //如果binCount>=8-1，达到7个时
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        //要么扩容，要么树化
                        treeifyBin(tab, hash);
                    break;
                }
                //如果key重复了，就跳出for循环，此时e结点记录的就是那个key重复的结点
                if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;//下一次循环，e=p.next，就类似于e=e.next，往链表下移动
            }
        }
        //如果这个e不是null，说明有key重复，就考虑替换原来的value
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e); //什么也没干
            return oldValue;
        }
    }
    ++modCount;
    
    //元素个数增加
	//size达到阈值
    if (++size > threshold)
        resize(); //一旦扩容，重新调整所有映射关系的位置
    afterNodeInsertion(evict); //什么也没干
    return null;
}
```

```java
final Node<K,V>[] resize() {
    Node<K,V>[] oldTab = table; //oldTab原来的table
    //oldCap：原来数组的长度
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    //oldThr：原来的阈值
    int oldThr = threshold;//最开始threshold是0
    
    //newCap，新容量
	//newThr：新阈值
    int newCap, newThr = 0;
    if (oldCap > 0) { //说明原来不是空数组
        if (oldCap >= MAXIMUM_CAPACITY) { //是否达到数组最大限制
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            //newCap = 旧的容量*2 ，新容量<最大数组容量限制
			//新容量：32,64，...
			//oldCap >= 初始容量16
			//新阈值重新算 = 24，48 ....
            newThr = oldThr << 1; // double threshold
    }
    else if (oldThr > 0) // initial capacity was placed in threshold
        newCap = oldThr;
    else {               // zero initial threshold signifies using defaults
        newCap = DEFAULT_INITIAL_CAPACITY; //新容量是默认初始化容量16
        //新阈值= 默认的加载因子 * 默认的初始化容量 = 0.75*16 = 12
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    if (newThr == 0) {
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    threshold = newThr; //阈值赋值为新阈值12，24.。。。
    //创建了一个新数组，长度为newCap，16，32,64.。。
    @SuppressWarnings({"rawtypes","unchecked"})
    Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    table = newTab;
    if (oldTab != null) { //原来不是空数组
        //把原来的table中映射关系，倒腾到新的table中
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {//e是table下面的结点
                oldTab[j] = null; //把旧的table[j]位置清空
                if (e.next == null) //如果是最后一个结点
                    newTab[e.hash & (newCap - 1)] = e; //重新计算e的在新table中的存储位置，然后放入
                else if (e instanceof TreeNode) //如果e是树结点
                    //把原来的树拆解，放到新的table
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                else { // preserve order
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    //把原来table[i]下面的整个链表，重新挪到了新的table中
                    do {
                        next = e.next;
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```

```java
Node<K,V> newNode(int hash, K key, V value, Node<K,V> next) {
    //创建一个新结点
    return new Node<>(hash, key, value, next);
}
```

```java
final void treeifyBin(Node<K,V>[] tab, int hash) {
    int n, index; 
    Node<K,V> e;
    //MIN_TREEIFY_CAPACITY：最小树化容量64
    //如果table是空的，或者  table的长度没有达到64
    if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
        resize();//先扩容
    else if ((e = tab[index = (n - 1) & hash]) != null) {
        //用e记录table[index]的结点的地址
        TreeNode<K,V> hd = null, tl = null;
        /*
			do...while，把table[index]链表的Node结点变为TreeNode类型的结点
			*/
        do {
            TreeNode<K,V> p = replacementTreeNode(e, null);
            if (tl == null)
                hd = p;//hd记录根结点
            else {
                p.prev = tl;
                tl.next = p;
            }
            tl = p;
        } while ((e = e.next) != null);

        //如果table[index]下面不是空
        if ((tab[index] = hd) != null)
            hd.treeify(tab);//将table[index]下面的链表进行树化
    }
}	
```

![jdk7 HashMap](jdk7HashMap.jpg)
![jdk8 HashMap](jdk8HashMap.jpg)

#### LinkedHashMap源码剖析

##### 源码

内部定义的Entry如下：

```java
static class Entry<K,V> extends HashMap.Node<K,V> {
	Entry<K,V> before, after;
	
	Entry(int hash, K key, V value, Node<K,V> next) {
		super(hash, key, value, next);
	}
}
```

LinkedHashMap重写了HashMap中的newNode()方法：

```java
Node<K,V> newNode(int hash, K key, V value, Node<K,V> e) {
    LinkedHashMap.Entry<K,V> p =
        new LinkedHashMap.Entry<K,V>(hash, key, value, e);
    linkNodeLast(p);
    return p;
}
```

```java
TreeNode<K,V> newTreeNode(int hash, K key, V value, Node<K,V> next) {
    TreeNode<K,V> p = new TreeNode<K,V>(hash, key, value, next);
    linkNodeLast(p);
    return p;
}
```

![jdk8 LinkedHashMap](jdk8LinkedHashMap.jpg)

### Set接口分析

#### Set集合与Map集合的关系

Set的内部实现其实是一个Map，Set中的元素，存储在HashMap的key中。即HashSet的内部实现是一个HashMap，TreeSet的内部实现是一个TreeMap，LinkedHashSet的内部实现是一个LinkedHashMap。

#### 源码剖析

**HashSet源码：**

```java
//构造器
public HashSet() {
    map = new HashMap<>();
}

public HashSet(int initialCapacity, float loadFactor) {
    map = new HashMap<>(initialCapacity, loadFactor);
}

public HashSet(int initialCapacity) {
    map = new HashMap<>(initialCapacity);
}

//这个构造器是给子类LinkedHashSet调用的
HashSet(int initialCapacity, float loadFactor, boolean dummy) {
    map = new LinkedHashMap<>(initialCapacity, loadFactor);
}

//add()方法：
public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}
//其中，
private transient HashMap<E,Object> map;
private static final Object PRESENT = new Object();

//iterator()方法：
public Iterator<E> iterator() {
    return map.keySet().iterator();
}
```

**LinkedHashSet源码：**

```java
//构造器
public LinkedHashSet() {
    super(16, .75f, true);
} 
public LinkedHashSet(int initialCapacity) {
    super(initialCapacity, .75f, true);//调用HashSet的某个构造器
}
public LinkedHashSet(int initialCapacity, float loadFactor) {
    super(initialCapacity, loadFactor, true);//调用HashSet的某个构造器
} 
```

**TreeSet源码：**

```java
public TreeSet() {
    this(new TreeMap<E,Object>());
}

TreeSet(NavigableMap<E,Object> m) {
    this.m = m;
}
//其中，
private transient NavigableMap<E,Object> m;

//add()方法：
public boolean add(E e) {
    return m.put(e, PRESENT)==null;
}
//其中，
private static final Object PRESENT = new Object();
```

### HashMap的相关问题

#### 1、说说你理解的哈希算法

hash算法是一种可以从任何数据中提取出其“指纹”的数据摘要算法，它将任意大小的数据映射到一个固定大小的序列上，这个序列被称为hash code、数据摘要或者指纹。比较出名的hash算法有MD5、SHA。hash是具有唯一性且不可逆的，唯一性是指相同的“对象”产生的hash code永远是一样的。


#### 2、Entry中的hash属性为什么不直接使用key的hashCode()返回值呢？

不管是JDK1.7还是JDK1.8中，都不是直接用key的hashCode值直接与table.length-1计算求下标的，而是先对key的hashCode值进行了一个运算，JDK1.7和JDK1.8关于hash()的实现代码不一样，但是不管怎么样都是为了提高hash code值与 (table.length-1)的按位与完的结果，尽量的均匀分布。


JDK1.7：

```java
    final int hash(Object k) {
        int h = hashSeed;
        if (0 != h && k instanceof String) {
            return sun.misc.Hashing.stringHash32((String) k);
        }

        h ^= k.hashCode();
        h ^= (h >>> 20) ^ (h >>> 12);
        return h ^ (h >>> 7) ^ (h >>> 4);
    }
```

JDK1.8：

```java
	static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```

虽然算法不同，但是思路都是将hashCode值的高位二进制与低位二进制值进行了异或，然高位二进制参与到index的计算中。

为什么要hashCode值的二进制的高位参与到index计算呢？

因为一个HashMap的table数组一般不会特别大，至少在不断扩容之前，那么table.length-1的大部分高位都是0，直接用hashCode和table.length-1进行&运算的话，就会导致总是只有最低的几位是有效的，那么就算你的hashCode()实现的再好也难以避免发生碰撞，这时让高位参与进来的意义就体现出来了。它对hashcode的低位添加了随机性并且混合了高位的部分特征，显著减少了碰撞冲突的发生。

#### 3、HashMap是如何决定某个key-value存在哪个桶的呢？

因为hash值是一个整数，而数组的长度也是一个整数，有两种思路：

①hash 值 % table.length会得到一个[0,table.length-1]范围的值，正好是下标范围，但是用%运算效率没有位运算符&高。

②hash 值 & (table.length-1)，任何数 & (table.length-1)的结果也一定在[0, table.length-1]范围。


JDK1.7：

```java
static int indexFor(int h, int length) {
    // assert Integer.bitCount(length) == 1 : "length must be a non-zero power of 2";
    return h & (length-1); //此处h就是hash
}
```

JDK1.8：

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)  // i = (n - 1) & hash
        tab[i] = newNode(hash, key, value, null);
    //....省略大量代码
}
```

#### 4、为什么要保持table数组一直是2的n次幂呢？

因为如果数组的长度为2的n次幂，那么table.length-1的二进制就是一个高位全是0，低位全是1的数字，这样才能保证每一个下标位置都有机会被用到。

举例1：

```java
hashCode值是   ？
table.length是10
table.length-1是9

？   ????????
9	 00001001
&_____________
	 00000000	[0]
	 00000001	[1]
	 00001000	[8]
	 00001001	[9]
	 一定[0]~[9]
```

举例2：

```java
hashCode值是   ？
table.length是16
table.length-1是15

？   ????????
15	 00001111
&_____________
	 00000000	[0]
	 00000001	[1]
	 00000010	[2]
	 00000011	[3]
	 ...
	 00001111    [15]
	 范围是[0,15]，一定在[0,table.length-1]范围内
```

`HashMap` 中的数组大小（`table.length`）被设计成 **2 的幂** 是为了提高性能和效率，特别是在计算索引和处理哈希值时，这种设计有几个主要原因：

1. 高效的取模操作（使用位运算替代取模）

当我们需要将哈希值映射到数组中的某个位置时，一般做法是使用取模操作（`hash % n`），即通过数组的大小 `n` 来对哈希值进行取模运算。取模运算比较耗时，尤其是在较大的数据规模下。

但如果 `n` 是 2 的幂，`HashMap` 可以用位运算替代取模操作：

```java
index = (n - 1) & hash;
```

其中：
- `n` 是数组大小，且为 2 的幂（如 16, 32, 64...）。
- `n - 1` 是二进制的低位全为 1 的数，例如 `n = 16`，则 `n - 1 = 15`，即 `0000 1111`。

位运算 `&` 是非常高效的，它比 `%` 运算快得多。因此，通过将数组大小设为 2 的幂，可以大大提升索引计算的速度。

2. 保证散列均匀分布

通过将数组大小设为 2 的幂，可以确保哈希值的所有位（尤其是低位）都能参与到索引的计算中。设为 2 的幂可以确保取模操作的低位全为 1，充分利用哈希值中的低位信息。例如，`n = 16` 时，`n - 1 = 15`，即 `0000 1111`，可以通过位与操作取哈希值的低 4 位作为索引。

如果数组大小不是 2 的幂，例如是 10，那么 `n - 1 = 9`，即 `0000 1001`。这种情况下取模时，只会使用哈希值的一部分位（如最低的 1 位和第 4 位），这可能会导致哈希值的某些位无法参与索引计算，增加哈希冲突的概率。

3. 扩容时保持均匀分布

当 `HashMap` 扩容时，通常将数组大小翻倍（从 16 增加到 32）。由于翻倍仍然是 2 的幂，哈希值的映射特性可以得到保留。这意味着，扩容后，原来在数组中第 `i` 个位置的元素，要么留在相同的位置（第 `i` 位置），要么移动到新数组中的 `i + oldCapacity` 位置。这种特性减少了重新计算哈希值和再分配元素的复杂度。

这是由于扩容后，新的数组大小 `n'` 仍然是 2 的幂，这样在计算 `(n - 1) & hash` 时，只是多了一个高位的判断条件，而不会影响到低位哈希的分布。

4. 减少哈希冲突

数组大小为 2 的幂能够最大限度利用哈希值的每一位，确保哈希值不同的键尽可能分布到不同的桶中。如果数组大小不是 2 的幂，某些位的信息可能无法参与索引计算，增加哈希冲突的可能性。因此，通过使用 2 的幂作为数组大小，可以减少冲突，提高查询和插入的效率。

总结

`HashMap` 的数组大小被设计成 2 的幂，主要是为了以下原因：
- 通过高效的位运算替代取模操作，提升索引计算效率。
- 保证哈希值能够均匀分布在整个数组上，减少哈希冲突。
- 在扩容时保证哈希值的良好分布，降低重新分配元素的成本。

这种设计是基于性能考虑的最佳选择。


#### 5、解决[index]冲突问题

虽然从设计hashCode()到上面HashMap的hash()函数，都尽量减少冲突，但是仍然存在两个不同的对象返回的hashCode值相同，或者hashCode值就算不同，通过hash()函数计算后，得到的index也会存在大量的相同，因此key分布完全均匀的情况是不存在的。那么发生碰撞冲突时怎么办？

JDK1.8之间使用：数组+链表的结构。


JDK1.8之后使用：数组+链表/红黑树的结构。


即hash相同或hash&(table.lengt-1)的值相同，那么就存入同一个“桶”table[index]中，使用链表或红黑树连接起来。

#### 6、为什么JDK1.8会出现红黑树和链表共存呢？

因为当冲突比较严重时，table[index]下面的链表就会很长，那么会导致查找效率大大降低，而如果此时选用二叉树可以大大提高查询效率。

但是二叉树的结构又过于复杂，占用内存也较多，如果结点个数比较少的时候，那么选择链表反而更简单。所以会出现红黑树和链表共存。

#### 7、加载因子的值大小有什么关系？

如果太大，threshold就会很大，那么如果冲突比较严重的话，就会导致table[index]下面的结点个数很多，影响效率。

如果太小，threshold就会很小，那么数组扩容的频率就会提高，数组的使用率也会降低，那么会造成空间的浪费。

#### 8、什么时候树化？什么时候反树化？

```java
static final int TREEIFY_THRESHOLD = 8;//树化阈值
static final int UNTREEIFY_THRESHOLD = 6;//反树化阈值
static final int MIN_TREEIFY_CAPACITY = 64;//最小树化容量
```

* 当某table[index]下的链表的结点个数达到8，并且table.length>=64，那么如果新Entry对象还添加到该table[index]中，那么就会将table[index]的链表进行树化。

* 当某table[index]下的红黑树结点个数少于6个，此时，
  * 当继续删除table[index]下的树结点，最后这个根结点的左右结点有null，或根结点的左结点的左结点为null，会反树化
  * 当重新添加新的映射关系到map中，导致了map重新扩容了，这个时候如果table[index]下面还是小于等于6的个数，那么会反树化

```java
package com.atguigu.map;

public class MyKey{
    int num;

    public MyKey(int num) {
        super();
        this.num = num;
    }

    @Override
    public int hashCode() {
        if(num<=20){
            return 1;
        }else{
            final int prime = 31;
            int result = 1;
            result = prime * result + num;
            return result;
        }
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj)
            return true;
        if (obj == null)
            return false;
        if (getClass() != obj.getClass())
            return false;
        MyKey other = (MyKey) obj;
        if (num != other.num)
            return false;
        return true;
    }

}

```

```java
package com.atguigu.map;

import org.junit.Test;

import java.util.HashMap;

public class TestHashMapMyKey {
    @Test
    public void test1(){
        //这里为了演示的效果，我们造一个特殊的类，这个类的hashCode（）方法返回固定值1
        //因为这样就可以造成冲突问题，使得它们都存到table[1]中
        HashMap<MyKey, String> map = new HashMap<>();
        for (int i = 1; i <= 11; i++) {
            map.put(new MyKey(i), "value"+i);//树化演示
        }
    }
    @Test
    public void test2(){
        HashMap<MyKey, String> map = new HashMap<>();
        for (int i = 1; i <= 11; i++) {
            map.put(new MyKey(i), "value"+i);
        }
        for (int i = 1; i <=11; i++) {
            map.remove(new MyKey(i));//反树化演示
        }
    }
    @Test
    public void test3(){
        HashMap<MyKey, String> map = new HashMap<>();
        for (int i = 1; i <= 11; i++) {
            map.put(new MyKey(i), "value"+i);
        }

        for (int i = 1; i <=5; i++) {
            map.remove(new MyKey(i));
        }//table[1]下剩余6个结点

        for (int i = 21; i <= 100; i++) {
            map.put(new MyKey(i), "value"+i);//添加到扩容时，反树化
        }
    }
}

```

#### 9、key-value中的key是否可以修改？

key-value存储到HashMap中会存储key的hash值（在节点数据结构如Node的域中有一个int hash来存储），这样就不用在每次查找时重新计算每一个Entry或Node（TreeNode）的hash值了，因此如果已经put到Map中的key-value，再修改key的属性，而这个属性又参与hashcode值的计算，那么会导致匹配不上。

这个规则也同样适用于LinkedHashMap、HashSet、LinkedHashSet、Hashtable等所有散列存储结构的集合。

#### 10、JDK1.7中HashMap的循环链表是怎么回事？如何解决？


避免HashMap发生死循环的常用解决方案：

- 多线程环境下，使用线程安全的ConcurrentHashMap替代HashMap，推荐
- 多线程环境下，使用synchronized或Lock加锁，但会影响性能，不推荐
- 多线程环境下，使用线程安全的Hashtable替代，性能低，不推荐

HashMap死循环只会发生在JDK1.7版本中，主要原因：头插法+链表+多线程并发+扩容。

在JDK1.8中，HashMap改用尾插法，解决了链表死循环的问题。


在 Java 的 `HashMap` 中，理论上确实有可能出现 **循环链表** 的问题，但这种情况非常少见，主要发生在多线程并发操作 `HashMap` 时且未使用适当的同步机制的情况下。在 JDK 1.8 之前的 `HashMap` 中，这个问题更加容易发生，尤其是在高并发下进行 `put` 操作时。

##### 循环链表的问题是如何产生的？

循环链表问题通常与 **多线程并发操作** 相关。在多线程环境下，如果多个线程同时向 `HashMap` 中进行插入操作（特别是当发生哈希冲突，需要多个节点形成链表的情况），而没有正确的同步机制时，链表可能会形成循环。形成循环后，继续进行操作时会导致程序陷入死循环，导致 CPU 占用率 100%，程序无法继续运行。

这个问题的根本原因是 `HashMap` 在并发访问时不是线程安全的。在 `put` 操作时，`HashMap` 可能会进行链表节点的插入或扩容操作，而这些操作在多线程环境下没有正确的同步控制时，可能会导致链表结构被破坏，甚至出现循环链表。

##### 循环链表的产生示例（简化示意）

假设有两个线程 `Thread1` 和 `Thread2`，它们同时对 `HashMap` 进行插入操作，且都要将元素插入到同一个位置（发生哈希冲突），导致需要在链表中插入新的节点。由于 `HashMap` 在早期 JDK 中没有正确的同步机制，可能出现以下情况：

https://www.bilibili.com/video/BV1n541177Ea/?spm_id_from=333.337.search-card.all.click&vd_source=6c26f427606a59575440e9bc6cec44af
##### 如何避免循环链表问题？

为了避免循环链表或其他类似的并发问题，在多线程环境下使用 `HashMap` 时，推荐使用以下方法：

1. **使用 `ConcurrentHashMap`**  
   如果需要在多线程环境下使用 `Map`，应该使用 `ConcurrentHashMap`，它是线程安全的，设计上已经考虑到了并发操作的安全性，并避免了类似的循环链表问题。`ConcurrentHashMap` 使用了更细粒度的锁分段机制，允许更高的并发度。

2. **同步 `HashMap`**  
   如果必须使用 `HashMap`，可以通过手动加锁来确保线程安全。例如，可以使用 `Collections.synchronizedMap(new HashMap<>())` 来创建一个同步的 `HashMap`，保证在多线程环境下的安全操作。

3. **避免手动使用 `HashMap` 在高并发场景**  
   尽量避免在高并发场景中直接使用 `HashMap`，尤其是 `put` 操作，因为这时哈希冲突和链表操作更加频繁。如果需要多线程的高性能 `Map`，应优先选择 `ConcurrentHashMap` 等专为并发设计的数据结构。

##### JDK 1.8 对 `HashMap` 的改进

在 JDK 1.8 中，`HashMap` 对链表结构做了重要改进，当链表中的节点数量超过一定阈值（默认 8 个）时，会将链表转换为红黑树，从而降低了链表过长带来的性能问题和冲突问题。但这一改进仅仅是针对链表长度过长的场景，并没有解决多线程并发问题。

因此，即便在 JDK 1.8 中，`HashMap` 仍然不是线程安全的，在多线程环境下依然可能发生循环链表的问题，除非使用了其他同步措施或专为并发设计的数据结构（如 `ConcurrentHashMap`）。

##### 总结

- 在多线程环境下，`HashMap` 确实可能出现 **循环链表** 的问题，这会导致程序进入死循环，耗尽 CPU 资源。
- 这个问题主要发生在并发环境下的 `put` 操作，由于 `HashMap` 不是线程安全的，所以多个线程同时操作时会破坏链表结构。
- 避免这种问题的方式是使用线程安全的集合类，例如 `ConcurrentHashMap`，或通过同步机制来保护 `HashMap` 的操作。


## 第十二条：始终要覆盖toString

toString 应该返回对象中包含的所有值得关注的信息，例如在共享通用字符串表示方法的抽象类中，编写一个toString，就像大多数集合中使用的那样

## 第十三条：谨慎地覆盖clone

Cloneable接口中没有任何方法，和一般的接口作用不同，它不是表明类可以做什么，而是改变了protected clone() 的行为

`Cloneable` 接口在 Java 中是一个标记接口（marker interface），它本身没有任何方法，唯一的作用是表明实现该接口的类支持“浅拷贝”（Shallow Copy）。Java 中的对象拷贝是通过 `Object` 类中的 `clone()` 方法实现的，而要使用该方法，类必须实现 `Cloneable` 接口，否则会抛出 `CloneNotSupportedException` 异常。

### 一、`Cloneable` 接口的作用

1. **标记功能**：`Cloneable` 是一个空接口（没有任何方法），表示类的实例可以合法地被拷贝。
2. **避免异常**：如果一个类没有实现 `Cloneable` 接口，但尝试调用 `clone()` 方法，那么 JVM 会抛出 `CloneNotSupportedException` 异常。

### 二、`clone()` 方法的工作机制

`clone()` 方法定义在 `Object` 类中，主要用于创建当前对象的一个副本。一般步骤如下：

1. **实现 `Cloneable` 接口**：要使用 `clone()`，类必须实现 `Cloneable` 接口，否则会抛出异常。
2. **重写 `clone()` 方法**：通常需要在类中重写 `clone()` 方法，修改其访问权限为 `public`，并调用 `super.clone()`，实现对象拷贝。

```java
public class Person implements Cloneable {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public Person clone() {
        try {
            return (Person) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError(); // 这种异常在实现了Cloneable后不应该发生
        }
    }
}
```

### 三、浅拷贝与深拷贝

- **浅拷贝**：默认的 `clone()` 方法只会进行浅拷贝，即只拷贝对象的基本字段值（对于引用类型字段，拷贝的是引用地址而不是引用对象）。
  - 例如，在拷贝一个对象后，两个对象内部的引用类型字段仍指向同一个对象，因此修改一个对象的引用字段可能会影响另一个对象。
- **深拷贝**：对于深拷贝，则需要对所有引用类型字段重新分配内存，生成新的对象，这通常需要手动实现。

```java
class Person implements Cloneable {
    private String name;
    private int age;
    private Address address;

    public Person(String name, int age, Address address) {
        this.name = name;
        this.age = age;
        this.address = address;
    }

    @Override
    public Person clone() {
        try {
            Person cloned = (Person) super.clone();
            // 实现深拷贝
            cloned.address = address.clone();
            return cloned;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}

class Address implements Cloneable {
    private String city;

    public Address(String city) {
        this.city = city;
    }

    @Override
    public Address clone() {
        try {
            return (Address) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}
```

在以上代码中，`Person` 类和 `Address` 类都实现了 `Cloneable` 接口，并在 `Person` 的 `clone()` 方法中通过调用 `Address` 的 `clone()` 方法来实现深拷贝。

### 四、注意事项

1. **使用场景**：`clone()` 方法适合需要快速复制对象的场景，但由于浅拷贝和深拷贝的区别，复杂对象的拷贝需要小心处理。
2. **更灵活的替代方案**：在现代 Java 编程中，`clone()` 方法因其复杂性和潜在的错误风险，并不常用，许多开发者更偏向于使用构造方法或序列化的方式来复制对象。



JVM 在调用 `Object.clone()` 方法时，会在内部对调用对象的类型进行检查，并且包含类似以下伪代码的逻辑：

```java
if (!(this instanceof Cloneable)) {
    throw new CloneNotSupportedException();
}
```

不过，这段代码并不是直接在字节码层插入的，而是由 JVM 在 `Object.clone()` 的本地方法实现中处理的。这段逻辑是 `Object.clone()` 方法的一部分，由 JVM 本地代码完成类型检查后决定是否抛出 `CloneNotSupportedException`。

---

### JVM 行为的具体说明
1. 当 `clone()` 方法被调用时：
   - JVM 会检查当前对象是否实现了 `Cloneable` 接口。
   - 如果没有实现 `Cloneable`，则抛出 `CloneNotSupportedException`。
   - 如果实现了 `Cloneable`，JVM 调用本地方法进行内存级别的对象拷贝。

2. `Object.clone()` 是一个本地方法，其实现由 JVM 提供，并且它对对象的克隆过程高度优化，包括：
   - 分配一个新的对象实例。
   - 将原对象的所有字段复制到新对象中（浅拷贝）。

3. **为什么需要 `instanceof Cloneable` 检查？**
   - Java 的设计理念是通过 `Cloneable` 作为标记接口，显式声明一个类支持克隆。
   - 通过 `instanceof` 操作符来检查类是否实现了该接口，是一种简单且有效的方式。

---

### 如何验证这段逻辑？

我们可以通过反射绕过直接调用 `Object.clone()`，验证没有 `instanceof Cloneable` 检查时会发生什么：

#### 示例代码
```java
class A {
    // 没有实现 Cloneable
}

public class Test {
    public static void main(String[] args) {
        A a = new A();
        try {
            // 直接通过反射调用 Object 的 clone 方法
            A cloneA = (A) a.getClass().getSuperclass().getDeclaredMethod("clone").invoke(a);
        } catch (Exception e) {
            e.printStackTrace(); // 抛出 CloneNotSupportedException
        }
    }
}
```

#### 运行结果
即使通过反射调用 `clone()`，JVM 依然会检查 `Cloneable`，并抛出如下异常：
```
java.lang.CloneNotSupportedException: A
```

---

### 总结

- JVM 在执行 `Object.clone()` 时确实会自动插入类似 `if (!(this instanceof Cloneable))` 的检查。
- 这段检查逻辑不是在字节码层面插入的，而是 `Object.clone()` 本地方法实现的一部分。
- 这种设计保证了只有显式实现了 `Cloneable` 的类才能成功克隆，未实现的类调用 `clone()` 会安全地失败。


