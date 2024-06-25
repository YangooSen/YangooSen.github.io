---
title: effectiveCPP
katex: true
date: 2024-06-08 10:02:04
excerpt: 《effective c++》读书笔记
tag: cpp
---


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

This is all need almost all the time. The only exception is **when you need the value of a class constant during compilation of the class**, such as in the declaration of the array (where compilers insist on knowing the size of the array during compilation). Then the accepted way to compensate for compilers that fobid the in-class sepecification of initial value for static integral class constants is to use what is known as `the enum hack`

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

## item3: Use const whenever possible

