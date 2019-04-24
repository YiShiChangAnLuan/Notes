## How to Write a C++ Program


>#### 定义新的class应该为他提供自己的output运算符
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;让class的用户得以像面对内置类型一样地以同样方式输出对象内容  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;重载？！(目前并没有看到用什么来解决)

## Defining and Initialzing a Data Object

>#### 构造函数初始化语法
```
// TypeName Variable(value);
int num(0);
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果对象需要多个初值（例如复数），普通的就没办法完成任务，使用构造函数初始化语法就可以完成了。  

>#### const
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;被定义为const的对象，在获得初值之后，无法再有任何变动。一旦再给const对象指定新值，就会产生***编译错误***。

>#### 条件运算符（三目运算符）
```
expr
    ? 如果expr为true，执行这里
    : 如果expr为false，执行这里
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;其实他只是一个简单的`if-else`语句
```
if (expr)
{
    // code
}
else (expr)
{
    // code
}
```
>#### 运算符的优先级
1.  逻辑运算符&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**NOT**
2.  算术运算符&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\*** **/** **%**
3.  算术运算符&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**+** **-**
4.  关系运算符&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**<** **>** **<=** **>=**
5.  关系运算符&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**==** **!=**
6.  逻辑运算符&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**AND**
7.  逻辑运算符&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**OR**
8.  赋值运算符
```
// 这是不可能正确的，永远为false
! ival % 2 
```
## Writing Conditional and Loop Statements

>#### 这是一小段程序的伪码，觉得他对程序的逻辑挺有帮助的，小记一下
```
while 用户想要猜测的某个数列
{
    #1
    显示该数列
    while 用户所猜的答案并不正确 and 用户想要再猜一次
    { #2
        读取用户所猜的答案
        递增 number_of_tries
        if 答案正确
        { #3
            递增correct_guess
            将got_ittrue的值设为true
        }
        else
        {
            用户打错了，在此对他表示遗憾，并根据用户已猜过的总数，产生不同的回应结果    // #4
            询问用户是否愿意再试一次
            读取用户的意愿
            if 用户相应 no  // #5
                将go_for_it 的值设为false
        }
    }
}
```
>#### for循环中`++i`和`i++`区别浅析
```
for (size_t i = 0; i < n; ++i)
{
    // code
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在以前我常使用的是`i++`，并且在很多编译器中的预设代码也是`i++`，但是在《C++ Primer》中发现了不一样的东西。  

`i++`要==另外申请一个变量==来保存`i + 1`之后的值，因为`i`和`i + 1`的值都会用到，但是`++i`就不一样只存在一个变量。试想一下我们使用一个*类*来做运算的时候，效率是不是会特别低。

>#### switch
```
switch( flag )
{
    case 0:
    case 1:
        // code
        break;
    // ...
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这样写是可以的，相当于`||`
```
if (flag == 0 || flag == 1)
{
    // code
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;不过第一次遇到这样写，还是记录一下，说不定有什么用处

## How To Use Arrays and Vectors

>#### 使用array来初始化vector
```
// ArrayName[]   
vector<TypeName> VecName (ArrayName, ArrayName + ArrayNameSize);
```

## Pinters Allow for Flexibility

>#### & 取址运算符
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;希望取得对象所在的内存地址而非对象的值
>#### * 提领
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;取得位于该指针所指内存地址上的对象
>#### 防止对null指针进行提领操作
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;预防程序报错
```
if (pi && *pi == 1)
{
    // code
}
```
>#### `.`(dot) 和 `->`(arrow)
- `.`：对象自身调用操作
- `->`：指向对象指针调用操作

## Writing and Reading Files

>#### 头文件
```
#include <fstream>
```
>#### 输出模式开启文件
```
ofstream OutFile("FileName.txt");
```
- 如果指定文件不存在，就会有一个新的文件被产生出来并打开供输出使用。  
- 如果指定文件已经存在，这个文件会被打开用于输出，而文件中原有的数据就会被丢弃。
```
// 追加模式
ofstream OutFile("FileName.txt", ios_base::app);
// 如果需要边读边写，以追加模式打开档案，文件位置位于末尾，必须重新定位。
// 但是写入操作都会将数据添加在文件末尾
ofstream IoFile("FileName.txt", ios_base::in|ios_base::app);
IoFile.seekg(0);    // 定位到起始处
```
