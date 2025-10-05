```C++
#include <iomanip> 是 C++ 标准库中的一个头文件，用于控制输入输出流的格式化。主要包含了各种操纵符（manipulators），用于设置和调整输出流中数据的格式、精度、对齐方式等。
下面是一个简单的例子，演示如何使用 <iomanip> 头文件中的一些操纵符来格式化输出：
cpp
#include <iostream>#include <iomanip>int main() {
    double value = 3.14159;

    // 设置小数点后显示2位
    std::cout << std::fixed << std::setprecision(2);
    std::cout << "Value with 2 decimal places: " << value << std::endl;

    // 将数值用科学计数法表示
    std::cout << std::scientific;
    std::cout << "Value in scientific notation: " << value << std::endl;

    // 设置输出宽度为10，并右对齐
    std::cout << std::right << std::setw(10) << "Hello" << std::endl;

    return 0;
}
```