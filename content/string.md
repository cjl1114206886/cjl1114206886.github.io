## `include <string>`
```C++
String
1.std::string::npos 是 std::string 类的一个静态成员变量，通常被用来表示字符串中不存在指定子字符串或字符的情况。
	静态变量
		std::string result = "{\"opcode\":" + 10 + "," + "\"room\"" +
		 ": room," + "\"data\"" + ": 1111111111111" + "}";
	 变量
		int         opcode = 10;
		std::string room   = "room";
		long        data   = 1111111111111;
		std::string result = "{\"opcode\":" + std::to_string(opcode) +
		 "," + "\"room\":\"" + room + "\"," + "\"data\":" + std::to_string(data) + "}";
 
2.string 转 vector
	vector  vcBuf;
	string        stBuf("Hello DaMao!!!");
	vcBuf.resize(stBuf.size());
	vcBuf.assign(stBuf.begin(), stBuf.end()); 

string转char *：
	c_str()函数
string限制展示数据数量
str.json.at(100)=0;
if (str_json.length() > 100)
{
    str_json.at(100) = 0;    // output the front 100 characters.
}
```
### 转移字符
```C++
Asc码: a小写：97，大写A：65;
0-31控制字符，31-126键盘上常见的
转义字符://
换行：\n,
制表符tab：\t(累计8个空格)对齐效果;
反斜杠：\\
分号：\"

拼装json
std::string getname() { return "liqiang"; }
std::string getclass() { return "3.5"; }
std::string getage() { return "18"; }
int main(int argc, char const *argv[]) {
  std::string str;
  str = std::string("{") + std::string("\"name\":") + std::string("\"") +
        getname() + std::string("\"") + std::string(",") +
        std::string("\"age\":") + getage() + std::string(",") +
        std::string("\"class\":") + std::string("\"") + getclass() +
        std::string("\"") + std::string("}");
  std::cout << str << std::endl;
  return 0;
}
```
```C++
c++中string怎么获取length（），及怎么具体判断'\0'的问题：
用string_straits中的string模板，模板参数T为char，length()函数调用
while(eq(*src++,eos()))
    len++;
eos()的实现是：return 0,返回转为char类型直接就是'/0'