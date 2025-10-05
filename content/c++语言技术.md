### 重载
```C++
在类中重载多个函数
重载：函数的重载(函数名相同,函数的参数类型,参数个数,顺序不同)
   引用在重载的时候注意点:优先匹配最合适的函数
   默认参数在重载的时候注意点:注意二义性:即避免二义性
1.通过成员函数重载+号
2.通过全局函数重载+号

// 左移运算符重载:1.不用利用cout来重载左移运算符,因为无法实现cout在左侧;只能用全局变量来重载
返回引用和返回值结果一样，但是返回值不可以叠罗汉，即返回的是一个新的对象
// 引用为了一直对一个对象进行操作
// 重载后置++：返回值不可以作为重载的条件,所以用占位符区分:必须用int
```
### 数组
```C++
初始化
1.一维数组：int arr[5]={ , , ,}
2.二维数组：int arr[5][5]{{ , , },{ , , }}
```
### 值传递和地址传递
```C++
地址传递（Call by Reference）：形参用原型，是参用原型
在地址传递中，实际参数的地址被传递给形式参数，形式参数可以通过该地址来访问和修改实际参数的值。
值传递（Call by Value）：形参用指针，实参用&
在值传递中，函数调用时实际参数的值被复制给形式参数，形式参数在函数内部和外部作用域是独立的，修改形式参数的值不会影响实际参数的值。
在C++中，不能直接返回指向局部变量的指针或引用，因为局部变量在函数执行完毕后会被销毁，
这样返回的指针或引用指向的内存将不再有效。但是，可以通过值传递来返回局部变量的副本。get()函数

引用传递：引用传递是C++特有的一种参数传递方式，使用引用作为形式参数。
修改形式参数的值会直接影响实际参数的值。
所以在实际应用中，通过使用const int & a作为形参，既避免了值传递复制浪费资源，
也避免了在子函数中修改实参
```
### 模板(属于泛型)
```C++
template <class T>
inline void deallocate(T* buffer) {
    // 调用 operator delete 释放指针指向的内存空间
    ::operator delete(buffer);
}

模板声明 template <class T>：这表明 deallocate 是一个模板函数，可用于任何类型 T。通过这种方式，你可以使用任何数据类型的指针作为参数调用此函数。
函数定义 inline void deallocate(T* buffer)：
inline：这个关键字建议编译器尝试内联展开函数，即在每个调用点将函数体替换为函数调用。这有助于减少函数调用的开销，尤其是在小函数中效果明显。
void：函数返回类型为 void，意味着这个函数不返回任何值。
deallocate(T* buffer)：函数接受一个指向类型 T 的指针 buffer 作为参数，这个指针应指向先前通过 new 操作符动态分配的内存。
函数体 ::operator delete(buffer);：
::operator delete(buffer)：使用全局作用域运算符 :: 来确保调用的是全局的 delete 运算符，而非可能在某些类中被重载的版本。这保证了无论上下文如何，都会调用标准的内存释放过程。
::operator delete(buffer) 这段代码使用了全局作用域符号 ::，以确保调用的是全局的 delete 操作符。这里并没有对 delete 操作符进行重载，而是直接调用标准的全局 delete 操作符
这行代码释放 buffer 指针指向的内存。适用于通过 new 表达式分配的任何类型的对象或原始内存。
总的来说，这个函数提供了一种类型安全的方式来释放任意类型的动态分配内存，增加了代码的可复用性和抽象级别。

关键字 explicit：
explicit 关键字用于防止构造函数参与隐式类型转换，确保只有在明确调用时才能用单个参数构造 vector 对象。这有助于避免意外的类型转换可能引起的错误。
```
```C++
 使用时机:功能相同，类型不同：eg:dataPrintHelper()
 template <typename T = 默认类型>   包括:函数模板，类模板 
模板：
函数模板/类模板->编译两次
第一次编译：直接将T放入，g++ -E test.cpp -o test.i
第二次编译：运行时，对T进行分析

使用的时候显示指定add<int>(a, b)，避免自动推导
-----------------------------------------------


 ----------------------------------------------------
函数模板（函数模板有两次编译，所以同名的时候可以编译过）
普通函数和函数模板的调用规则：都可以发生重载
1.优先调用普通函数,只要普通函数是可以匹配(包括普通函数形参为int，但是传入实参为char可以，传入实参为float不可以)的，都是普通函数
    定义好的类型和自定义类型都可以
2.可以通过空模板参数列表强制调用函数模板
    add<int>(a, b)
3.如果函数模板可以产生更好的匹配，优先调用函数模板
普通函数不需要教，函数模板需要教才会隐式类型转换
 普通函数可以发生隐式类型转换
    cout << myAdd(a, c) << endl;
    自动类型推导,无法进行隐式类型推导
    cout << myAdd01(a, c) << endl;//都会调用普通函数
    cout << myAdd01<int>(a, c) << endl; // 显示类型推导可以
    相当于告诉编译器把T变成int类型
----------------------------------------------------  
函数模板的缺陷
自定义类型操作运算失败
多方法1：重载运算符
方法2：模板具象化(相当于二次编译的时候让编译器用这个)
template <typename T>
T compare(T t1, T t2)
{
    return t1 > t2 ? t1 : t2;
}
template <> Person compare<Person>(Person t1, Person t2)
{
}
----------------------------------------------------  
 类模板：用class T，用以和函数模板区分
 template<class T>
class Demo{    
}
在类模板中Demo<int>作为类的标识

 类模板和函数模板的区别：
1.类模板没有自动类型推导的使用方式，只有显示指定类型
即Person<int> p;

类模板分文件编写
1.类模板的成员函数是在调用的时候创建，会导致文件编写时链接不到
2.解决方式：将声明和实现(不分离)写到同一个文件当中，并更改后缀名为.hpp,hpp是约定，不是强制
类模板的头文件中实现成员函数，而不是将实现代码分离到独立的源文件中。这样，在每个使用类模板的地方都能看到并实例化相关的成员函数。


类模板做函数参数：
showData(const Dtata<int>& p1)


 
类模板类外实现
template <class T>
Dtata<T>::Dtata(T a, T b)
{
    this->a = a;
    this->b = b;
}
类模板与继承
1.当子类父类，父类是一个类模板时，子类在声明时，要指出父类中的T的类型，否则无法声明，无法确定父类的内存大小
2.如果想灵活继承父类，子类也要变成父类
类模板派生正常类
Dtata<int>,确定空间

类模板派生类模板
template <class T1, class T2>
class Base : Dtata<T2>
{
private:
    T1 a;

public:
    Base(T1 c, T2 b) : Dtata<T2>(c, c)
    {
        std::cout << "子函数" << std::endl;
        std::cout << a << " " << c << std::endl;
    }
};

类模板配合友元的类内实现和类外实现
1.类内写：直接声明友元的存在
2.类外写：需要提前编译让编译器知道全局函数的存在
 
通过全局函数打印类内信息
 
类外实现，需要让编译器提前知道函数模板和类的存在

普通函数模板/普通函数作为 类模板的友元
template <class T>
class Data
{
    friend void printData(Data<int> ob);
    template <typename T1>
    friend void printData2(Data<T1> ob);

private:
    T a;

public:
    Data()
    {
        std::cout << "有参构造" << std::endl;
    }
    Data(T a)
    {
        this->a = a;
        std::cout << "又参构造" << std::endl;
    }
};
//普通函数作为类模板的 友元：目的是访问私有数据

void printData(Data<int> ob)
{
    std::cout << "a = " << ob.a << std::endl;
}
//函数模板作为类模板的友元
template <typename T1>
void printData2(Data<T1> ob)
{
    std::cout << "  1111" << ob.a << std::endl;
}

获取数据类型
getData<decltype(out)>(out, verbose); declaration type
```
### 返回值类型
```C++
std::result_of 是一个用于获取函数调用结果类型的模板类，它在C++11标准中引入。通过 std::result_of，可以获取函数对象、函数指针或者函数成员指针调用后的返回值类型。
#include <type_traits>

// 定义一个函数
int foo(int x) { return x + 1; }

int main() {
    // 使用 std::result_of 获取函数调用的返回类型
    using result_type = typename std::result_of<decltype(foo)(int)>::type;

    // 使用 result_type 来声明变量
    result_type result = 0;
    
    return 0;
}

try{}catch(...){}:catch（...）表示捕获任何异常类型

template <typename From = int> 默认模板参数，没参数传入是int，有参数传入是传入的参数

UNUSED(topicTo); 是一种常见的编程惯例，用于标记一个变量或参数未使用的情况。这通常出现在代码中，当某些变量被定义但实际上并未在后续代码中被使用时。
在很多编程语言中，例如C++，编译器可能会发出警告或错误提示，指出未使用的变量。为了避免这些警告，程序员可以在未使用的变量之后加上 UNUSED 的宏定义或函数，以告知编译器忽略这个变量的未使用警告。
int calculateSum(int num1, int num2) {
    UNUSED(num2); // 标记 num2 未使用

    return num1;
}

函数指针：
return_type (*ptr_name)(param_list);
其中，return_type 是函数的返回类型，ptr_name 是函数指针的名称，param_list 是函数的参数列表。通过这种方式声明的函数指针，可以指向具有相同返回类型和参数列表的函数。
```
### 函数指针
```Plain
函数指针：int *  (*test)(int a,int b);//定义test为函数指针,
场景：最基本的回调方法，适用于简单的回调需求。
优点：直接、简单，没有额外开销。
缺点：不能携带上下文信息，不够灵活。
函数对象（functor）：
场景：适用于需要携带上下文信息或状态的回调。
优点：可以携带额外参数，更具灵活性。
缺点：编写和管理多个函数对象可能会显得繁琐。
#include <iostream>// 定义一个函数对象 AddFunctor，用于计算两个整数的和
struct AddFunctor {
    int operator()(int a, int b) {
        return a + b;
    }
};

int main() {
    // 创建一个实例化的函数对象
    AddFunctor addFunc;

    // 使用函数对象调用重载的 operator() 函数来计算结果int result = addFunc(3, 4);

    std::cout << "The result of addFunc(3, 4) is: " << result << std::endl;

    return 0;
}

Lambda 表达式：
场景：适用于轻量级、匿名的回调需求。
优点：简洁、直观，能够轻松捕获外部变量。
缺点：可读性可能稍差，不适合复杂逻辑。

typedef 语句定义的不是一个函数类型，而是一个函数指针类型。函数指针类型的定义并不需要两个参数，因为它只是指向一个特定类型的函数，而不是定义函数本身。

typedef double(*FUNC)(double) 的含义：
typedef：用于创建类型别名。
double：表示函数返回类型为 double。 
(*FUNC)：表示 FUNC 是一个指针，指向一个返回类型为 double、参数为 double 的函数。
(double)：表示该函数接受一个 double 类型的参数。
因此，FUNC 是一个指向返回类型为 double、参数为 double 的函数的指针类型的别名。
如果想要定义一个函数类型，需要指定函数的参数列表和返回类型，例如：

typedef double FUNC(double);
typename有两种用法，第一种用于声明模板时，表示模板类型参数，如下所示。在用于模板声明时，typename 和 class 等价，具有同等含义。
typename的第二种用法，用于表示一个类内所指的类型为一个 “class” 类型（内置类型、自定类型），而非其他（变量、函数类型）。更一般的说法是，typename 用来告诉编译器类中的嵌套名称表示的是一个类型。看下面一个例子。
```
##  强行转换
```C++

在 C++ 中，(uint8_t*)(recv_msgs[1].data) 和 static_cast<uint8_t*>(recv_msgs[1].data()) 
都可以用于将指针转换为 uint8_t* 类型。
两者实际上是相等的，并且在大多数情况下都能正常工作。
原始 C 式的强制类型转换 (uint8_t*)(recv_msgs[1].data) 在 C++ 中仍然是有效的，但它通常不被推荐使用。
这是因为 C 式的强制类型转换在语法上更灵活，但也更容易出错并丧失了编译器的类型检查能力。

C++ 中引入了 static_cast、reinterpret_cast 等更具类型安全性和可读性的强制类型转换操作符。
1.使用 static_cast 进行类型转换时，编译器会在转换前检查是否存在合理的类型转换方式，并在编译时进行类型检查，
以确保类型转换是安全的。因此，static_cast 在类型转换上比较严格，并且不允许执行具有潜在危险的转换。会改变底层的二进制。通常用于处理阶段之类的
2.reinterpret_cast 是 C++ 中的一种类型转换运算符，用于进行底层的重新解释类型转换。不会改变底层的二进制
```
### Using关键字
```C++
Using 关键字：
using关键字在C++中用于创建别名和类型别名。
使用using关键字可以为现有的类型或复杂的类型定义创建一个简洁的别名。这样做可以方便代码的阅读和理解。
以下是使用using关键字的一些常见场景：
1.创建类型别名：可以使用using关键字为已有的类型创建一个新的名称，
这样可以使类型的名称更清晰、更具描述性。例如：using MyInt = int; 将MyInt作为int类型的别名。
2.创建模板别名：可以使用using关键字为模板类型创建一个简洁的别名，以方便在代码中使用。
例如：using MyVector = std::vector<int>; 将MyVector作为std::vector<int>类型的别名。
3.创建函数指针别名：可以使用using关键字为函数指针类型创建一个别名，以方便在代码中使用函数指针。(函数指针)
例如：using MyFuncPtr = void(*)(int); 将MyFuncPtr作为指向接受int参数并返回void的函数指针类型的别名。
```
```
using namesapce xxx，不同的namespace的相同类型视作不同类型
```
### 友元
```C++
友元,另一个类访问该类的成员变量
1.全局函数做友元 2.类做友元 3.成员函数做友元
```

