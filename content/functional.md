```C++
常用的查找算法：
for_each(v.begin(),v.end(),函数/仿函数)，transform
find(找到元素返回迭代器，找不到返回结束迭代器)，对比自定义重载==
find_if：返回迭代器
adjancent_find:查找相邻的重复元素；查找成功返回第一个迭代器的位置，没有的化返回end迭代器的位置
binary_search:二分查找法;查到返回true，查不到返回false；只能在有序；如果是无序，结果未知
count;count_if；_if就是加一个谓词
 
常用排序算法：sort；
random_shuffle:洗牌，指定范围内的元素随机调整;引入<ctime.h>srand((unsigned int)time(null));按照时间打乱
merge:容器元素合并，放到另一个容器中;两个容器必须都是有序的；需要resize
reverse
 
常用的拷贝和替换算法：copy,replace,replace_if,swap

内建的函数对象：需要引入头文件<functional>1.算数仿函数 2.关系仿函数 3.逻辑仿函数
算数仿函数：实现的是四则运算 1.加法plus，减minus，乘multiplies，除divides，取模modules，取反negate
关系仿函数:实现关系对比的操作；eg：greater
逻辑仿函数：logical_and/or/not
```