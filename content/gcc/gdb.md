# GCC(包括gcc和g++)

## 简介：

```C
GNU（compiler colleation）包括了gcc，g++，ar,nm可以编译很多语言如java，python等，可以多平台编译如x86,arm平台
编译器版本在4.8.5以下不支持c++11新特性
```

## 工作流程

```C
.c预编译.i->.i编译.s->.s汇编.o->.o链接
参数： E S C
名称：C I S O
gcc参数：
-o起别名
-l指定include包含文件的搜索路径(-lpthread)
-g编译的时候加入调试信息
-wall生成警告信息
-std:指定语言标准，linux通常是gnu89,windows通常是c89和c99
    g++ -o test test.cpp -std=c++11 -lpthread
 预编译：展开头文件，宏替换，去掉注释
     makefile编译：make工具依赖编译规则文件makefile
```

## 静态库和动态库（本质是对知识产权的保护）

```C
静态库：
命名规范：libxxx.a
生成静态库：
1.不需要链接，直接进行汇编 gcc *.c -c
2.使用ar包惊醒打爆，需要三个参数csr：s是索引，r是要插入的模块，
    命令：at rcs 静态库名字(libxxx.a) 原材料(*.o)
    gcc -c add.c -l ./iniclude/
```

```C
动态库：（Linux：libxxx.so;Windows:libxxx.dll）
执行权限：静态库无执行权限，动态链接器ld（负责找动态库）属于操作系统
动态链接器执行步骤：
    1.找可执行文件内部的DT_PATH部分
    2.找系统的环境变量 LD_LIBRABY_PATH
    3.找/etc/ld.so.cache
    4.找/lib/或/usr/lib
    5.查看执行程序能否找到动态库的名字:ldd 可执行文件名字

制作动态库过程：
    1.在编译家 -fpic(生成的文件为和位置无关的程序)，打包的时候加 -shared
    gcc -c -fpic add.c -l ./include/
    gcc -shared *.o libcalc.so

静态库放到代码区，动态库放到动态库加载，不放入代码区；
静态库绝对地址，动态是相对地址
```

Makefile(make识别make文件里的规则编译，实现自动化编译)

```C
规则=命令(command)+依赖（depend）+目标(target)
target:depend
    command
伪目标：执行了，但是目标不生成，依赖也可以不存在
hello:hello.o
    g++ -o hello hello.c
hello.o:hello.c
    gcc hello.c -c
变量：
    1.自定义变量：abc=123 取值$(abc)
    2.预定义变量(makefile自带的变量):CC（用于c）,CXX(用于c++)，CFLAGS(c语言编译选项)
    3.自动变量
取值方式相同$()且只能在规则的命令中使用

#define daemon makefile
daemon_libs:=-lpthread -lsystem -licoctl
daemon_src:=\
    $(subdirectory)/iecmrp_ccb.c\
    $(subdirectory)/iecmrp_db.c
ifeq($(DEV),mpu)
daemon_src:=\
    $(subdirectory)/iecmrp_fsm.c\
    $(subdirectory)/iecmrp_hs.c
endif
ifeq($(DEV),lpu)
daemon_src:=\
    $(subdirectory)/iecmrp_sm.c\
    $(subdirectory)/iecmrp_s.c
endif
$(eval $(call[make - program,iecmrpd，$(daemon),$(daemon_lib)))

1.eval:对后边的文本进行解析，对一层调用的变量进行展开
2.wildcard：获取目录下所有文件名字
3.call:函数实现对自己定义函数的引用 call 变量，参数，参数...
4.:=与=的区别：B=A会将变量赋值给A，A的变化B随时变化，：=是直接复制，A的变化不会引起B的变化
patsubst <pattern>,<replacement>,<text>按照指定的模式替换文件后缀
```

```C++
关闭gcc省略构造函数的选项
g++ -std=c++17 -fno-elide-constructors  test2.cpp  -o test2  
```

### CMake

```C++
1.Make 工具：
Make 是一个在类 Unix 系统中常用的构建工具，用于自动化构建过程。它通过读取一个名为 Makefile 的文件，根据其中定义的规则来决定哪些文件需要重新编译，并且如何重新编译。
Make 工具主要负责管理和执行 Makefile 文件中的命令和规则，用于构建项目和处理依赖关系。
2.CMake 工具：
CMake 是一个跨平台的构建工具，其作用是生成适用于不同构建系统（如 Makefile、Visual Studio 等）的构建脚本。CMake 通过读取项目根目录下的 CMakeLists.txt 文件来生成相应的构建系统所需的构建文件。
CMake 工具主要用于帮助开发者管理和配置跨平台项目的构建过程，提供了更高级、更灵活的构建能力，使得项目能够在不同平台和开发环境中进行构建。
3.关系：虽然 Make 和 CMake 都是构建工具，但它们发挥的作用不同。
Make 专注于管理 Makefile 中的规则和命令来实现项目的构建，而 CMake 则负责生成 适用于不同构建系统的 构建脚本，简化了项目的管理和配置过程。
在实际开发中，通常会使用 CMake 来生成 Makefile，并利用 Make 工具执行 Makefile 中的构建规则，从而实现项目的构建。因此，虽然 Make 和 CMake 不是同一个工具，但它们可以结合使用来更好地管理项目的构建过程。
```