## c++面向对象三大特性

```Plain
面向对象三大特性：
1.封装:对象+行为作为一个整体 2.将属性和行为加以权限控制(public,private,protected)
2.继承：提高代码重用:继承->多继承->菱形继承->
继承（class 子类：继承方式 父类名）
public继承把父类的权限整体继承
protected继承把父类public和protected都变成protected
private把public和protected都变成private

继承中同名的静态成员的处理方式
与非静态一致，加作用域 

如果子类中出现和父类同名的成员函数，子类的同名成员函数会隐藏掉父类中所有的同名成员函数（包括参数不同的）

多继承 class 子类 ：继承方式 父类1 ， 继承方式 父类2...
多继承可能会引发父类中有同名成员出现，需要加作用域区分（实际中不建议使用多继承）

菱形继承（钻石继承）会出现两次继承公共祖先的问题：用virtual虚继承来解决
animal称为虚基类;使用虚继承后，继承了两份指针(虚基类指针vbptr)，虚基类指针指向虚基类表(vtable),虚基类表记录的是通过
虚基类指针访问公共祖先数据的偏移量

对于静态变量：
派生类会继承基类的静态变量，但是派生类无法重载静态变量。派生类内的静态变量与基类的静态变量是独立的，两者并不共享内存空间。
对于静态函数：
派生类同样会继承基类的静态函数。如果在派生类中定义同名的静态函数，则会隐藏基类中的同名静态函数，而不是重载它。

3.多态(poly/mor/phism)：允许不同类型的对象以相同的方式被处理。
静态重载：函数重载,运算符重载以及重定义：编译阶段
动态多态：虚函数和派生类：运行阶段
  动态多态的条件：
  1.有继承关系
  2.子类重写父类的虚函数（重写：函数返回值类型，函数名，参数列表完全相同）
// 动态多态使用：父类指针指向子类对象就可以使用
// 使用后，子类的vfptr覆盖掉父类的vfptr
// 地址早绑定，在编译阶段确定了函数的地址
如果想让猫说话，需要在运行阶段绑定，在父类上加virtual就可以
----------------------------------------------------
//基类
class Base {
public:
    // 虚函数virtual 
    void display() {
        std::cout << "Base class" << std::endl;
    }
};
// 派生类
class Derived : public Base {
public:
    // 重写虚函数void display() override {
        std::cout << "Derived class" << std::endl;
    }
};
int main() {
    Base* basePtr;
    Base baseObj;
    Derived derivedObj;
    basePtr = &baseObj;  // 基类指针指向基类对象
    basePtr->display();  // 调用基类的虚函数
    basePtr = &derivedObj;  // 基类指针指向派生类对象
    basePtr->display();     // 调用派生类的重写虚函数return 0;
}
------------------------------------------------------
```
## 虚函数/纯虚函数/虚析构/纯虚析构
```C++
虚构函数由来
父类强调共性，子类强调个性；所以继承之后，需要重写子类的函数，
这时候需要一个算法，可以让在需要时，
指向需要的子类->这时候的需要找出所有子类的共性->共性即父类的指针/引用
->这就解释了为什么要用父类指针存储子类地址，但是又要调用子类，所以必须传子类
父类指针指向子类空间带来了一些问题
不加虚构函数的时候
class Animal
{
private:
    /* data */
public:
    void speak()
    {
        std::cout << "Animal speak" << std::endl;
    }
};
class Dog : virtual public Animal
{
public:
    void speak()
    {
        std::cout << "Dog speak" << std::endl;
    }
};
int main(int argc, char const* argv[])
{
    //父类指针指向子类空间
    Animal* p = new Dog;
    p->speak();//Animal speak
    return 0;
}
仍然会指向父类空间；因为没有声明为虚函数，所以编译器会检查指针类型，根据指针类型类型进行调用；
即优先级：虚函数>指针类型
想让动态绑定：1.父类定义虚函数 2.子类重写（只有内容不一样）
加虚构函数的时候
class Animal
{
private:
    /* data */
public:
    virtual void speak() = 0;
};
class Dog : virtual public Animal
{
public:
    virtual void speak()    //按理来说写不写都可以都会被认为是虚函数，但是必须写上
    {
        std::cout << "Dog speak" << std::endl;
    }
};
int main(int argc, char const* argv[])
{
    //父类指针指向子类空间
    Animal* p = new Dog;
    p->speak();    //Animal speak
    return 0;
}

为什么加了virtual就可以了->动态绑定机制
c语言实现多态机制：https://blog.csdn.net/weixin_42142630/article/details/123260819
动态绑定机制：子类继承父类，会继承父类的虚函数指针和虚函数表，一旦重写父类虚函数，就会在子类中更新虚函数表

纯虚函数
virtual void name ()=0;
->父类不会生成对象(一定)&&子类一定会是重写父类所有的纯虚函数,子类必须全部实现否则也将成为抽象类 
->父类函数体无意义:用纯虚函数：virtual void func()=0;加入=0是为了区别函数的声明
->类中有纯虚函数，这个类就是抽象类
抽象类必须被继承，如果子类不重写父类纯虚函数，也变成抽象类

如果子类开辟了堆区，父类无法释放子类的析构函数，只会释放自己的析构函数
虚析构：
通过父类的指针，释放子类的空间
也是通过vptr，在继承的时候，虚析构指向的vbftable修改为子类的析构函数入口地址，并且自动析构父类
基类需要被实例化，并且希望通过基类指针来删除派生类对象时

纯虚析构 (通常类内声明，类外实现)
本质是析构函数，析构函数为了完成类的清理工作->必须实现函数体
基类只用于作为接口规范，不应该被实例化的时候，适合使用纯虚析构函数
纯虚析构也需要实现，需要让他跑一遍，清楚父类堆区的内容，类也属于抽象类



constexpr：用于计算常量表达式，表示在编译时计算常量的值

 使用const ClassDemo & cd作为型参可以减少数据的拷贝，提高性能,const使其不可修改，提高安全性
 std::function是C++标准库提供的一个通用的函数封装类模板。
 它可以包装任意可调用对象（函数、函数指针、lambda表达式等），并提供统一的调用接口。
```
## lamaba表达式
```C++
lamaba表达式
auto fut = std::async(std::launch::async, [&]() {0

std::async(std::launch::async, [&]()：这里调用了 std::async 函数，指定了启动策略为 
std::launch::async，表示任务会立即在新线程中执行。
Lambda 表达式 [&]()表示创建了一个捕获了当前作用域所有变量的 Lambda 函数。
[&] 表示通过引用捕获所有外部变量，
 这样可以在 Lambda 内部修改外部变量的值。
{}：Lambda 函数的主体，里面包含了实际的任务代码。
auto fut =：将 std::async 的返回值（即代表异步执行结果的 std::future 对象）存储在 fut 变量中。
通过这个 std::future 对象，可以获取 Lambda 函数执行的结果或等待任务完成。

->T&: This specifies the return type of the lambda function. In this case, 
it returns a reference (&) to an object of type T.
This means that when the lambda is called, it returns a reference to an object of type T. 
The arrow (->) is used to specify the return type in a lambda expression.

auto func = [&]() -> int {
        return 42;
    };
```
### Using namespace std;

