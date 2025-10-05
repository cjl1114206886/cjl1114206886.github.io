### JSON

```C++
JSON（JavaScript Object Notation）是一种轻量级的数据交换格式，
具有易读性和易写性，常用于前后端之间的数据传输和存储。下面是JSON格式的一些基本概念和语法：
对象（Object）：以花括号 {} 包围的键值对集合。每个键值对由冒号 : 分隔，键为字符串，值可以是任意JSON类型。键值对之间使用逗号 , 分隔。
{"key1": "value1","key2": "value2"}
数组（Array）：以方括号 [] 包围的值的有序集合。每个值可以是任意JSON类型，多个值之间使用逗号 , 分隔。
["value1","value2","value3"]

字符串（String）：被双引号 "" 包围的文本内容。
"Hello, JSON!"

数值（Number）：表示数字的值。
12345

布尔（Boolean）：表示真假的值，只有两个取值：true 和 false。

空值（Null）：表示空值的值，只有一个取值：null。

JSON格式的数据可以嵌套使用，可以在对象中包含对象、数组等复杂结构。例如：
{
  "person": {
    "name": "John",
    "age": 25,
    "hobbies": ["reading", "music"],
    "address": {
      "street": "123 Main St",
      "city": "New York"
    }
  }
}
```

```C++
nlohmann 
nlohmann 是一个流行的开源 JSON 库，提供了对于现代 C++ 的便捷、高效的 JSON 处理功能。
nlohmann JSON  库支持各种 JSON 数据操作，如解析、序列化、访问、修改等，同时也支持与 STL 数据结构的无缝集成。
以下是一个简单的示例展示了如何使用  nlohmann JSON 库：
首先，您需要从 nlohmann JSON 的 GitHub 仓库（https://github.com/nlohmann/json）中下载并安装这个库。
```
### 自己实现json
![](https://u0b8ml0m3m.feishu.cn/space/api/box/stream/download/asynccode/?code=NzkxOTcwNzc5MWU2NjgxNGEwZGE2MjEwY2MwOTU1MzRfNFNSd3FROUl6aDhVdjJHdDUyVkkzSzJ2SElIRW5XbVlfVG9rZW46T3pUU2JoYWNMb2lnWjh4cWRBdmNKS1ZybnNnXzE3NTk1NjY2NDU6MTc1OTU3MDI0NV9WNA)![](https://u0b8ml0m3m.feishu.cn/space/api/box/stream/download/asynccode/?code=N2JkM2Y4NzFkNDYyMmVlYTJhMjgxOTg2YmRlYzg0NGNfUklGcE1XMkltTnZjV015bmgxZnlKQ01Cb2ZiRVBFZXFfVG9rZW46WmFlT2Jyem1Yb3pSRTR4U0JkUGM2MjFRbmdoXzE3NTk1NjY2NTE6MTc1OTU3MDI1MV9WNA)