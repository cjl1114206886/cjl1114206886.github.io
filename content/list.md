
```
## `#include <list>`
```C++
list链表容器：stl中的list是一个有哨兵的双向循环链表；满足左闭右开原则
struct __list_node {
  typedef void* void_pointer;
  // 双向链表节点
  void_pointer next;
  void_pointer prev;
  T data;
};
push_front();//底层是insert()
push_back();
pop_front();//底层是erase()
pop_back();
begin()
```