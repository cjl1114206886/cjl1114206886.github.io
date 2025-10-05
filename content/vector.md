
```
## `include <vector>`
```C++
vector采用的数据结构是线性的连续空间，
vector在增加元素时，如果超过自身最大的容量，vector则将自身的容量扩充为原来的两倍。
GNU IOS C++:const size_type __len = size() + (std::max)(size(), __n);

.data() 是 std::vector 的成员函数，用于返回容器中存储数据的指针。
append()将新的字符添加到字符串尾部

c++中建立向量:vector<vector<>>

std::vector<uint8_t> aa;
aa.assign(bb.begin(), bb.end());用bb的开始和结尾来填充aa

std::vector的emplace_back()函数用于在向量中的特定位置（或末尾）构造一个元素。
与push_back()函数类似，
但是emplace函数允许您通过传递参数来直接在容器内构造元素。
emplace函数接受变参模板参数，这意味着您可以将参数传递给元素的构造函数。
当使用emplace_back方法时，参数会直接传递给元素类型的构造函数，因此不需要手动创建一个临时对象。
```
```C++
更重要的：auto it = v.begin();，下面为知识点
   // 通过迭代器访问容器中的数据
    vector<int>::iterator itbegin = v.begin(); // v.begin是起始迭代器，指向容器中第一个元素
    vector<int>::iterator itend = v.end();     // v.end是结束迭代器 指向容器最后一个元素的下一个元素
    while (itbegin != itend)
    {
        cout << *itbegin << endl;
        itbegin++;
    }
     // 第二种遍历(常用)
    // for (vector<int>::iterator i = v.begin(); i != v.end(); i++)
    // {
    //     cout << *i;
    // }
    
	vector容器中存放自定义数据类型 vector<person v>;
	vector容器中嵌套容器 vector<vector<int v>>;
	
```
```C++
vector容器（又称为单端数组）
iterator start; iterator begin() { return start; }
iterator finish; iterator end() { return finish; }
iterator end_of_storage;

vector与普通数据的区别：vector可以动态扩展，数组不可以,动态扩展是找到一块新空间，再复制过去
vector的迭代器是可以支持随机访问的迭代器

vector的构造函数：
     1.vector() : start(0), finish(0), end_of_storage(0) {}//成员初始化列表
     2.vector(InputIterator first, InputIterator last) :start(0), finish(0), end_of_storage(0)
     3.n个元素element构造：vector(size_type/int/long n, const T& value)
     4.拷贝构造函数
        vector(const vector<T, Alloc>& x) {
        start = allocate_and_copy(x.end() - x.begin(), x.begin(), x.end());
        finish = start + (x.end() - x.begin());
        end_of_storage = finish;
        }

vector的赋值操作
1.= ：vector给vector赋值，但是不同的namespace可以
2.assign

vector的容量和大小 
    empty(); return begin() == end();
    capacity()容量; return size_type(end_of_storage - begin());
    size()容器中元素个数；finish-start
    max_size():max_size() const { return size_type(-1) / sizeof(T); } size_type(-1)所以这个相当于一个有符号数-1，赋值给了一个无符号数
    resize()重新指定容器元素个数，若容器变长，则以默认元素填充
