> #### STL
- ##### 容器
    - 顺序性容器：vector、list、deque（对于前端的插入效率更高
    ）
    - 关联性容器：便于快速查值。set（只含有key）、map（一对对的key/value）
- ##### 泛型算法：提供了许多可作用于容器类及数组类型上的操作
    - find()
    - sort()
    - replace()
    - ...

## The Arithmetic of Pointers


## Making Sense of Iterators
> #### 定义使用标准容器的iterator
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;每个标准容器都提供有一个名为`begin()`和`end()`的操作函数，返回的`iterator`分别指向第一个元素和指向最后一个元素的下一个位置。  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;所以不管怎么定义一个`iterator`对象，都是对其进行赋值、比较、递增、提领操作。
 1. 迭代对象的类型决定如何访问下一元素
 2. `iterator`所指的元素类别决定提领操作的返回值  

如果我们要定义`iterator`来指向容器的开始或者结尾
```
#include <vector>
#include <iterator>
// 以vector为例
typename vector<int>::iterator iter1 = vec.begin();
typename vector<int>::iterator end_iter = vec.end();
typename vector<int>::const_iterator iter2 = vec.begin(); //不允许修改

```
```
// 遍历操作
for (; iter1 != end_iter; ++iter1)
{
    cout << *iter1 << ' ';
}
```
***注意*** ：为什么要在`typename vector<int>::iterator`使用`typename`？  

    在vc6.0下，如果不加typename，是可以编译的；但是在gcc下，就不能。
    原因大概是要让编译器认为iterator是一个类型，而不是数据成员（这里很模糊，也不知道是否正确，如果想起来查阅资料会补充的）
    
## Operations Common to All Containers
> #### 容器类共通操作（包括string）
- equality `==` 和inequality `!=` ，返回`true`和`false`
- assignment `=` ，复制
- `empty()` 返回`true`和`false`
- `size()` 返回容器类元素个数
- `clear()` 删除所有元素
- `begin()` 返回一个iterator，指向第一个元素
- `end()` 返回一个iterator， 指向最后一个元素的下一个位置
- `insert()` 插入
- `erase()` 删除

## Using the Sequential Containers
> #### 顺序容器的定义
- 空
```
list<int> slist;
vector<int> vec;
```
- 特定大小
```
list<int> slist(100);
vector<int> vec(100);
```
- 特定大小，每一个元素给定同样的初值
```
list<int> slist(100, -1); // 全是-1
vector<int> vec(100, 1);
```
- 通过iterator产生容器
```
int ia[3] = {1, 2 ,3};
vector<int> fib(ia, ia + 3);
```
- 复制
```
list<int> list1;
list<int> list2(list1);
```

## Using the Generic Algorithms
```
#include <algorithm>
```
- `find()`：找到的`iterator`指向该目标，没找到指向`last`
- `binary_search()`：用于有序集合搜索，搜到目标返回`true`否则返回`false`
- `count()`
- `search()`
- ...
- 

## How to Design a Generic Algorthm
> #### 思想：点->面，不断的将一个算法泛型化

```
// bind2nd() 绑定适配器 还没有讲到
template <typename InputIterator, typename OutputIterator, typename Elemtype, typename Comp>
OutputIterator filter(InputIterator first, InputIterator last, OutputIterator at, const Elemtype &val, Comp pred)
{
    while ((first = find_if(first, last, bind2nd(pred, val))) != last) {
        cout << *first << endl;
        *at++ = *first++;
    }
    return at;
}
```
> #### Function Object Adapter
绑定适配器（暂时没有详细内容）


## Using a Map
> #### 使用
```
#include <map>
#include <string>
map<string, int> words;
// 输入key/value最简单的方式
words["iAmKey"] = 1;

// 统计次数
string str;
while (cin >> str)
    words[str]++;
```

> #### 查询map中是否存在key
- 将key当成索引使用，但是如果map中没有，会将它放入map里
```
int count = 0;
if (!(count = words["lalala"])
    // 
```
- 利用map的`find()`不是泛型的`find()`，入股哦存在会返回一个`iterator`，反之返回一个`end()`
```
words.find("lalala");
```
- 利用map的`count()`，返回个数
```
if( words.count("fasdf"));
    // 
```

## Using a Set
> #### 使用
```
#include <set>
#include <string>
set<string> iset;
```
> #### 插入
```
iset.insert(ival);
iset.insert(vec.begin(), vec.end());
```

## How to Use Iterator Inserters
## Using the iostream Iterator