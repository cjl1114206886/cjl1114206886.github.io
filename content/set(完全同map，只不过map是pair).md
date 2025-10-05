```C++
set和multiset容器：插入的时候自动排序（关联式容器）：底层结构是二叉树（红黑树）
set不允许有重复元素，multiset允许有重复元素
// 包含头文件时只需要包含一个
 
// 构造：1.默认构造 2.拷贝构造
 
// 赋值操作：=
 
// 插入数据时，
只有insert方法,clear,erase;insert返回pair<iterator,bool>;用pair.first和second取出
 
// 大小和交换:empty,size,swap
 set插入数据时会返回是否成功，multiset不会做检测
// 查找和统计：
find：如果存在，返回迭代器，若不存在，返回set.end()的位置;count（key）在set中这可能是0或1，multiset可能大于1
// set排序规则：默认从小到大，利用仿函数，可以改变排序规则
// 1.set存放内置数据类型 2.set存放自定义数据类型
// 自定义数据类型，都会指定排序规则
// pair对组的使用
// 创建方式：1. 2.make_pair
// 不需要包含头文件
```