vector的插入和删除 puch_back尾部插入;
    push_back();
        GNU_IOS_C++:
        void push_back(const T& x) {
            if (finish != end_of_storage) { //还有剩余空间
              construct(finish, x); // 全局函数
              ++finish; //指针++
            }
            else
              insert_aux(end(), x);
          }
          g++:
          vector中申请两倍的逻辑判断：
            1.push_back()填满的情况下，分配为两倍
                push_back中是size_type(1),即__n的值为1，而size最小为1，所以申请两倍
                const size_type __len = size() + (std::max)(size(), __n);
            2.push_back()没填满正常的情况
            push_back不满的情况下，直接插入，模板类的构造函数的完美转发
                template<typename _Tp, typename... _Args>
                  static
                  _Require<__and_<__not_<__has_construct<_Tp, _Args...>>,
                             is_constructible<_Tp, _Args...>>>
                  _S_construct(_Alloc&, _Tp* __p, _Args&&... __args)
                  noexcept(std::is_nothrow_constructible<_Tp, _Args...>::value)
                  { ::new((void*)__p) _Tp(std::forward<_Args>(__args)...); }
            SGI C++:const size_type len = old_size != 0 ? 2 * old_size : 1; // 空间变为原来的两倍,平均代价为O(1)
            当c++版本>=11时，push_back()和emplace_back()版本没有任何区别
            #if __cplusplus >= 201103L
            void push_back(value_type&& __x)
            { emplace_back(std::move(__x)); }
    pop_back()尾部删除；
        void pop_back() {
                --finish;
                destroy(finish);
              }
    insert,erase,(insert和erase都需要提供迭代器)clear;
    使用 insert 方法插入元素（需要提供迭代器）：
        insert:
        1. 复制最后 3 个元素到新位置：
        [ 1, 2, 3, 4, 5, 6, 7, _, 5, 6, 7 ]
                            ↑
                          插入位置
        
        2. 将插入位置后的元素向后移动：
        [ 1, 2, 3, 4, _, _, 4, 5, 6, 7 ]
                            ↑
                          插入位置
        
        3. 填充插入的新元素：
        if (elems_after > n) {
            uninitialized_copy(finish - n, finish, finish);
            finish += n;
            copy_backward(position, old_finish - n, old_finish);
            fill(position, position + n, x_copy);
        }
        
        [ 1, 2, 3, 4, x, x, x, 5, 6, 7 ]
        else {插入点后面的元素少于或等于需要插入的元素，此时要插入的元素会填满甚至超过这些已有的元素空间。
                先移动到最后，插入值，再整体向前移动
            uninitialized_fill_n(finish, n - elems_after, x_copy);
            finish += n - elems_after;
            uninitialized_copy(position, old_finish, finish);
            finish += elems_after;
            fill(position, old_finish, x_copy);
        }
        4. 在未初始化内存中填充新的元素：
        [ 1, 2, 3, 4, 5, 6, 7, x, x, x ]
                            ↑
                          插入位置
        
        5. 复制插入点后的元素到新位置：
        [ 1, 2, 3, 4, x, x, x, 4, 5, 6, 7 ]
                            ↑
                          插入位置
        
        6. 填充插入的新元素：
        [ 1, 2, 3, x, x, x, 4, 5, 6, 7 ]
        
        
        erase:填充的是：ptrdiff_t是两个指针相减的结果的类型 ptrdiff_t *(0)
        iterator erase(iterator position) {
            if (position + 1 != end())
              copy(position + 1, finish, position);
            --finish;
            destroy(finish);
            return position;
          }
        cpp
        std::string str = "Hello";
        std::string::iterator it = str.begin() + 3; // 获取迭代器
        str.insert(it, 'X'); // 在位置为3的字符后插入字符 'X'
        使用 erase 方法删除元素（需要提供迭代器）：
        cpp
        std::string str = "Hello";
        std::string::iterator it = str.begin() + 1; // 获取迭代器
        str.erase(it); // 删除位置为1的字符
     insert:
vector的数据存取 
    1.at
    2.[]: return *(begin() + n);
    2.下标（1.2同string） 
    3.front返回容器中第一个元素 :return *begin();
    4.back返回容器中最后一个元素:return *(end() - 1);
vector容器互换（交换两个容器的元素） 内存大小必须相同
    swap：调用的是 std::swap(start, x.start);
    
vector预留空间：
    reserve():
            void reserve(size_type n) {
                if (capacity() < n) {
                  const size_type old_size = size();
                  iterator tmp = allocate_and_copy(n, start, finish);
                  destroy(start, finish);
                  deallocate();
                  start = tmp;
                  finish = tmp + old_size;
                  end_of_storage = start + n;
                }
              }
    减少vector在动态扩展时的扩展次数：reserve：预留空间不初始化，元素不可访问
```
## `#include <map>`
```C++
 // map和multimap容器
所有的元素都为pair；
pair中第一个元素为key键值，第二个元素为value，所有的元素都会根据key自动排序
// 关联式容器，底层是二叉树
// map和multimap区别：map不允许有重复key值，但是value可以重复
 
// 构造：1.默认 2.拷贝
 
// 赋值：=
 
// 插入：insert，同set
 
// 大小和交换
 
// 插入和删除：insert(1.2.make_pair 3.map<int, int>::value_type(7, 20) 4.m[4]=20;//第四种不建议)，clear，earse（key）
// 第四种如果是没有，则创建一个key为[]的下标，value为0的
 
// find，count（map只能为0和1，multimap可能为多个），返回迭代器
 
// map容器排序sort
```