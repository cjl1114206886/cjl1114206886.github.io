```C++
std::promise
std::promise 类是 C++11 标准库中的一部分，用于在一个线程中设置一个值，
然后可以在另一个线程中获取这个值。
void generateData(std::promise<int>& p) {
    // 模拟生成数据，并将数据设置到 promise 中int data = 42;
    p.set_value(data);
}

int main() {
    // 创建一个 promise 对象，用于产生数据
    std::promise<int> myPromise;

    // 获取与 promise 相关联的 future 对象
    std::future<int> myFuture = myPromise.get_future();

    // 在另一个线程中生成数据std::thread dataThread(generateData, std::ref(myPromise));

    // 获取异步操作产生的结果int result = myFuture.get();

    // 等待数据生成线程结束
    dataThread.join();

    // 输出异步操作产生的结果
    std::cout << "Generated data: " << result << std::endl;

    return 0;
}

linux的posix线程库更加细粒度。c++的<thread>更适合面向对象
```