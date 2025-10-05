```C++
外部库
libev，libev是一个事件库，提供了事件循环、定时器和IO多路复用等功能，用于处理异步事件。
+-----------------------+
|        Application    |
|        Code           |
+----------^------------+
           |
+----------v------------+
|         libev          |
|  (Event-driven library)|
+----------^------------+
           |
+----------v------------+
|         epoll          |
|   (IO Multiplexing)    |
+-----------------------+
|         Kernel         |
|       (Linux OS)       |
+-----------------------+

底层原理:
+----+
|    |  bucket 0: [key1, value1] -> [key2, value2]
+----+
|    |  bucket 1: [key3, value3]
+----+
|    |  bucket 2: [key4, value4]
+----+
turn 0;
}
```