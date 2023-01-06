转自：[foreach for each for\_each引发的探讨：c++世界中的循环语句\_w\_419675647的博客-CSDN博客](https://blog.csdn.net/w_419675647/article/details/106187529 "foreach  for each for_each引发的探讨：c++世界中的循环语句_w_419675647的博客-CSDN博客")

## 一 背景：

代码中看到 **for each**，注意，两个单词中间没有下划线，有同事问这个是不是和 **for\_each**一样？和**foreach**呢？我回答应该一样，但是内心很不安，尤其是作为一个c++的多年用户。

## 二 资料收集整理：

### 1 首先来看看我们最熟悉的 for\_each。

他的全名是 std::for\_each，来源c++的stl。头文件<algorithm>.  
当时是个模板函数了  
template <class InputIterator, class Function>  
Function for\_each (InputIterator first, InputIterator last, Function fn);  
标准库中的例子：

```
// for_each example
#include <iostream>     // std::cout
#include <algorithm>    // std::for_each
#include <vector>       // std::vector

void myfunction (int i) {  // function:
  std::cout << ' ' << i;
}

struct myclass {           // function object type:
  void operator() (int i) {std::cout << ' ' << i;}
} myobject;

int main () {
  std::vector<int> myvector;
  myvector.push_back(10);
  myvector.push_back(20);
  myvector.push_back(30);

  std::cout << "myvector contains:";
  for_each (myvector.begin(), myvector.end(), myfunction);
  std::cout << '\n';

  // or:
  std::cout << "myvector contains:";
  for_each (myvector.begin(), myvector.end(), myobject);
  std::cout << '\n';

  return 0;
}  
```

### 2 再来看看foreach。

搜索引擎里可以搜到很多相关标题，但是点进去后会发现，几乎都是在讲 for\_each。仅有的几篇异同点其实是个错误的宏定义。  
后来试着在QT的帮助文档里找了下，果然找到了。  
他是： - Global Qt Declarations  
foreach(variable, container)  
This macro is used to implement Qt’s foreach loop. The variable parameter is a variable name or variable definition; the container parameter is a Qt container whose value type corresponds to the type of the variable. See The foreach Keyword for details.  
Note: Since Qt 5.7, the use of this macro is discouraged. It will be removed in a future version of Qt. Please use C++11 range-for, possibly with qAsConst(), as needed.  
See also qAsConst().  
大致的翻译：**从Qt 5.7开始，不建议使用此宏。 它将在Qt的将来版本中删除。 请根据需要使用C ++ 11 range-for，可能与qAsConst（）一起使用。  
另请参见qAsConst（）。**  
是的，不再建议使用。

### 3 最后找找for each。

```
真难找，好像没有这个语法，但是代码里确实有使用，代码里的关键字是两个分开的单词，也无法到到对应的定义文件。
换到bing引擎，终于还是找到了。
《https://docs.microsoft.com/en-us/cpp/dotnet/for-each-in?view=vs-2019》
原来是微软自己的定义。
Iterates through an array or collection. This non-standard keyword is available in both C++/CLI and native C++ projects. However, its use is not recommended. Consider using a standard Range-based for Statement (C++) instead.
翻译：遍历数组或集合。 此非标准关键字在C ++ / CLI和本机C ++项目中均可用。 **但是，不建议使用它。** 考虑改为使用标准的基于范围的语句（C ++）。
是的，不建议使用了。
```

## 三 聊聊c++ 世界的循环语句

### 1 对容器中的每一个元素都调用函数的方法有以下：

第一种：基于迭代器

```
for (std::vector<int>::iterator it = ve.begin(); it < ve.end(); ++it)
  f(*it);
for (std::vector<int>::const_iterator it = ve.cbegin(); it < ve.cend(); ++it)
  f(*it);
for (std::vector<int>::iterator it = ve.begin(); it != ve.end(); ++it)
  f(*it);
for (std::vector<int>::const_iterator it = ve.cbegin(); it != ve.cend(); ++it)
  f(*it);
```

第二种：基于下标

```
for (int i = 0; i != ve.size(); ++i)
    f(ve[i]);
for (int i = 0; i < ve.size(); ++i)
    f(ve[i]);
for (std::size_t i = 0; i != ve.size(); ++i)
  f(ve[i]);
for (std::size_t i = 0; i < ve.size(); ++i)
  f(ve[i]);
for (std::vector<int>::size_type i = 0; i != ve.size(); ++i)
  f(ve[i]);
for (std::vector<int>::size_type i = 0; i < ve.size(); ++i)
  f(ve[i]);
```

第三种：stl模板函数

```
std::for_each(ve.begin(), ve.end(), f);
std::for_each(ve.cbegin(), ve.cend(), f);
std::for_each(std::begin(ve), std::end(ve), f);
std::for_each(std::cbegin(ve), std::cend(ve), f); // C++14
```

第四种：基于范围的循环

```
for (auto val : ve)
  f(val);
for (auto &val : ve)
  f(val);
for (const auto &val : ve)
  f(val);
```

### 2 分析：

**第一种方法**：书写太复杂并不是所有容器的迭代器都支持小于操作容易误写成 it <= ve.end()  
**第二种方法**：调用 ve.size()可能有效率的问题；调用 ve\[i\]可能有效率的问题；并不是所有的容器都支持下标操作。
下标的标准规范是无符号的数，所以使用int 是错误的。标准并未说明下标具体用何种类型说明，所以使用std::size\_t可能有可移植性的问题使用 std::vector::size\_type正确，但书写太复杂了容易误写为 i <**=** ve.size()  
**第三种方法**：相比前两种，不容易出错相比前两种，书写也简单多了如果在函数调用处写函数的话还是比较复杂  
**第四种方法**：相比前三种，不容易出错相比前三种，书写更简单了直接在函数调用处写函数也很简单需要 C++11 或之后