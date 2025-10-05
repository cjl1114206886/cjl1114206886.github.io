```C++
std::move是C++中的一个函数模板，位于<utility>头文件中。
它用于将对象的所有权从一个对象转移到另一个对象，通常被用于实现移动语义。
使用std::move后，源一维向量内的数据会变得无效，此时只能通过目标一维向量来访问和操作这些数据
int main() {
    // 创建一个 vector 对象，并向其中添加一些元素
    std::vector<int> vec1 = {1, 2, 3, 4, 5};

    // 输出原始 vector 的大小
    std::cout << "Original vector size: " << vec1.size() << std::endl;

    // 使用 std::move 将 vec1 中的元素移动到 vec2 中
    std::vector<int> vec2 = std::move(vec1);

    // 输出移动后的 vector 的大小
    std::cout << "Moved vector size: " << vec2.size() << std::endl;

    // 尝试访问原始 vector，将会发现其已经被移动过去了
    std::cout << "Original vector size after move: " << vec1.size() << std::endl;

    return 0;
}
```