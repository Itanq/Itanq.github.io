+++
title = "C++ 类型转换"
date = 2017-02-10
[taxonomies]
categories = ["c++修道"]
tags = ["c++"]
+++

C++添加了四种类型转换运算符：
```
const_cast<type-name>(expression);
static_cast<type-name>(expression);
dynamic_cast<type-name>(expression);
reinterpret_cast<type-name>(expression)
```
其中`type-name`表示转换后的类型，`expresion`表示被转换的表达式。

<!-- more -->

## const_cast
const_cast 只能用于改变值的`const`或`volatile`属性。就是把常量转为变量，把变量转为常量。也就是说它的`type-name`和`expression`类型必须相同。

```c++
#include<iostream>
class High{};
class Low{};
int main()
{
    High fuck;
    const High* pfuck = &fuck;
    High* ph = const_cast<High*>(pfuck);// 合法;只改变类型属性。
    Low* pl = const_cast<Low*>(pfuck);　// 不合法;除了改变类型属性外还改变了类型。
    return 0;
}
```

## static_cast
static_cast 用于C++内置类型之间的转换以及相互联系的自定义类之间的转换。
```c++
double d = 5.987;
int n = static_cast<int>(d);
```
假设High类是Low的基类，而Pond是一个无关的类，则从High到Low,从Low到High的转换都是合法的，而从Low或者High到Pond的转换则是不允许的。
```c++
#include<iostream>
#include<cstdio>
class High{};
class Low:public High{};
class Pond{};
int main()
{
    High bar;
    Low blow;
    High* ph = static_cast<High*>(&blow);   //合法
    Low* pl = static_cast<Low*>(&bar);      //合法
    const Low* ll = static_cast<const Low*>(&bar);  // 合法
    Pond* pp = static_cast<Pond*>(&bar);            //不合法
    return 0;
}
```

## dynamic_cast
dynamic_cast是在运行阶段进行的类型转换检查。它有几个要点:

* dynamic_cast  只能用于类之间的转换，不能用于内置类型之间的转换。
* dynamic_cast转换如果成功的话返回的是转换后指针或引用，转换失败的话则会返回NULL。
* 类之间一定要有继承关系。
* 在进行向下类型转换的时候基类必须要有虚函数。
* 在类层次间进行上行转换时，dynamic_cast和static_cast的效果是一样的；在进行下行转换时，dynamic_cast具有类型检查的功能，比static_cast更安全。


所谓上行转换或下行转换就是对于一颗继承树而言，从派生类到基类的转换属于上行转换，也即向上类型转换；从基类到派生类的转换属于下行转换，也即向下类型转换。

```c++
#include<iostream>
#include<cstdio>
class High{public:virtual void f(){}};
class Low:public High{};
int main()
{
    High bar;
    Low blow;

    High* ph = dynamic_cast<High*>(&blow); // up
    Low* pl = dynamic_cast<Low*>(&bar); //down;不一定总成功。
    return 0;
}
```

## reinterpret_cast
相当于Ｃ风格的通用类型转换，任意两个类型之间都可以转换，但是不安全，很少会用。