```C++
这个关键字适用于一个文件里边都用统一的域，
更常用的：如果一个文件里两边或者多种不同的域，使用std::分别给变量加域
```
### c++构造函数
```C++
1.构造函数的方法:
括号法(相当于创建匿名对象:当前行执行完会立即释放);类名()
Classname();  // 调用默认构造函数
显示法;
Classname obj;  // 调用默认构造函数
Classname obj(1,2,3);
隐式转换法(实参派生类传给形参基类)：
class Base {
  public:
    Base() { cout << "Base constructor" << endl; }
};

class Derived : public Base {
  public:
    Derived() { cout << "Derived constructor" << endl; }
};

void test(Base b) {
  // 函数中传递基类对象，但实际上传入了派生类对象，会发生隐式转换
}

int main() {
  Derived d;
  test(d);  // 传递派生类对象给基类对象参数
}
类 类名();会认为是函数的声明
son so();会认为是一个返回值为son的无参函数

2.构造函数：父类->成员变量（包括类成员）->子类；析构正好相反
简记:父成子

3.子类会自动调用成员对象，父类 的默认构造->想调用类成员/父类的有参构造，必须使用初始化列表（父类用类名，类成员用成员名）
    son(int a, int b, int c) : Base(a), o(b)//Base是类名，子类成员Other o是用成员名
    {
        std::cout << "子类又参构造 c:" << c << std::endl;
    }

4.重定义：父类子类同名处理（子类对父类所有同名函数进行屏蔽，加作用域可以破除屏蔽）
：最简单，最安全的方式：加作用域->访问父类成员加作用域，访问子类成员用不加（子类成员优先级高）
s.func();
s.Base::func();
区别于重载：重载是无继承关系，同一作用域，参数个数，顺序，类型任一不同 都可以重载
区别于重写：重写是除了函数体不同，其他都一样
重定义：只有名字相同即可，其余不做限制

5.子类无法继承父类的内容：构造，析构，operator=(复制运算符的重载)


6.c++编译器会给一个类添加四个函数
 1.默认构造（空） 2.默认析构（空） 3.默认拷贝函数(浅拷贝) 4.赋值运算操作符operator=(对属性进行值拷贝)
 
 仿函数是一个类或者结构体，重载了 operator() 运算符，使得该类的实例可以像函数一样被调用。仿函数通常用于实现自定义的比较、排序、映射等功能。
 函数调用运算符重载，即（）重载：由于使用起来非常像函数调用，所以称为仿函数
 class Compare {
public:
    bool operator()(int a, int b) {
        return a > b;  // 自定义比较逻辑，返回a是否大于b
    }
};

int main() {
    Compare compare;

    int num1 = 5, num2 = 3;
    if (compare(num1, num2)) {
        std::cout << num1 << " is greater than " << num2 << std::endl;
    } else {
        std::cout << num1 << " is not greater than " << num2 << std::endl;
    }

    return 0;
}
```
### 拷贝构造函数
```C++
拷贝构造函数
globalConfig(const globalConfig&)                    = delete;
该类的拷贝构造函数被禁用（= delete），意味着对象不能通过拷贝构造函数进行复制。
左值引用最常见的应用，但是他是const，是常量的左值引用，即左值和右值都可以使用
通常情况下，会选择禁用拷贝构造函数的原因包括：
1.防止浅拷贝：如果一个类包含指针等资源，进行浅拷贝可能导致多个对象共享同一份资源，
当其中一个对象释放资源时，其他对象可能无法再正常访问，导致程序行为异常。
2.保护不可复制对象：有些类可能包含不可复制的成员变量，或者类本身就是单例类、唯一实例等，在这种情况下，禁用拷贝构造函数可以确保对象不能被复制，保证了设计上的一致性。
3.提高性能：有些场景下，对象的复制是不必要的，禁用拷贝构造函数可以防止不必要的复制操作，提高程序性能。
4.避免逻辑错误：有时候由于业务逻辑的特殊性，对象的拷贝可能导致逻辑错误或不符合预期的行为，禁用拷贝构造函数可以避免这种情况的发生。

拷贝构造函数调用时机:
1.已经创建完毕的对象来初始化一个对象 2.值传递方式给函数传值(即值传递会传递一个拷贝一个副本)

类的内部：
成员默认private；
共有public,protected,private
默认一个构造(用户有系统无),析构,拷贝(同构造)构造;也可以重载,构造函数自动调用,不用写void
构造函数按照参数分类:1.有参 2.无参
构造函数按照类型分类:1.普通构造函数 2.拷贝构造函数(不要用匿名对象创建拷贝对象)

拷贝构造函数目的：
让对象在初始化的时候能够像变量一样直接赋值：参数永远是const 类名的引用，适合用初始化列表来实现

浅拷贝和深拷贝：
        深拷贝:在堆区重新分配内存,进行拷贝
        浅拷贝:编译器提供的赋值操作,使两把钥匙指向同一个房子:
        拷贝函数的指针释放->利用深拷贝解决,即使用拷贝函数时,重新申请一个(即自己写一个拷贝函数)
```
### 常量函数
```C++
void test() const{}
将成员函数声明为常量成员函数有几个好处：
安全性和可维护性：常量成员函数可以确保在函数内部不会修改对象的状态，从而增加代码的安全性。这样一来，其他开发人员在调用常量成员函数时也可以放心地知道对象的状态不会被改变。
代码优化：编译器可以对常量成员函数进行代码优化。因为常量成员函数不会修改对象的状态，编译器可以在某些情况下执行更多优化，提高程序性能。
与常量对象兼容：常量成员函数可以被常量对象调用。如果一个对象是常量（即通过 const 修饰），则只能调用对象的常量成员函数，这是因为常量对象不允许修改其状态。
语义明确：通过将不修改对象状态的函数声明为常量成员函数，可以使类的接口更加清晰和语义化。
总体上，将常量成员函数用于表明该函数不修改对象状态，这样做既能提高代码的安全性和可维护性，也能使代码更具语义明确性。因此，在设计类时，如果有某个成员函数不需要修改对象状态，就可以考虑将其声明为常量成员函数。
希望这些解释对您有帮助。如果您有任何其他问题，请随时告诉我。我很乐意继续帮助您。
```
### 程序入口函数
```SQL
(int argc, char** argv)是C/C++程序中main函数的参数，其中argc表示命令行参数的数量，argv是一个指向字符指针数组的指针，每个字符指针指向一个命令行参数的字符串。
int argc: 表示命令行参数的数量，包括程序本身在内，即argc至少为1。
char** argv: 是一个指向字符指针数组的指针，它用于存储命令行参数的字符串。每个argv[i]（其中0<=i<argc）指向一个以null结尾的C字符串，即命令行参数的字符串。
例如，在命令行运行程序时可以这样传递参数：
bash
./myprogram arg1 arg2 arg3
在这种情况下，argc的值将为4（包括程序名本身），而argv将指向以下内容：
argv[0]指向"./myprogram"
argv[1]指向"arg1"
argv[2]指向"arg2"
argv[3]指向"arg3"
```
### 多线程多进程

