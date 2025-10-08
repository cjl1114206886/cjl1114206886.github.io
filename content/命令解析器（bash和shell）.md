```C++
指令->命令解析器
echo $PATH ，如果让一个命令全局执行，就将他加入PATH路径

在 shell 脚本中，&& 是逻辑与操作符，用于连接两个命令，
并且只有前一个命令成功执行（返回 0）时，才会执行后一个命令。因此，&& 是具有“先后执行”的关系。
在 shell 脚本中，> 是重定向符号，用于将命令的输出重定向到指定的文件或设备中。
具体来说，command > file 表示将 command 命令的标准输出（stdout）输出到名为 file 的文件中，
如果文件不存在，则会创建该文件；如果文件已经存在，则会覆盖原有内容。

`ls -rt`命令用于按时间顺序列出文件和目录，并以最旧的文件或目录排在前面，最新的排在最后。下面是`ls -rt`中各个参数的解释：

- `-r`: 反转排序顺序，即将最新的文件或目录排在前面，最旧的排在最后。
- `-t`: 按修改时间排序，将最新修改的文件或目录排在前面，最旧的排在最后。

综合起来，`ls -rt`命令会按照文件或目录的修改时间从旧到新进行排序，将最旧的文件或目录排在前面，最新的排在最后。

外接执行shell脚本：check_log_size.cpp
```
![[Shell.mm]]
## 命令提示器

```C
用户名@主机名：用户所在目录
~表示用户根目录
$代表普通用户
#代表超级用户
root@iZ2ze84zwrzp1mlr3amtaoZ:~/Desktop/code/cloud-pan#
表示超级用户且在根目录的/Desktop/code/cloudpan下
```

## 命令解析器（bash和shell）

