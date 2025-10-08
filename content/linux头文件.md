[进程调度](进程调度)
## 源码

```C++
https://www.kernel.org/
https://mirrors.edge.kernel.org/pub/linux/kernel/v6.x/
```
[linux文件系统](linux文件系统.md)

[linx日志](linux日志)

[命令解析器（bash和shell）](命令解析器（bash和shell）)

[进程间调度IPC](进程间调度IPC)

[gcc/gdb](gcc/gdb)

##  `#include <errno.h>`
```



```C
linux某些系统调用错误和库函数错误都存储在这里边，errno由操作系统维护，存储最近依次发生的错误(下一次错误会覆盖上一次错误)，通常与printf("%s",strerror(errno));
```

### `#include <unistd>`

```C
unix标准库
alarm()以秒为单位的计时器，单位和sleep相同
```

```C++
access()函数
access() 函数是用来检查文件是否具有某种权限（读、写、执行）或者文件是否存在的函数。
函数原型：
1.检查文件权限
int access(const char *pathname, int mode);
    参数，pathname 是要检查的文件路径名，mode 是要检查的权限模式（R_OK 表示可读，W_OK 表示可写，
    X_OK 表示可执行，F_OK 表示文件存在）
    返回值：函数返回值为 0 表示成功，-1 表示失败并设置 errno 错误码来指示具体错误。
    常见的 errno 错误码包括：
        EACCES：无法访问文件，可能是因为权限不足。
        ENOENT：指定的文件或路径不存在。
        EINVAL：无效的参数，比如 mode 不是合法的权限模式。
        
2.检查是否存在
    access(pathname,0)==0表示只需检查文件是否存在，不需要检查权限
#include <unistd.h>
#include <iostream>
int main() {
    const char* filename = "test.txt";
    int result = access(filename, R_OK | W_OK);
    
    if(result == 0) {
        std::cout << "File has read and write permissions" << std::endl;
    } else {
        std::cerr << "Cannot access file: " << filename << std::endl;
    }
    
    return 0;
}
在上述示例中，access() 函数检查指定的文件 "test.txt" 是否具有读写权限。
如果具有读写权限，则输出相应的消息；否则，输出错误信息。
请注意，使用 access() 函数只是检查是否具有某种权限，它并不执行实际的读写操作。
因此，仍然需要在实际读写文件之前检查返回值并处理可能的错误。
```

### `#include <sys/stat.h>`

```C++

习惯上将 C语言 的头文件使用 ".h" 扩展名，而将 C++ 的头文件使用
 ".hpp" 扩展名。这种习惯性的命名约定有助于在项目中更清晰地区分 C 代码和 C++ 代码。
 
 头文件加入顺序：
系统头文件
c++头文件
自定义头文件
 
<sys/stat.h> 是 C/C++ 标准库中的一个头文件，它包含了一些用于文件状态和属性获取的函数和数据类型的定义。
该头文件提供了以下函数和数据类型的声明：
int chmod(const char *path, mode_t mode)：修改文件的权限。
int mkdir(const char *path, mode_t mode)：创建一个目录。
int stat(const char *path, struct stat *buf)：获取文件状态信息，并将结果存储在 struct stat 结构体中。
int lstat(const char *path, struct stat *buf)：类似于 stat 函数，但是对于符号链接，会返回链接本身的信息，而不是链接指向的文件的信息。
int fstat(int fd, struct stat *buf)：获取文件描述符所对应文件的状态信息。
mode_t S_ISDIR(mode_t mode)：判断给定的文件模式是否表示一个目录。
mode_t S_ISREG(mode_t mode)：判断给定的文件模式是否表示一个普通文件。
mode_t S_ISLNK(mode_t mode)：判断给定的文件模式是否表示一个符号链接。
此外，<sys/stat.h> 头文件还定义了 struct stat 结构体，其中包含了与文件相关的各种状态和属性，
如文件类型、文件大小、访问权限等。
下面是一个示例代码，展示如何使用 <sys/stat.h> 头文件中的一些函数和结构体：
c
#include <stdio.h>
#include <sys/stat.h>
int main() {
    const char* file_path = "example.txt";
    struct stat file_stat;
    if (stat(file_path, &file_stat) == 0) {
        printf("File size: %lld bytes", file_stat.st_size);
        printf("File permissions: %o", file_stat.st_mode & 0777);
        if (S_ISDIR(file_stat.st_mode)) {
            printf("This is a directory.\n");
        } else if (S_ISREG(file_stat.st_mode)) {
            printf("This is a regular file.\n");
        } else if (S_ISLNK(file_stat.st_mode)) {
            printf("This is a symbolic link.\n");
        }
        } else {
            printf("Failed to get file information.\n");
        }
    
    return 0;
}
上述示例中，我们使用 stat 函数获取了一个名为 example.txt 的文件的状态信息，
并打印了文件的大小和权限。然后，通过 S_ISDIR、S_ISREG 和 S_ISLNK 宏判断文件的类型，并相应地进行输出。
```