### CMakeLists(可以根据各个平台生成编译文件，linux平台就是CMake)

![](https://u0b8ml0m3m.feishu.cn/space/api/box/stream/download/asynccode/?code=NWRiODA3NTFjOTRlNzliZWQ0ZGZhYzQwNDhhZTYxNGRfa0N2MWw3Z0tPVkZKNktmYUhKSUNLRGxxSjIxVlhFUnZfVG9rZW46Q21BTmJ2Y095b0FjQUN4U2tzUGNQYzZVbmVnXzE3NTk5NjU3MTM6MTc1OTk2OTMxM19WNA)

  

```C++
小写命令是首选
生成静态库.lib   add_library(test STATIC test.c)

判断x86和arm架构
if (${BUILDTYPE} STREQUAL "aarch64")
    link_directories(${PROJECT_SOURCE_DIR}/lib/arm)
    link_directories(${PROJECT_SOURCE_DIR}/lib/arm/release)
else(${BUILDTYPE} STREQUAL "x86_64")
    link_directories(${PROJECT_SOURCE_DIR}/lib/x86)
    link_directories(${PROJECT_SOURCE_DIR}/lib/x86/release)

段代码的逻辑如下：
如果 ${BUILDTYPE} 的值等于 "aarch64"，则将链接库的路径设置为 ${PROJECT_SOURCE_DIR}/lib/arm 和 ${PROJECT_SOURCE_DIR}/lib/arm/release。
如果 ${BUILDTYPE} 的值不等于 "aarch64"（即为其它值，假设为 "x86_64"），则将链接库的路径设置为 ${PROJECT_SOURCE_DIR}/lib/x86 和 ${PROJECT_SOURCE_DIR}/lib/x86/release。
在 CMake 中，link_directories 函数用于添加要搜索的库文件目录，以便在链接阶段查找动态链接库。${PROJECT_SOURCE_DIR} 是 CMake 变量，表示项目的根目录。

模板
制定cmake最小版本
1.cmake_minimum_required(VERSION 3.5)
指定工程名字
2.project(hello_world)
生成可执行文件
add_exectuable(hello hello.c)

常用命令：
message("1111"+${CMAKE_CURRENT_SOURCE_DIR})
1111+/home/cjl/Desktop/code/0.cjl
```

```Python
要将多个部分（包括外部动态库和自己编写的文件）组合成一个整体项目，需要在CMakeLists.txt文件中逐层配置。以下是一种可能的方法：
创建顶层CMakeLists.txt文件：在项目的根目录下创建一个CMakeLists.txt文件，用于指导整个项目的构建过程。
添加子目录：在顶层CMakeLists.txt文件中，使用add_subdirectory()命令来添加每个部分的子目录。比如，如果您的项目有4个部分，可以添加4个子目录：
cmake
add_subdirectory(part1)
add_subdirectory(part2)
add_subdirectory(part3)
add_subdirectory(part4)
在每个子目录中创建CMakeLists.txt文件：在每个部分的子目录下创建一个对应的CMakeLists.txt文件，用于配置该部分的构建规则。
配置动态库：如果某个部分是由别人给出的动态库，您可以使用find_library()或find_package()/add_library()命令来查找和链接该动态库。在对应的CMakeLists.txt文件中进行配置。
配置自己编写的文件：对于您自己编写的文件（比如log.cpp），在相应的CMakeLists.txt文件中添加源文件并将其编译为一个库或可执行文件。
将部分整合成整体：在顶层CMakeLists.txt文件中，可以通过target_link_libraries()命令将不同部分连接到一起。这样可以确保整个项目能够正确链接和构建。
生成构建系统：最后，在项目根目录下运行cmake命令来生成构建系统文件，并使用make或其他构建工具来构建整个项目。
```

```C++
在 CMake 脚本语言中，list 是一个用于操作列表变量的指令或函数。下面是 list 功能的一些常见用途：
创建列表变量：通过使用 set 命令或其他方式创建一个空的列表变量，并使用 list 命令对其进行操作。
向列表添加元素：可以使用 list(APPEND LIST_VAR item1 item2 item3 ...) 命令向列表变量中添加一个或多个元素。
获取列表元素：可以使用 list(GET LIST_VAR index OUTPUT_VARIABLE) 命令获取列表变量中特定索引位置的元素，并将其存储在另一个变量中。
移除列表元素：可以使用 list(REMOVE_ITEM LIST_VAR item1 item2 ...) 命令从列表中移除指定的元素。
获取列表长度：使用 list(LENGTH LIST_VAR OUTPUT_VARIABLE) 命令可以获取列表变量的长度，并将其存储在另一个变量中。
遍历列表元素：可以结合循环语句，如 foreach 或 while，来遍历列表中的所有元素。
```

## GDB

```C
gdb普通cpp文件：
g++ -g test.cpp -o test
gdb test
b 21
run
```

```C++
GDB：
1.n ->next下一行 
2.s  ->进入函数
3. u跳转到某一行 
4.call（）在GDB中调用函数 
5.finish跳出函数 
6.display 变量名
7.设置变量名 set var 变量名 =值
8.watch 变量名 -> 跟踪变量的变化
打GDB的思路：知道大体功能，一直查看返回值


调试常见命令网站
https://bbs.kanxue.com/thread-272300.htm
```

## Perf

```C
linux中性能分析工具
常见的命令：生成火焰图
```

## 移植

```Shell
嵌入式中，如果要使用linux，必须对linux进行剪裁，因为嵌入式资源有限
```

## 编程规范

## epoll

```C
epoll 本身不缓冲数据，它只是告诉你 哪个文件描述符需要处理，具体的数据仍然需要从文件描述符（如 socket）中读取

发展史：select(阻塞I/O)->poll(非阻塞I/O)->epoll->信号I/O
poll：用户从内核区拷贝到进程区的时候会阻塞，但是采用回调函数机制，处理好了就通知应用进程
select/poll效率和并发量要求不高，使用的是数组和链表
epoll（三个函数和两个数据结构）效率和开发量要求高，不支持跨平台，只支持linux，使用的是双链表和红黑树，触发事件后单独放到就绪链表中，只遍历就绪链表
epoll_create()：创建红黑树和就绪队列
epoll_ctl():负责在红黑树中增加/删除文件句柄fd
epoll_wait():阻塞等待事件发生函数，返回事件发生的数目，将事件写入events数组当中

事件触发：水平触发（LT）/边缘触发(ET)
LT:epoll只要事件发生了，每一次都通知，处理了才算结束；如果文件描述符的事件（如可读或可写）未被处理完毕，epoll 会不断地通知你，直到你处理完成为止。
ET:触发一次通知一次，不处理下次就不通知了，除非事件发生变化
假设一个文件描述符可读：
如果文件描述符变为可读，epoll 会通知你一次。
如果你没有完全读取数据，而该文件描述符再次可读，epoll 不会再次通知你，除非文件描述符的状态再次发生变化（如从不可读变为可读）
```

## Leetcode

```C++
位计数（"汉明重量"算法）
auto kret = [](int num){
            int cnt = 0;
            while(num){
                cnt += num%2;
                num/=2;
            }
            return cnt;
        };
 使用：kret(num)：即可获得整数num中有几个1
```

```C++


void moveZeroes(std::vector<int>& nums)
{
    int j         = 0;
    int i         = 0;
    int count     = 0;
    int numlength = 0;
    for (; i < nums.size(); i++)
    {
        if (nums[j] == 0)
        {
            count++;
            j++;
        }
        else
        {
            nums[i] = nums[j];
            i++;
            j++;
            count++;
            numlength++;
        }
    }
    int div = count - numlength;
    int k   = numlength - 1;
    while (--div)
    {
        nums[k] = 0;
        k++;
    }
}
```

## Git

```C++
1.git submodule update --init 是一个 Git 命令，用于初始化并更新 Git 仓库中的子模块。
具体来说，当一个 Git 仓库中包含子模块（即另一个 Git 仓库），在克隆主仓库时，子模块的内容并不会被自动拉取下来，需要使用 git submodule update --init 命令来初始化和更新子模块。
解释一下这个命令的含义：
git submodule ：Git 的子模块命令，用于管理子模块。
update ：子命令，表示更新子模块。
--init ：选项，表示初始化子模块。如果某个子模块尚未初始化，则会被初始化。初始化子模块意味着 Git 会将存储在主仓库中的子模块信息注册到本地的 .gitmodules 文件中。
因此，git submodule update --init 的作用是初始化子模块（如果尚未初始化）并更新子模块的内容。它会按照主仓库中指定的子模块配置，从远程仓库中拉取子模块的代码到本地。

2.git权限问题
manager拥有全部权限，gitlab授权Devloper才可以提交代码，Repoter只可以拉取代码。guest
3.git拉分支和提交分支
git branch -avv查看个分支和提交情况
git commit -m "xxx"(记得把生成的编译文件删除，build和install)
git push

git clone b <branch_name> <repository_url> 如果不加-b默认master
```