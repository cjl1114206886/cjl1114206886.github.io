```C++
deque（double-ended queue"）内部有一个中控器，维护其内部空间，支持随机访问
deque容器：逻辑模型：双端数组  物理模型：
deque 容器存储数据的空间是由一段一段等长的连续空间构成，各段空间之间并不一定是连续的，可以位于在内存的不同区域。为了管理这些连续空间，deque 容器用指针数组 map 管理各个连续空间的首地址； map 数组中存储的都是指针，指向那些真正用来存储数据的各个连续空间。
与vector的区别：
1.vector对头部插入删除效率低，数据越大效率越低，deque头端插入删除快 
2.vector访问元素的速度比deque快
两端的插入和删除 push_back(),push_front;pop_front,pop_back()

deque排序：sort，需要包含头文件，默认升序；支持随机访问的迭代器，都可以用sort算法进行排序
```