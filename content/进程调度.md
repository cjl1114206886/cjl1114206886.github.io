## 进程间调度（IPC）

```C
1.管道（pipe和fifo）
pipe：
int main(int argc, char const *argv[])
{
    int fd[2];
    int ret = pipe(fd);
    if (pid==-1)
    {
        cout<<error<<endl;
    }else{
        close(fd[1]);
        char buf[1024];
        ret = read(fd[0],buf,sizeof(buf));
        write(STDOUT_FILENO,buf,ret);
    }else if (pid > 0)
    {
        sleep(1);
        close(fd[0]);
        cout<<"parent pid is:"<<getpid()<<endl;
        wait(NULL);
    }
    return 0;
}

fifo:适用于无血缘关系的进程
#define P_FIFO "./p_fifo"
读进程：
if(mkfifo(P_FIFO,0777)<0)
    cout<<"make fifo file failed"<<endl;
fd=open(P_FIFO,O_RDONLY|O_NONBLOCK);
while(1){
    memset(cache,0,sizeof(cache));
    if(0==read(fd,cache,100)){
        cout<<"no data"<<endl;
    }else{
        cout<<get data:<<cahce<<endl;
    }
    sleep(1);
}

写进程:
fd=open(P_FIFO,O_WRONLY|O_NONBLOCK);
if(-1==fd){
    cout<<"open failed"<<endl;
    return 0;
}else{
    cout<<"open success"<<endl;
}
write(fd,argv[0],100);
2.信号（开销小）：主要用于事件通知，比如重启，keepalive
Linux中一共有
3.消息队列:支持进程间传输结构化消息
#include <mqueue.h>

int main() {
    // 创建队列
    mqd_t mq = mq_open("/test_queue", O_CREAT | O_RDWR, 0644, NULL);
    
    // 发送消息
    char message[] = "Hello POSIX MQ";
    mq_send(mq, message, strlen(message), 0);
    
    // 接收消息
    char buffer[256];
    mq_receive(mq, buffer, sizeof(buffer), NULL);
    printf("收到: %s\n", buffer);
    
    mq_close(mq);
    mq_unlink("/test_queue");
    return 0;
}
4.共享内存mmap
零拷贝：数据不需要在内核和用户空间间拷贝
高性能：直接内存访问，速度最快
需要同步：必须配合信号量或互斥锁使用
大容量：可以共享GB级内存
应用场景
高性能数据传输：图像、视频、音频数据
实时系统：ADView这样的自动驾驶系统
数据库系统：进程间共享数据缓存
科学计算：大矩阵、大数组计算
https://blog.csdn.net/weixin_61857742/article/details/128252280
#include 
#include 
#include 
#include 
#include 
#include 

#define SHM_SIZE 1024 // 共享内存大小
#define SHM_NAME "/myshm" // 共享内存名称

int main() {
    int fd;
    char *shmaddr;
    char s8ReadBuf[1024] = {0};
    const char *msg = "Hello, world!";

    // 创建共享内存区域
    fd = shm_open(SHM_NAME, O_CREAT | O_RDWR, 0666);
    if (fd == -1) {
        perror("shm_open");
        exit(1);
    }

    // 调整共享内存区域的大小
    if (ftruncate(fd, SHM_SIZE) == -1) {
        perror("ftruncate");
        exit(1);
    }

    // 映射共享内存区域到进程地址空间中
    shmaddr = mmap(NULL, SHM_SIZE, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    if (shmaddr == MAP_FAILED) {
        perror("mmap");
        exit(1);
    }

#if 1
    // 在共享内存中写入数据
    strncpy(shmaddr, msg, SHM_SIZE);
#else
    // 读数据
    // memcpy(s8ReadBuf, shmaddr, 1024);
    // printf("s8ReadBuf:%s\n", s8ReadBuf);
#endif
    // 解除共享内存区域与进程地址空间的映射关系
    if (munmap(shmaddr, SHM_SIZE) == -1) {
        perror("munmap");
        exit(1);
    }

    // 删除共享内存区域的文件名并释放资源
    if (shm_unlink(SHM_NAME) == -1) {
        perror("shm_unlink");
        exit(1);
    }

    return 0;
}
同步机制
// 必须配合信号量使用
#include <semaphore.h>

sem_t *sem = sem_open("/shm_sem", O_CREAT, 0644, 1);

// 写入时加锁
sem_wait(sem);
strcpy(shared_data, "Protected data");
sem_post(sem);

// 读取时也要加锁（如果需要一致性）
sem_wait(sem);
printf("Data: %s\n", shared_data);
sem_post(sem);
用管道作为共享内存的控制机制

套接字(本地套接字和网络套接字)
```

## 多进程

```C
创建进程：pid_t childparent = fork(); 父进程返回子进程号，子进程返回0，失败父进程返回-1；读时共享，写时复制，共享文件描述符
孤儿进程：子进程的释放由父进程管理，父进程释放但是子进程没有释放，子进程只能交由init进程管理(init是os创建的初始进程)
僵尸进程：子进程结束，但是没有人释放子进程的资源
回收子进程：在父进程的代码里边调用waitpid，父进程等待子进程释放
```

## 多线程

```C
头文件：#include <pthread.h>
POSIX库（不属于linux默认系统库，编译时需要用-lpthread参数）
void *task(void *arg){
    子进程执行的函数；
}

线程创建：pthread_create(&tid,NULL,task,NULL);创建成功返回0，失败返回非0
线程取消：pthread_cancel(tid);
线程阻塞：pthread_join(tid,NULL);导致调用this线程的函数，阻塞等待该线程
用c++语言时，用<thread>库
```

### 线程池

```C++
线程池：对多线程进行了管理
任务队列：c++中用stl中的容器；生产者消费者模型；使用任务队列的是生产者，
设计形式参数：函数的地址是函数指针，所以第一个参数为函数指针
第二个为参数
工作的线程：相当于消费者：准备N个
管理者的线程：管理工作的线程，监测生产者数量和消费者的数量，合理增加/减少工作线程，准备一个线程id
```

### 线程间(thread)同步

```C
互斥锁
读写锁（读时共享，写时互斥）
条件变量
信号量（升级版互斥锁）
自旋锁（可以避开进程/线程上下文的开销）mtxObj
```

```Shell
自旋锁的应用
```