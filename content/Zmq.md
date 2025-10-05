```C++
完整讲解zmq和序列化
https://blog.csdn.net/qq_21438461/article/details/135275159

inproc（In-process）：表示进程内通信，允许在同一个进程内创建多个 ZeroMQ 套接字，并通过这些套接字进行快速的、高效的通信。数据在同一进程内通过内存进行传递，不需要经过网络协议栈，因此具有低延迟和高性能的优势。
ipc（Inter-process Communication）：表示进程间通信，允许不同进程之间通过共享文件系统进行通信。多个进程可以通过相同的文件路径来连接到相同的 ZeroMQ 套接字，实现跨进程的数据传输。

epgm 和 pgm 的区别：
epgm（Encapsulated PGM）：是一种基于 UDP 多播的传输协议，使用 PGM（Pragmatic General Multicast）协议封装数据包进行多播传输。epgm 允许 ZeroMQ 套接字在网络上通过 PGM 协议进行多播通信，提供可靠的数据传输。
pgm（Pragmatic General Multicast）：是一种多播传输协议，可确保数据通过网络以最佳方式进行传输，即使在存在丢包或网络拥塞的情况下也能保持可靠性。

当你创建一个新的上下文时，它从一个I/O线程开始。一般的经验法则是每秒允许每GB数据有一个I/O线程进出。要增加I/O线程的数量，请在创建任何套接字之前使用zmq_ctx_set（）调用：
int io_threads = 4;
void *context = zmq_ctx_new ();
zmq_ctx_set (context, ZMQ_IO_THREADS, io_threads);
assert (zmq_ctx_get (context, ZMQ_IO_THREADS) == io_threads);
```