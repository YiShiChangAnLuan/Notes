## How to Write a Function

>#### 得到某个类型的最大/最小值，参考标准库`numeric_limits class`
```
#include <limits>
int max_int = numeric_limits<int>::max();
double min_dbl = numeric_limiys<double>::min();
```

## Invoking a Function

>#### reference 引用

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在为了函数的**参数**和传入的**实际对象**产生关联，（可以简单认为修改参数，就可以修改实际对象），可以通过***传址***（pass by reference）来实现，最简单的方法就是将参数声明称一个`reference`
```
void ChangeValue(int & value)
{
    value = 666;
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;将参数声明为`reference`:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **1.** **希望得以直接对传入的对象进行修改**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **2.** **降低复制大型对象的额外负担** 

###### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;但是在传递==内置类型==的时候，建议==不要==使用传址的方式。
###### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;传址机制主要用于传递`class object`

>#### 作用域及范围
###### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;储存期（范围）：为对象分配内存的存活时间
###### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;作用域（scope）：对象在程序内的存活区域
###### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象如果在函数以外声明，具有所谓的`file scope`  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象如果拥有`file scope`，那么从他的声明点到文件末尾都是可见的。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对象的内存在`main()`开始执行前就已经分配好了，直到程序结束。

>#### 动态内存管理
###### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;内存由程序的空闲空间（free store）分配而来，也称为堆内存（heap memory）
```
// 表达式的形式如下
// type可以是任意的内置类型，也可以是程序知道得class
new Type
// eg.1
int *pi = new int;
delete pi;

// eg.2
int *pia = new int[20];  // 声明一个长度为20的数组，但是不能给该数组初始化
delete [] pia;
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`delete` 会释放指针所指的对象  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`delete []` 会释放数组中的所有对象，所以指针指向数组一定用这个  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;如果我们不使用`delete`释放对象，那么由`heap`分配的对象就永远不会被释放，这就叫==memory leak==（内存泄漏）

## Providing Default Parameter Value

>#### 提供默认参数值
```
void WriteSomething(string info, ofstream &ofil)
{
    // code
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;默认情况下，其实不想产生`ofstream &ofil`这个参数，用户可能不清楚需要填写这个参数。但是又希望那些知道这个参数，并且要自定义的用户使用他们，这时候就可以使用默认参数。
```
void WriteSomething(string info, ofstream *ofil = 0)
{
    // code
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;==注意==这里讲引用换成了指针，这是因为引用必须代表一个对象。

>#### 默认参数规则
1. ###### 默认值的解析操作由最右边开始。如果提供了默认值，那么这一参数的右边的所有参数必须拥有默认值。
1. ###### 默认值只能指定一次，可以在函数声明出，也可以在函数定义处。但是不能再两个地方同时设定
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;通常函数声明放在头文件中，方便观察。所以为了提高可见性，默认值放在函数声明处。
```
// xxx.h
void display(string str, ostream & = cout);
```
```
// xxx.cpp
include "xxx.h"
void display(string str, ostream & os)
{
    os << string << endl;
}
```
## Using Local Static Object

>#### 局部静态对象
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;局部静态对象所处的空间，即使在不同的函数调用过程中，依然持续存在。

## Declaring a Function Inline


## Providing Overloaded Function

>#### 重载函数
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;名字相同、返回类型相同，参数列表不相同的函数。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;在函数调用的时候会根据调用者提供的实际参数列表，来和每一个重载函数作对比，找出其中合适的。

## Defining and Using Template Function

>#### 模板函数
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`function template`将参数列表中指定的全部（或部分）参数的类型信息抽离了出来。
```
template <typename T> // template <class T>两者意义是一样的
                        //但是建议使用typename，以防出现多重意思
void display(const string &msg, const vector<T> &vec)
{
    // code
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`function template`也可以重载
```
template <typename T>
void display(const string &msg, const list<T> &lt)
{
    // code
}
```
## Pointers to Function Add Flexibility

>#### 函数指针
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;函数指针必须指明函数的**返回类型**和**参数列表**
```
// 这些函数返回一个vector类型的指针
const vector<int> *func1( int size );
const vector<int> *func2( int size );
const vector<int> *func3( int size );

bool IsUseFunc(const vector<int>* (*func_ptr)(int))
{
    // code
}
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;只要在`bool IsUseFunc(const vector<int>* (*func_ptr)(int))`传入函数的名字就可以调用了。  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;为什么不是
```
const vector<int>* *func_ptr(int);
// const vector<int>** func_ptr(int);
// 声明了一个函数
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这句话代表的是返回类型是一个指针，这个指针指向一个另一个指针，后一个指向一个元素类型为`int`的`const vector`。

## Setting Up a Header File
