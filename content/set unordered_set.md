
```C++
unordered_set底层是hash表，使用桶排序，是无序关联容器，设计目的是为了增加，删除，查找更加快速
|场景|unordered_set|set|vector|
|---|---|---|---|
|查找元素|O(1) ✅|O(log n)|O(n)|
|插入元素|O(1) ✅|O(log n)|O(1)末尾|
|去重|自动 ✅|自动|需手动|
|有序性|❌|✅|❌|
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
```
#include <unordered_set>

#include <vector>

#include <iostream>

std::vector<int> removeDuplicates2(std::vector<int> nums)

{

    std::unordered_set<int> s(nums.begin(), nums.end());

    return std::vector<int>(s.begin(), s.end());

}

int main(int argc, char const *argv[])

{

    std::unordered_set<int> uset = {4, 5, 6, 8, 9, 11, 12, 13, 17, 18, 19};

    uset.insert(20);

    for (auto x : uset)

    {

        std::cout << x << std::endl;

    }

  

    std::cout << "uset.size()" << uset.size() << std::endl;

    std::cout << "uset.empty()" << uset.empty() << std::endl;

  

    std::cout << "bucket count : " << uset.bucket_count() << std::endl;

    std::cout << "装载因子 load factor bucket factor :" << uset.load_factor() << std::endl;

    // 经典用法 1.count判断是否存在

    if (uset.count(1) > 0)

    {

        std::cout << "0x03u" << std::endl;

        return 0x03u;

    }

    if (uset.find(8) != uset.end())

    {

        std::cout << "判断存在方法2：find" << 8 << std::endl;

    }

    // if (uset.contains(8))

    // {

    //     std::cout<<"c++ 20 特性"

    // }

  

    // 经典用法 2.用set构造去重

    std::vector<int> nums = {4, 2, 4, 1, 2, 3, 1};

    std::vector<int> numremoveDuplicate = removeDuplicates2(nums);

    for (auto x : numremoveDuplicate)

    {

        std::cout << x << " ";

    }

    // 安全删除,c++20有std::erase_if

    if (uset.count(8) > 0)

    {

        uset.erase(8);

        std::cout << "已经删除8" << std::endl;

    }

  

    return 0;

}
```