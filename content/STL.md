## 简介
```SQL
序列化容器
关联容器：没理解
迭代器（只理解了几个概念）：
    1.patial speciallization:针对模板的参数更进一步的条件限制给出一个特化版本，而不是限制一个固定的值
    2.迭代器如何做到独立的：container和iterator是两个模板，容器作为iterator的T，在iterator中添加T *ptr;
```
```C++
STL(standard template library)标准模板库:数据结构和算法的一套标准
划分：1.容器 2.算法 3.迭代器
1.HP版本：（惠普）版本是STL实现版本的始祖
2.PJ版本:Visual C++使用的是P.J.Plauger版本，Visual C++现在都是集成在Visual Studio当中
3.RW版本:C++Builder使用Rouge Wave版本的STL；
4.SGI版本：g++使用SGI版本的STL，书中提到GCC指的是GNU Compiler Collection(GNU 编译器集合)，包括了g++和gcc。
5.GNU ISO C++：linux下/usr/include/c++/9下的版本，属于SGI的改良版本:https://github.com/gcc-mirror/gcc/blob/master/libstdc%2B%2B-v3/include/bits/stl_vector.h

容器：array，vector，list,map...
算法：algorithm
    1.质变算法（在运算过程中会改变区间内元素的内容） 2.非质变算法（运算过程中不会更改）
迭代器：算法通过迭代器访问容器中的元素；每个容器有自己专属的迭代器；迭代器非常类似指针
    分类：输入，输出，前向，双向，随机访问（最强大）；后两个常用
    
2.1 序列容器
	2.1.1 定长数组 array `c++11`
	2.1.2 动态数组 vector
	2.1.3 双端队列 deque
2.2 链表容器
	2.2.1 双向链表 list
	2.2.2 单向链表 forward_list `c++11`
2.3 适配器容器
	2.3.1 栈 stack
	2.3.2 队列 queue
	2.3.3 优先序列 priority_queue
		2.3.3.1 构造小顶堆
		2.3.3.2 使用自定义类型作为基础类型
2.4 有序关联容器（底层：红黑树）
	2.4.1 集合 set/multiset
	2.4.2 映射 map/multimap
2.5 无序关联容器（底层：哈希表）
	2.5.1 无序映射 unordered_map/unordered_multimap `c++11`
	2.5.2 无序集合 unordered_set/unordered_multiset `c++11`
```
## [string](string.md)
## [vector](vector)

## [unordered_map](unordered_map)

## [list](list)

## [queue](queue)

## [dequeue](dequeue)

## [stack](stack)
## [set(完全同map，只不过map是pair)](set(完全同map，只不过map是pair).md)

## [算法库](算法库)
