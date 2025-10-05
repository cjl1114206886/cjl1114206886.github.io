```C++
文本文件和二进制文件
文本文件：ASC码
二进制：010101存储的

ofstream输出，ifstream输出，fstream读写
    // 创建流文件对象
    ofstream ofs;
    // // 指定打开方式
    // ofs.open("E:/桌面/test.txt", ios::out); // 绝对路径和相对路径都可以
    // // 写内容
    // ofs << "张 三 1  8岁！" << endl; // 输出一个换行
    // // 关闭文件
    // ofs.close();
    // 读文件类似
    ifstream ifs;
    ifs.open("E:/桌面/test.txt", ios::in);
    if (!ifs.is_open())
    {
        cout << "文件打开失败！" << endl;
        return 0;
    }
    // 读数据（三种方式）
    // 1
    //  char buf[1024] = {0};
    // while (ifs >> buf)
    // {
    //     cout << buf << endl;
    // }
 
    // 2
    // char buf[1024] = {0};
    // while (ifs.getline(buf, sizeof(buf)))
    // {
    //     cout << buf << endl;
    // }
 
    // // 3
    // string buf;
    // while (getline(ifs, buf)) // 用的一个全局的getline
    // {
    //     cout << buf << endl;
    // }
    // 4
    char c;
    while ((c = ifs.get()) != EOF) // 没有读到文件里边EOF=（-1）:end of file
    {
        cout << c;
    }
 
    // 关闭
    ifs.close();
    
    // 流文件
// 写入ostream的write
// 1.包含头文件
    // 2.创建流文件
    ifstream ifs;
    // 3.打开判断是否成功
    ifs.open("E:/桌面/test.txt", ios::in | ios::binary);
    if (!ifs.is_open())
    {
        cout << "打开失败" << endl;
    }
    // 读文件
    person p;
    ifs.read((char *)&p, sizeof(person)); // 对象接受，再强转为char*
    cout << p.m_Age << "  " << p.m_Name;
    
写入文件当路径不存在时不会自动创建文件夹，并且受到linux的文件权限影响
    std::ofstream log;
    char*       ss        = "1111";
    std::string pathLdm   = "./";
    std::string data_name = "test";
    log.open(pathLdm + data_name + ".ldm", std::ios::out);
    std::cout << "11" << std::endl;
    log.write(ss, sizeof(ss));
    log.close();
```