```C++
#include <mutex>
std::call_once 是 C++11 中提供的函数，用于保证一个函数只被调用一次。
它通常与 std::once_flag 结合使用。
#include <iostream>
#include <mutex>

void myFunction()
{
    std::cout << "Function called" << std::endl;
}

int main()
{
    std::once_flag flag;
    std::call_once(flag, myFunction); // myFunction will be called only once

    return 0;
}
```