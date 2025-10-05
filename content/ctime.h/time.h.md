### include `<ctime.h>#include <time.h>`
```Shell
    GMT和UTC的区别
    https://blog.csdn.net/sinat_25207295/article/details/130609868
    C++中时间的使用
    http://sodino.com/2015/03/15/c-time/
    
    time_t     json_time{};    //c语言头文件，用于存储从1970到现在的秒数
    struct tm* time_info{};    //c语言，struct tm结构体包含年月日时分秒
    (void)time(&json_time);    //time.h头文件提供的函数， 是调用了 C 语言标准库中的 time 函数，用于获取当前系统时间的时间戳，并将时间戳存储到变量 json_time 中
    time_info = gmtime(&json_time);    //变量的声明中使用 extern 关键字，表示该变量的定义在其他文件中 extern time_t time (time_t *__timer) __THROW;获取UTC时间
    //gmtime 函数用于将时间戳（time_t 类型）表示的时间转换为世界协调时间（UTC）
    std::string sec  = std::to_string(time_info->tm_sec);
    std::string min  = std::to_string(time_info->tm_min);
    std::string hour = std::to_string(time_info->tm_hour + 8);    //东八区
    std::string mday = std::to_string(time_info->tm_mday);
    std::string mon  = std::to_string(time_info->tm_mon + 1);
    std::string year = std::to_string(time_info->tm_year + 1900);
    
    // std::cout<<level<<"-"<<year<<"-"<<month<<"-"<<day<<"-"<<hour<<"-"<<mintu<<"-"<<sec<<".log"<<std::endl;
    std::string timeCombina =levelName[level]+"-"+year+"-"+mon+"-"+mday+"-"+hour+"-"+min+"-"+sec+".log";

    // char ch[100] = "";
    //     当使用 char utc_str[20] = ""; 时，初始化了整个字符数组 utc_str 为一个空字符串，也就是所有元素都被设置为 \0。
    // 而当使用 utc_str[19] = '\0'; 时，只是将数组 utc_str 中的第 20 个元素（下标为 19）赋值为字符串结束符 \0，
    // YYYY-MM-DD HH:MM:SS
    // strftime(ch, sizeof(ch), "%Y-%m-%d %H:%M:%S", time_info);
    // printf("UTC标准格式是:%s\n", ch);
    
    
UTC（Coordinated Universal Time），协调世界时，是国际标准时间，是世界标准时间的基础，其时钟与地球自转同步。
UTC 是由国际无线电咨询委员会（CCIR）建立的，其前身是格林尼治标准时间（GMT）。UTC 与格林尼治平均时（UT1）之间的差异不应超过0.9秒。UTC 通常通过使用原子钟来维持其稳定性。
UTC 不考虑夏令时的影响，因此与 GMT 有所区别。在 UTC 中，一天被划分为24小时，每小时被划分为60分钟，每分钟被划分为60秒。UTC 时间格式通常采用 "YYYY-MM-DD HH:MM:SS" 的形式表示。
夏令时是一种能够节省能源、增加白昼时段以及减少晚间照明需求的时间调整方式。通常在夏季期间，当钟表调快一小时时，就进入了夏令时；而当钟表调回时，则退回到标准时间

闰秒是UTC和GMT的差值：产生的原因是：地球自转不是恒定的，而UTC认为是恒定的，所以会产生差值
```