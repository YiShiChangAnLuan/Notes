 ## 使用编译器替换预处理器
 ---
 #### 常量
 `#define`不被视为语言的一部分
 ```
 #define NUMBER 0.9527
 ```
 - 这样写使用的名称很可能并未进入记号表。  
 - 预处理器如果盲目将NUMBER替换为0.9527可能导致目标码出现多份0.9527
 ```
 // 解决办法
 const double Number = 0.9527;
 ```
 如果用常量替换宏有两个特殊情况
 
- ##### 定义常量指针时  
由于常量的定义式通常是放在头文件内，很有必要将**指针**（而不只是指针所指之物）声明为const
```
const char* const str = "i am a str";
```
C++中最好不要使用`char*`使用`string`更好
```
const std::string str("i am a str");
```
- ##### class专属常量（#define 是无法创建一个class专属常量的）
为了将常量的作用域限制于class内，必须让他成为class一个成员。并且确保此常量至多只有**一个实体**，那就让他成为一个`static`成员
```
class ClassName
{
    private:
        static const int Num = 9527; // 常量声明式
}
```
这里`Num`只是一个声明式，而**非定义式**。通常C++要求所使用的任何东西都提供一个定义式。如果他是个class专属常量又是`static`且为整数类（int、char、bool），需要特殊处理。
- 不取他们的地址：可以声明并使用它们而无需提供定义式
- 如果要取某个class专属常量的地址，或者编译器强制要看到一个定义式，就必须另外提供一个定义
```
const int ClassName::Num;
```
***将该式子放进一个实现文件（而不是头文件）***，由于class常量在声明时获得初值，定义的时候就不需要再设初值。  

旧式的编译器不支持上述语法
```
// 位于头文件
class ClassName
{
    private:
        static const int Num; // 常量声明式
}
// 位于实现文件
const int ClassName::Num = 9527;
```
但是如果class编译期间需要一个class常量值，可改用“the enum hack”补偿做法。（一个枚举类型的数值可以当作int被使用）
```
// 位于头文件
class ClassName
{
    private:
        static const int Num; // 常量声明式
        enum { size = 5 };
        int arr[size];
}
```
- “enum hack”比较像`#define`，取一个`const`地址是合法的，但是取一个`enum`地址是不合法的
- 许多代码都用到了，所以还是必须认识。（哈哈哈哈哈，莫名其妙）


#### 形似函数的宏
```
// 写这种宏的时候要将每个实参加上小括号
#define CALL_WITH_MAX(a, b) f((a) > (b) ? (a) : (b))
```
一种特殊情况
```
int a = 5, b = 0;
CALL_WITH_MAX(++a, b);  // a被累加两次
CALL_WITH_MAX(++a, b + 10);  // a被累加一次
```
可以使用以下方法来杜绝
```
template <typename T>
inline void CallWithMax(const T& a, const T& b)
{
    f(a > b ? a : b);
}
```
每个函数都接受两个同型对象，并以其中较大者调用`f`。

#### REMEMBER
- ***对于单纯常量，最好以`const`对象或者`enums`替代`#define`***
- ***对于形似函数的宏，最好改用`inline`函数替代`#define`***