[Shell](https://u0b8ml0m3m.feishu.cn/mindnotes/MRxubFtH9mC2uon5yG9cTOH3nwd)

```C++
指令->命令解析器
echo $PATH ，如果让一个命令全局执行，就将他加入PATH路径

在 shell 脚本中，&& 是逻辑与操作符，用于连接两个命令，
并且只有前一个命令成功执行（返回 0）时，才会执行后一个命令。因此，&& 是具有“先后执行”的关系。
在 shell 脚本中，> 是重定向符号，用于将命令的输出重定向到指定的文件或设备中。
具体来说，command > file 表示将 command 命令的标准输出（stdout）输出到名为 file 的文件中，
如果文件不存在，则会创建该文件；如果文件已经存在，则会覆盖原有内容。

`ls -rt`命令用于按时间顺序列出文件和目录，并以最旧的文件或目录排在前面，最新的排在最后。下面是`ls -rt`中各个参数的解释：

- `-r`: 反转排序顺序，即将最新的文件或目录排在前面，最旧的排在最后。
- `-t`: 按修改时间排序，将最新修改的文件或目录排在前面，最旧的排在最后。

综合起来，`ls -rt`命令会按照文件或目录的修改时间从旧到新进行排序，将最旧的文件或目录排在前面，最新的排在最后。

外接执行shell脚本：check_log_size.cpp
```

## 文件操作(整理到这里1004)

```C
fopen()打开文件，返回一个文件句柄
fseek();按照索引在文件中查询
fget();读取一个字符
fclose();关闭一个字符

int main(int argc, char const *argv[])
{
    file *f =fopen("filename.c","w");
    if (NULL == f)
    {
        exit(0);
    }
    for (int i = 0; i < 20; i++)
    {
        fputs("hello");
    }
    fclose(f);
    f= fopen("filename.c","c");
    fseek(f,1,SEEK_SET);
    char c = fgetc(f);
    fclose(f);
    return 0;
}
```

## 信号

```Shell
在 Linux 和类 Unix 操作系统中，有许多不同类型的信号可以用于进程间通信和控制。以下是一些常见的 Linux 信号及其含义：
SIGINT（2）：由终端发送给前台进程组，通常由用户按下 Ctrl + C 触发，用于请求进程终止。
SIGQUIT（3）：由终端发送给前台进程组，通常由用户按下 Ctrl + \ 触发，用于请求进程终止并生成 core dump 文件。
SIGKILL（9）：由系统管理员用来强制终止进程，进程无法捕获或忽略该信号。
SIGTERM（15）：请进程优雅地终止，允许进程清理工作后退出。
SIGHUP（1）：当终端连接断开时发送给服务进程，通常用于重启服务。
SIGALRM（14）：定时器信号，用于响应定时器事件。
SIGUSR1（30）和 SIGUSR2（31）：由用户定义的信号，可用于自定义进程间通信。
SIGCHLD（17）：子进程状态发生变化时（终止或停止）向父进程发送的信号。
SIGPIPE（13）：当往一个已经关闭写段的管道或 Socket 写数据时，将会收到该信号。
SIGSEGV :操作系统引用了一个内容未空的内存
除了这些常见的信号外，还有其他几十种不同的信号，每种信号都有特定的含义和用途。您可以使用 kill -l 命令在终端上查看系统支持的所有信号及其对应的编号。
```

## SOCKET

```C
客户端：socket->connect->send/recv
服务端：socket->bind->listen->accept->send/recv
int socket(int domain, int type, int protocol);
htons的头文件为<arpa/inet.h>
```

![](https://u0b8ml0m3m.feishu.cn/space/api/box/stream/download/asynccode/?code=YjgwM2VmNzI2NmJkODE4MDAxOGE5ODc0ZjlkNzBjNDVfT2xiMWNLa3E0Qko1c042Q1ZwRWhEYlozZHNtSU9sS3NfVG9rZW46QTJHUmI0YWpEb2NDT2J4ME0zZmNESFo3bmxkXzE3NTk5NjU1ODY6MTc1OTk2OTE4Nl9WNA)

## Vim

```C
dd:直接删除一行
:set number显示行数
vim搜索：/关键字 ->回车   再n跳转到下一个

过滤系统日志：
grep "error" /var/log/syslog > /path/to/newfolder/errors.syslog

查看总行署：:echo line('$')

vim同时搜索两个   :/44719\&AA_Broker_M

修改vim乱码

cat ~/.vimrc
set number
let &termencoding=&encoding
set fileencodings=utf-8
```

  

## 常见命令

```Plain
tmux插件
插件的使用tmux使用手册:
sudo apt-get update
sudo apt-get install tmux
1.Ctrl+b当于开启tmux的快捷键
2.Ctrl + b, shift+% (按下快捷键 Ctrl + b，然后输入 %):分屏
3.切换到上下左右的窗格：
ctrl+b ，上/下/左/右箭头键
4.创建同一个会话的一个新窗口：
ctrl +b,c
5.创建一个新会话
tmux new-session
6.退出会话:
exit/ctrl+d
7.移动窗格
按住ctrl+b,再点击->/<-



tar -zxf 是一个常用的命令，用于解压缩.tar.gz或.tgz格式的压缩文件。下面是对该命令的解析：
tar: tar 是一个用于在 Unix 和类 Unix 系统中打包和解包文件的命令行工具。它可以创建归档文件（通常称为 tarball），并支持对归档文件的解压缩操作。
-z: 选项表示使用 gzip 压缩算法对文件进行压缩。也就是说，-z 表示需要使用 gzip 解压缩压缩文件。
-x: 选项表示解压缩操作，即从归档文件中提取文件。
-f: 选项用于指定归档文件的文件名。紧随其后的参数是要操作的归档文件的名称。
tar -czvf 目的.tar.gz 源目录

prctl:获取/设置进程信息 prtctl(RR_SET_NAME,"cjl");

在 scp 命令中，-P 参数用于指定连接远程主机时所使用的端口号。
默认情况下，scp 使用 SSH 默认端口号22来连接远程主机。
如果需要连接的远程主机使用非默认端口号，则可以使用 -P 参数来指定特定的端口号。

scp从远程获取
scp -r -P 27386 nvidia@192.168.35.2:/work ./

sudo su xxx 是一个 Linux 命令，用于以特定用户（xxx）的身份启动一个新的 shell。
这个命令通常用于在系统中切换到其他用户的身份进行操作。
具体来说：
sudo 是一个用于在 Linux 系统中以超级用户（root）权限执行命令的工具。
通过使用 sudo，普通用户可以以管理员权限执行某些需要 root 权限才能执行的命令。
su 是一个切换用户身份的命令，使用 su 可以在当前 shell 中切换到其他用户的身份，并进入该用户的环境。
xxx 是要切换到的目标用户的用户名。
因此，sudo su xxx 命令的作用是以超级用户权限切换到用户 xxx 的身份，并进入该用户的 shell 环境。

如需永久显示行号，可以将以上命令添加到 Vim 配置文件中。通常情况下，
Vim 配置文件是 ~/.vimrc 文件。您可以打开该文件并在其中添加 set number 或 set nu 来永久显示行号。



1.配置ssh
ssh-keygen -t rsa -b 4096 -C "e-jianlong.chen@lotuscars.com.cn"
2.查看：cat id_rsa.pub
将key复制到网页里边的key
3.验证
ssh -T git@git.lotuscars.com.cn
 Welcome to GitLab, @e-jianlong.chen!
ssh 连接被拒绝：删除~/.ssh目录下的knownts重新连接

插件太大，避免上传到gitlab，同时又想避免上传，
放到别的目录里边，新建软连接到代码目录
ln -s /home/cjl/Downloads/thirdparty /home/cjl/Desktop/code/livekit_demo/project

软链接的主要用途包括：
创建指向共享库的软链接，以简化程序编译和运行时对共享库的引用。
创建便捷的符号链接，节省存储空间，而不必复制相同的文件。
在文件系统中轻松引用其他文件或目录，使文件之间的关联更加紧密。
ln -s 是创建软链接的命令行命令，其功能参数如下：
-s：创建软链接。如果不使用此选项，则 ln 命令将会创建硬链接（hard link）。
使用 ln -s 命令的语法为：

ln -s /path/to/original /path/to/link

readelf -d liblg_ldm_sdk.so
-------------------------------------------------------------------------------------------
解压命令
tar -zxvf 
压缩命令：
tar -cxvf 目标名.tar.gz 源目录名字

du(disk usage)命令
用于统计目录或文件占用的磁盘空间
du -h:以人类可读的方式统计
du -s统计目录大小，但是不会统计子目录

top动态查看cpu情况

chown:授权文件的所有者和所属组
chown LTS:Lts ./AA_Broker_M
chown LTS:Lts ./AA_Broker_M/* -R 

df命令：disk free查看可用空间
df -h用人类可以查看的方式显示

解压7z压缩包
7z x aaa.7z

在目录中搜索目录名称
find . -type d -iname "*STL*"


mount -o remount,rw /usr/sbin/

这个命令是Linux系统中的一个用法，用于重新挂载特定目录或分区为读写模式。具体来说，mount -o remount,rw /usr/sbin/ 这个命令的含义是将 /usr/sbin 目录重新挂载为读写模式。
mount: 是 Linux 系统中用于挂载文件系统的命令。
-o remount,rw: 这部分参数表示重新挂载，并设置为读写模式。remount 表示重新挂载，rw 表示读写模式，即可读写。
/usr/sbin: 是要重新挂载的目录或文件系统路径。
当执行这个命令时，系统会尝试重新挂载 /usr/sbin 目录为读写模式。这可能是因为该目录之前被以只读模式挂载，而现在需要对该目录进行写操作。请注意，执行这个命令需要以 root 用户或具有 sudo 权限的用户身份运行，
否则可能会提示权限不足的错误。

ls -t命令用于按照文件或目录的修改时间进行排序，最新修改的文件或目录会显示在最前面。这个命令对于查看最近修改过的文件或目录非常有用
ls -lrt

在Ubuntu这个Linux发行版中，可以通过以下步骤调整终端命令行的字体大小：
打开终端：按下 Ctrl + Alt + T 可以打开终端。
使用快捷键调整字体大小：使用以下快捷键可以调整终端字体大小：
Ctrl + Shift + = 或 Ctrl + Shift + +：放大字体大小
Ctrl + -：缩小字体大小
手动设置字体大小：如果想要手动设置字体大小，可以在终端中右击，选择“首选项”或“偏好设置”，在设置界面中选择“文本”选项卡，就可以设置字体、大小和颜色等终端显示的属性

-----------------------------
pip 是 Python 的包管理工具，用于安装 Python 包。它主要用于 Python 开发环境，并且可以安装和管理 Python 软件包。
```