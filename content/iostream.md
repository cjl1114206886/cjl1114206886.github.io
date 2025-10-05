```C++
std::cout打印uint8_t(unsigned char)会把它当作字符操作，而字符又以来ascii表，当值很小的时候输出的是控制字符，但是控制字符不显示
解决方法:用static_cast<int>(aa)输出，不可以用reinterpret<int>(aa)输出，因为static_cast会改变底层二进制(对于unsigned使用0扩展)，但是reinterpret不会改变底层二进制
    uint8_t aa = 0;
    while (aa < (u_int8_t)255)
    {
        //根据ascii表只输出49(1)到126(~)
        std::cout << aa << " ";
        aa++;
    }
```
```C++
头文件<fstream>中的flush和头文件<ostream>std::flush都是用来刷新输出缓冲区的函数，但是它们的使用场景有些不同。
头文件<fstream>中的flush是ofstream类中的一个成员函数，用于强制将输出缓冲区中的内容写入文件中，确保数据被实际写入磁盘，而不是仅停留在内存中等待写入。
而头文件<ostream>std::flush是iostream类中的一个操作符重载函数，用于强制将输出缓冲区中的内容立即输出到设备（如控制台或文件），并清空缓冲区。
因此，如果您想将文件中的数据刷新到磁盘上，应使用头文件<fstream>中的flush；如果您要在控制台或文件中立即输出数据，请使用头文件<ostream>std::flush

头文件<iostream>中std::endl;std::fulsh;std::ends区别
ends函数 终止字符串
flush函数 刷新缓冲区
endl函数 终止一行并刷新缓冲区
flush() 是把缓冲区的数据强行输出, 主要用在IO中，即清空缓冲区数据，
一般在读写流(stream)的时候，数据是先被读到了内存中，再把数据写到文件中，
当你数据读完的时候不代表你的数据已经写完了，因为还有一部分有可能会留在内存这个缓冲区中。
这时候如果你调用了close()方法关闭了读写流，那么这部分数据就会丢失，所以应该在关闭读写流之前先flush()。
```
```C++
 #include <unordered_map>
unordered_map:c++11#include <unordered_map>
```
```text
写入必须写序列化之前的数据，序列化->结构体->json。当为结构体直接写入是不可以的
原因需要研究一下

tellg()函数返回一个streampos类型的值，表示当前的读取位置在输入流中的偏移量。
通常情况下，tellg()函数结合seekg()函数一起使用，可以用于保存和恢复输入流的读取位置。
通过调用tellg()函数获取当前位置，然后再调用seekg()函数将读取位置设置回原先的位置。
int file_size = input.tellg();: 这行代码获取当前的读取位置，即文件的大小（以字节为单位），并将其存储在变量 file_size 中。

std::ios::beg是ios类的一个标志位（flag），通常用于指定文件定位操作的起始位置。
具体来说，std::ios::beg表示从文件的起始位置开始进行定位操作。
在输入/输出操作中，可以使用seekg()和seekp()函数来进行文件定位操作。通过指定std::ios::beg作为定位模式，可以确保定位操作从文件的起始位置开始。

input.seekg(0, std::ios::beg);中的第一个参数0表示相对于起始位置的偏移量，而std::ios::beg则表示将偏移量计算相对于文件的起始位置。因此，调用这个语句后，输入流的读取位置将被设置为文件的开头位置。

input.read(&file_data[0], file_size);中的第一个参数&file_data[0]表示要写入数据的目标缓冲区的起始位置，通常用于C++中的字符数组或字符串。第二个参数file_size表示要从输入流中读取的字节数量。
```
```C++
std::fixed是C++标准库中的一个操纵符，用于设置浮点数的输出格式为固定小数点表示法。在使用std::fixed之后，浮点数将以固定的小数点形式输出，不会使用科学计数法表示。
```