```SQL
在C++11出来之前，如果我们想要在C++中实现多线程，需要借助操作系统平台提供的API，比如Linux的<pthread.h>，或者windows下的<windows.h>

多线程共享堆内存和方法区
https://blog.csdn.net/lzyzuixin/article/details/135054742
多线程只能运行一次
std::once
创建多线程：
std::thread()
```
### 枚举类

```SQL
在C++中，使用 enum class 定义枚举类可以提供更好的封装性和类型安全性，相比传统的 enum 类型，在设计上更为优越。以下是一些 enum class 相对于传统 enum 的优势：
作用域限定：enum class 中的枚举值被限定在枚举类的作用域内，不会自动转换为整数类型，避免了与其他作用域中相同名称的变量或函数冲突。
类型安全：enum class 是强类型的枚举类，不会自动隐式转换为整数，需要显式地使用枚举类作用域进行访问。
可读性：由于 enum class 中的枚举值需要通过作用域来访问，可以提高代码的可读性，避免不小心混淆或误用不同枚举类型的情况。
编译时检查：使用 enum class 可以在编译时捕获一些潜在的错误，比如将不同类型的枚举值相互比较或赋值。
总而言之，通过使用 enum class 并将枚举值放在类的作用域中，可以提高代码的安全性、可读性和健壮性。因此，推荐在 C++ 中使用 enum class 来定义枚举类型。
```
### C++20
```C++
std::latch 

#include <iostream>
#include <latch>
#include <vector>
std::latch lathc
{
    10
}
void f(int id)
{
    std::cout << "执行任务" << std::endl;
    lathc.arrive_and_wait();    //计数器减少1并且继续等待为0
}
int main(int argc, char const* argv[])
{
    std::vector<std::jthread> threads;
    lathc.count_down(5);    //一口气减少5个
    for (int i = 0; i < 5; i++)
    {
        threads.emplace_back(f, i);
    }

    return 0;
}
```
```C++
std::thread 的一种替代类型，不同之处在于 std::jthread 在其析构函数中会自动调用 join() 方法，确保线程的执行完全结束。
std::jthread 是 C++20 标准中引入的一个类型，用于表示可以加入的线程（joinable thread）。它是 std::thread 的一种替代类型，不同之处在于 std::jthread 在其析构函数中会自动调用 join() 方法，确保线程的执行完全结束。
使用 std::jthread 类型可以简化线程管理，避免因为忘记调用 join() 或 detach() 方法而导致的线程资源泄漏问题。当 std::jthread 对象被销毁时，会自动调用 join() 方法，等待线程执行完成，保证线程资源的正确释放。

#include <iostream>
#include <thread>

void threadFunction() {
    std::cout << "Thread is running..." << std::endl;
}

int main() {
    std::jthread myThread(threadFunction); // 创建一个 jthread 对象，并传入线程函数
    
    // 等待线程执行完成
    myThread.join();
    
    std::cout << "Thread has completed." << std::endl;
    
    return 0;
}
```
### 局部作用域

