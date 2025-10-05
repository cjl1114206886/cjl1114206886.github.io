```C++
fcntl（file control）函数是一个用于对文件描述符进行各种控制操作的系统调用，在 POSIX 系统中广泛使用。它允许程序员执行各种文件控制操作，例如设置文件描述符的属性、获取文件描述符的状态等。
下面是 fcntl 函数的基本原型和常用操作：
cpp
#include <unistd.h>
#include <fcntl.h>
int fcntl(int fd, int cmd, ... /* arg */);
fd 是要进行操作的文件描述符。
cmd 是要执行的操作类型，可以是以下之一：
F_DUPFD：复制文件描述符。
F_GETFL：获取文件状态标志（文件打开方式）。
F_SETFL：设置文件状态标志。
F_GETFD：获取文件描述符标志。
F_SETFD：设置文件描述符标志。
其他操作还包括对锁定、信号等的控制。
```