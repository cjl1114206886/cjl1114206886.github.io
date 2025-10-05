```C++
共享指针
共享指针
shared_ptr 是 C++ 中的智能指针类，用于管理动态分配的对象。
它是 C++11 引入的标准库中的一部分，并位于 <memory> 头文件中。
shared_ptr 可以在多个指针之间共享对象所有权，
它使用引用计数的方式来跟踪共享指针的数量。每当创建 shared_ptr  并将其指向一个对象时，
引用计数会增加。当 shared_ptr 被销毁或重置时，引用计数会减少。当引用计数为零时，内存将被自动释放。
以下是 shared_ptr 的一些常见操作：
1.创建 shared_ptr 指向对象：
std::shared_ptr<T> ptr = std::make_shared<T>(args);
访问 shared_ptr 指向的对象：
2.T value = *ptr;  // 解引用 shared_ptr 获取对象
ptr->method();  // 使用箭头运算符访问对象成员函数
共享 shared_ptr 的所有权：
3.std::shared_ptr<T> ptr1 = std::make_shared<T>();
std::shared_ptr<T> ptr2 = ptr1;  // 共享 ptr1 的所有权
4.重置 shared_ptr 指向其他对象或空值：
ptr.reset();        // 重置为 nullptr，释放对象占用的内存
ptr.reset(new T()); // 重置为指向新对象
5.获取 shared_ptr 的引用计数：
std::size_t count = ptr.use_count();  // 获取引用计数

shared_ptr 可以有效地消除动态资源的内存泄漏和悬空指针问题，
并提供了一种简单和安全的方式来管理对象的生命周期。
然而，需要注意避免循环引用，由于 shared_ptr 的引用计数机制，循环引用会导致对象无法被释放，可能
引发内存泄漏。

std::unique_ptr独占指针；超出作用域的时候，内存自动释放；
    通常用std::make_unique来创建，用get()函数来创建
    xxxxxxxxx
    std::string serialized_data;
    {
      std::unique_lock<std::mutex> lock(hor_ldm_mtx_);
      hor_ldm_->SerializeToString(&serialized_data);
    }
    xxxxxxxxxxx
    问题：为什么有一个{}：局部作用域
在给定的代码段中，大括号 { } 被用来创建一个作用域边界，这种用法称为作用域限定符或局部作用域。在这种情况下，大括号内的代码块形成了一个局部作用域，在该作用域内定义的变量 std::string serialized_data 只在该作用域内有效，并在离开该作用域时被销毁。
具体来说，大括号内的代码块的作用是在获得互斥量 hor_ldm_mtx_ 的锁的同时对 hor_ldm_ 数据对象进行序列化操作，并将序列化后的数据存储到 serialized_data 中。一旦程序执行完大括号内的操作并离开该作用域，互斥量的锁就会自动释放，从而确保互斥量的安全使用。
这种技术常用于需要临时的作用域，例如在多线程环境中使用互斥量来保护临界区，确保只有一个线程能够访问共享资源。通过在大括号内创建局部作用域，可以控制变量的生命周期，从而减少不必要的资源占用和潜在的错误。
总之，在这种情况下，大括号内的代码块帮助确保使用互斥量对共享资源进行安全操作，并在正确的时机释放锁，避免死锁等问题。

    
std::shared_ptr共享指针，和weak_ptr一块用，用来消除循环引用问题
std::weak_ptr                        
```
```C++
怎么避免内存泄漏：
1.new和delete需要搭配使用;c++11之后，加入一个nullptr可以在使用内存之后再检查一遍
2.父类的析构函数用纯虚函数，派生类用虚析构函数，自己释放堆区    
3.智能指针：c++11当中，使得程序员不需要自己释放指针；会自动分配内存；解决独占/共享所有权指针的释放，传输
```