```Bash
#include <iostream>int main() {
    int x = 5;

    {
        int y = 10;
        std::cout << "Inside code block: x = " << x << ", y = " << y << std::endl;
    }

    // 在这里无法访问 y，因为 y 的作用域仅限于上面的代码块内// std::cout << "Outside code block: x = " << x << ", y = " << y << std::endl; // 这行代码会导致编译错误return 0;
}
```
# 编译

## 条件预编译

```C++
#ifdef DISTRUBED
分布式的函数
#else MPU
主板函数
#else LPU
备板函数
#endif
```

```Bash
g++ -o output  -DDEBUG testundermap.cpp
cjl@cjl-ThinkPad-T14-Gen-2i:~/Desktop/code/0.cjl$ ./output 
没有宏定义
拥有宏定义
cjl@cjl-ThinkPad-T14-Gen-2i:~/Desktop/code/0.cjl$ g++ -o output   testundermap.cpp
cjl@cjl-ThinkPad-T14-Gen-2i:~/Desktop/code/0.cjl$ ./output 
没有宏定义

#include <iostream>

int main(int argc, char const* argv[])
{
    std::cout << "没有宏定义" << std::endl;
#ifdef DEBUG
    std::cout << "拥有宏定义" << std::endl;
#endif
    return 0;
}
```

