## `include <chrono>`
```C++
时间类std::chrono
std::ratio（用于自定义时间）是C++标准库中的一个类模板，用于表示一个有理数比例。它基于编译时的常量表达式，
并提供了一种可以在编译时进行精确计算的方式。
std::ratio模板接受两个整数作为模板参数，分别表示有理数比例的分子和分母。
例如，std::ratio<1, 2>表示比例为1/2，std::ratio<3, 5>表示比例为3/5，依此类推。
std::ratio类提供了一些常用的成员类型和成员函数，
例如num和den成员类型分别表示分子和分母的值。
它还支持一些运算符重载，如加法、减法、乘法和除法，
这使得可以在编译时对比例进行各种数学操作。
std::ratio常用于C++标准库中的时间库（例如std::chrono），
用于表示时间的单位和分辨率。通过使用std::ratio，
可以在编译时确定时间单位的精度，
从而在不同系统和平台上实现时间操作的一致性。
using half_seconds = std::chrono::duration<double, std::ratio<1, 2>>;
Rep 是 double，表示时间刻度的类型是双精度浮点数。
Period 是 std::ratio<1, 2>，表示每个时间刻度代表0.5秒

std::chrono::duration<int> twenty_seconds(20);twenty_seconds是20秒
std::chrono::duration<double, std::ratio<60>> half_a_minute(0.5);half_a_minute是半分钟
std::chrono::duration<long, std::ratio<1,1000>> one_millisecond(1);one_millisecond是1毫秒
std::chrono::steady::milliseconds是系统给提供的
std::chrono::milliseconds ms(1000);
std::chrono::hours
std::chrono::minutes
std::chrono::seconds
std::chrono::milliseconds
std::chrono::microseconds
std::chrono::nanoseconds

std::chrono::_V2::steady_clock 
是 C++ 标准库中的一个时钟类，它提供了与系统时钟无关的稳定时间点和时间间隔的功能。
在 C++11 标准中，std::chrono::steady_clock 
被引入作为一个高精度、不可调整的时钟。
然而，在某些 C++ 实现中，它可能被定义为 std::chrono::_V2::steady_clock（或其他类似名称），
以标识其为 C++20 中提出的新版本。
std::chrono::_V2::steady_clock 的特点是：
它是一个单调递增的时钟，保证其返回的时间点不会退后。
它提供了较高的时钟精度，通常比 std::chrono::system_clock 更准确。
它不受系统时间调整的影响，因此适用于需要稳定时间间隔测量的应用程序。
您可以使用 std::chrono::_V2::steady_clock 来测量经过的时间、
计算时间间隔以及进行更精确的时间相关操作。
例如，您可以使用它来实现性能计时、定时任务等。
时间转换：
std::chrono::milliseconds ms(1000);
std::chrono::seconds sec = std::chrono::duration_cast<std::chrono::seconds>(ms);
```
|   |   |   |
|---|---|---|
|**类名**|**描述**|**对应的心理学概念**|
|std::chrono::system_clock|系统的实际时间，可能会受到系统时间调整的影响|外部环境对人的影响|
|std::chrono::steady_clock|稳定的时钟，时间从不调整|稳定、可靠的心理状态|
|std::chrono::high_resolution_clock|提供最小的可表示的时间间隔|细微的心理变化|
|std::chrono::time_point|表示特定的时间点|特定的记忆|
|std::chrono::duration|表示时间的长度|时间的感知|