## 数组

```C
初始化
char cNum[MAX_SIZE];
cNum[0] ='\0' 
复制用strlcpy();
```

## 结构体

```C
初始化用memset()
typedef和#define的区别:typedef只是起别名，#define是在预编译阶段进行替换
struct默认public

using和#define都可以用来创建别名或宏定义，但两者之间有几个主要的区别：
1.作用域：using关键字创建的别名具有作用域，它们只在定义它们的命名空间或块级作用域内可见。
而#define创建的宏定义没有作用域限制，它们在整个文件或项目中都可见。
2.类型安全：using关键字是类型安全的，可以为类型创建别名，并且会进行静态类型检查。
这意味着编译器会确保使用该别名时遵循正确的类型规则
。而#define定义的宏是文本替换，不进行类型检查，可能会导致错误或难以发现的问题。
3.可读性和维护性：由于using关键字创建的别名具有明确的语法和语义，因此使代码更易读、易理解和易维护。
相比之下，#define宏定义是简单的文本替换，可能会导致代码的可读性下降，尤其在宏定义较复杂时。
总体而言，using关键字更适合用于创建类型别名，并且能提供更好的类型安全性和代码可读性。
4.替换时间：
c语言的#define是在预处理阶段替换的，c++的using是在编译阶段替换的
而#define宏定义更适合用于简单的文本替换和宏操作。
在C++中，推荐使用using关键字来定义别名，特别是在引入类型的时候，以便更好地利用C++强大的类型系统。

结构体：
auth{10,20,50}可以使用这样进行初始化，auth是一个结构体
通常情况下，C++中的结构体和C语言中的结构体具有相同的大小和布局，即结构体的大小等于结构体成员大小的和。
然而，在C++中，结构体还可以包含成员函数、继承其他结构体或类等特性，这些是C语言中的结构体所没有的。
因此，当C++中的结构体涉及到继承、虚函数等情况时，其大小可能会受到编译器的影响而有所变化。
```

## 多语言编译

```C
extern("c")
```