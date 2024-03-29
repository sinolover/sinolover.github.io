# C++ STL 中 remove 和 erase 的区别
C++ STL中的remove和erase函数曾经让我迷惑，同样都是删除，两者有什么区别呢？

vector中的remove的作用是将等于value的元素放到vector的尾部，但并不减少vector的size

vector中erase的作用是删除掉某个位置position或一段区域（begin, end)中的元素，减少其size

list容器中的remove 成员函数，原型是void remove (const value_type& val);

他的作用是删除list中值与val相同的节点，释放该节点的资源。

而list容器中的erase成员函数，原型是iterator erase (iterator position);

作用是删除position位置的节点。这也是与remove不同的地方。

考虑到list::erase是与位置有关，故erase还存在API:   iterator erase (iterator first, iterator last);

对于set来说，只有erase API，没有remove API。 erase 的作用是把符合要求的元素都删掉。

(1) void erase (iterator position);

(2) size_type erase (const value_type& val);

(3) void erase (iterator first, iterator last);

综上所述，erase一般是要释放资源，真正删除元素的，

而remove主要用在vector中，用于将不符合要求的元素移到容器尾部，而并不删除不符合要求的元素

***

原文链接：http://hi.baidu.com/tkzlpocleodtxzr/item/3a3a6037fdc8460cceb9fe86

vector中erase是真正删除了元素， 迭代器访问不到了。 algorithm中的remove只是简单的把要remove的元素移到了容器最后面，然后其余元素前移，迭代器还是可以访问到的。因为algorithm通过迭代器操作，不知道容器的内部结构，所以无法做到真正删除。

remove并不真正从容器中删除元素（容器大小并未改变），而是将每一个与value不相等的元素轮番赋值给first之后的空间，返回值FowardIterator 标示出重新整理后的最后元素的下一个位置。所以可以有以下操作：

vector array;

array.erase(remove(array.begin(),array.end(),6),array.end());

删除数组中所有元素等于6的元素

***

原文链接：http://www.cnblogs.com/painful/archive/2011/08/16/2140704.html

C++的STL通过iterator将container和algorithm分离，并通过functor提供高可定制性。iterator可以看作是一种契约，algorithm对iterator进行操作，algorithm很难对container进行直接操作，这是因为algorithm对container所知甚少，一段代码，若未利用操作对象所知全部信息，将难以达到性能之极，并伴随其它种种折中现象。当然，这种“未知性”是必须的——algorithm对于真正的操作对象container不能做出太多假设，若假设过多，何来一个algorithm可以作用若干不同container的妙举，STL强大威力也将受损不少。

啰嗦几句，开个小头，转入正题。 先给出几个关于STL中erase和remove(remove_if等，下称remove类函数)的事实，小小复习：

1. erase一般作为一个container的成员函数，是真正删除的元素，是物理上的删除
2. 作为算法部分的remove类函数，是逻辑上的删除，将被删除的元素移动到容器末尾，然后返回新的末尾，此时容器的size不变化
3. 部分容器提供remove类成员函数，那么代表的是真正物理意义上的删除元素
4. 如果该容器是vector、string或者deque，使用erase-remove idiom或者erase-remove_if idiom
5. 如果该容器是list，使用list::remove或者list:remove_if成员函数
6. 如果该容器是一个associative container，使用asso_con::erase成员函数或者remove_copy_if结合swap等方式
7. 有一些比较特殊的容器具现，比如vector等，暂不考虑。

更多信息，可以参考《Effective STL》

综上一些信息，可以发现，STL提供给我们的“删除”语义并非真正统一，至少未达到最高层次的统一。有时候从一种容器换为另外一种容器，修修改改总少不了。

下面，提供一个统一的接口，来删除一个容器中的元素，原理较简单，使用编译器通过type deduce获知容器的类型，然后通过type traits在编译器就可以决定函数派送决定。比如，编译器知道当前容器是list，那么就会调用list:remove相关的成员函数，性能？inline当然少不了！代码来源是一个STL的教学视频上得之，做了些自以为是的简单修改，当然，我的修改可能让代码“恶”了，自己简单用了些容器做测试，程序行为正确，用了trace工具跟踪代码，足迹符合预期，当然，重在思想的运用，真正的代码使用还需要经过多次严格测试。
~~~cpp

//
//Source code originally from MSDN Channel 9 Video
//Modified by techmush
//NOTE: the original code may be perfect, the modified version may be buggy!
//Modifies: add string container, add some template parameters, alert some name
// add some notes, code style.
//

#pragma once

#ifndef erasecontainer_h__
#define erasecontainer_h__

#include
#include
#include
#include
#include
#include
#include
#include //string "as" a vector
#include
#include

namespace techmush
{
namespace detail
{
//erasing behavior like vector: vector, queue, string
struct vector_like_tag
{
};

//erasing behavior like list: list, forward_list
struct list_like_tag
{
};

//erasing behaviod like set: set, map, multiset, multimap, unordered_set, unordered_map
//unordered_multiset, unordered_multimap
struct associative_like_tag
{
};

//type traits for containers
template struct container_traits;

template
struct container_traits >
{
typedef vector_like_tag container_category;
};

template
struct container_traits >
{
typedef vector_like_tag container_category;
};

//full specialization traits for string
template <> struct container_traits
{
typedef vector_like_tag container_category;
};


template
struct container_traits >
{
typedef list_like_tag container_category;
};

template
struct container_traits >
{
typedef list_like_tag container_category;
};

template
struct container_traits >
{
typedef associative_like_tag container_category;

};

//If a multiset contains duplicates, you can't use erase()
//to remove only the first element of these duplicates.
template
struct container_traits >
{
typedef associative_like_tag container_category;
};

template
struct container_traits >
{
typedef associative_like_tag container_category;
};

template
struct container_traits >
{
typedef associative_like_tag container_category;
};

template
struct container_traits >
{
typedef associative_like_tag container_category;
};

template
struct container_traits >
{
typedef associative_like_tag container_category;
};

template
struct container_traits >
{
typedef associative_like_tag container_category;
};

template
struct container_traits >
{
typedef associative_like_tag container_category;
};


//for vector-like containers, use the erase-remove idiom
template
inline void erase_helper(Cont& c, const Elem& x, vector_like_tag /*ignored*/)
{
c.erase(std::remove(c.begin(), c.end(), x), c.end());
}

//for vector-like containers, use the erase-remove_if idiom
template
inline void erase_if_helper(Cont& c, Pred p, vector_like_tag)
{
c.erase(std::remove_if(c.begin(), c.end(), p), c.end());
}

//for list-like containers, use the remove member-function
template
inline void erase_helper(Cont& c, const Elem& x, list_like_tag)
{
c.remove(x);
}

//for list-like containers, use the remove_if member-function
template
inline void erase_if_helper(Cont& c, Pred p, list_like_tag)
{
c.remove_if(p);
}

//for associative containers, use the erase member-function
template
inline void erase_helper(Cont& c, const Elem& x, associative_like_tag)
{
c.erase(x);
}

//When an element of a container is erased, all iterators that point to that
//element are invalidated. Once c.erase(it) reuturns, it has been invalidated.
template
inline void erase_if_helper(Cont& c, Pred p, associative_like_tag)
{
for (auto it = c.begin(); it != c.end(); /*nothing*/)
{
if (p(*it))
c.erase(it++); //Rebalance the tree
//Must have an iterator to the next element
else //of c before call erase
++ it;
}
}
}

//Interface function for erase
template
inline void erase(Cont& c, const Elem& x)
{
detail::erase_helper(c, x, typename /*a type*/detail::container_traits::container_category());
}


//Interface function for erase_if
template
inline void erase_if(Cont& c, Pred p)
{
detail::erase_if_helper(c, p, typename detail::container_traits::container_category());
}
}
#endif // erasecontainer_h__

~~~

当然，既然选择了C++，就代表选择了折腾（这不也是种乐趣么！），如果容器内是raw pointer呢，你如果想删除，那还得手动去释放资源，万一又有异常发生，呃……好吧，使用auto_ptrs，可以么？（COAP！当然，也可以冒险使用之，注意auto_ptrs的行为特性）。嗯，使用shared_ptrs，较安全，c++ox或者boost。有时候，不得不用指针，因为我想虚多态动绑定。
# remove erase 区别_优秀
remove:用来“删除”从begin到end之间值为x的元素。例子中是“删除”从begin到end之间值为10的元素。确切的说remove翻译为移动比较好，因为这个函数不会改变vector中的元素，也不会破坏“删除”的元素，而是把所有没有“删除”的元素挪到了vector的前端。这个函数返回一个迭代器，指向vector中最后一个没有删除的元素后面的位置。

remove_if:“删除”那些满足条件的元素，需要有一个一元谓词函数。

remove_copy:例子中就是先去做移除10这个元素的操作，然后复制copy到c.begin()的位置。

remove_copy_if:“删除”那些满足条件的元素，再做拷贝。例子中是先移除所有大于9的元素，然后拷贝到c2.begin（）的位置。

源代码如下：

```cpp
// Fig22_28_STL_Alg_remove_remove_if_remove_copy_remove_copy_if.cpp : 此文件包含 "main" 函数。程序执行将在此处开始并结束。
///Standard Library functions remove ,remove_if,remove_copy,remove_copy_if

#include <iostream>
#include <algorithm>
#include <vector>
#include <iterator>
using namespace std;
bool greater9(int);//prototype

int main()
{
    //std::cout << "Hello World!n";
    const int SIZE = 10;
    int a[SIZE] = { 10,2,10,4,16,6,14,8,12,10 };
    ostream_iterator<int> output(cout," ");
    vector<int> v(a,a+SIZE);//copy of a
    vector<int>::iterator newLastElement;

    cout << "Vector v before removing all 10s:n";
    copy(v.begin(), v.end(),output);

    //remove all 10s from v
    newLastElement = remove(v.begin(),v.end(),10);
    cout << "n Vector v after removing all 10s:n";
    copy(v.begin(), newLastElement, output);
    cout << "n";
    copy(v.begin(),v.end(), output);//后面的元素就不一定是什么了


    vector<int> v2(a,a+SIZE);//copy of a
    vector<int> c(SIZE,0);//实例化 vector c
    cout << "nVector v2 before removing all 10s and copying:n";
    copy(v2.begin(), v2.end(), output);

    //copy from v2 to c,removing 10s in the process
    remove_copy(v2.begin(), v2.end(),c.begin(),10);//先是把begin----end之间的10给remove了，然后复制到c中
    cout << "nVector c after removing all 10 s from v2:n";
    copy(c.begin(), c.end(), output);

    vector<int> v3(a, a + SIZE);//copy of a
    cout << "nVector v3 before removing all elements greater than 9:n";
    copy(v3.begin(), v3.end(), output);

    //removing elements greater than 9 from v3
    newLastElement = remove_if(v3.begin(),v3.end(),greater9);
    cout << "nVector v3 after removing all elements greater than 9:n";
    copy(v3.begin(), newLastElement, output);
    cout << "n";
    copy(v3.begin(), v3.end(), output);

    ///
    vector<int> v4(a, a + SIZE);//copy of a
    vector<int> c2(SIZE, 0);//实例化 vector c2
    cout << "nVector v4 before removing all elements greater than 9 and copying:n";
    copy(v4.begin(), v4.end(), output);

    //copy elements from v4 to c2,removing elements greater than 9 in the process
    remove_copy_if(v4.begin(),v4.end(),c2.begin(),greater9);
    cout << "nVector c2 after removing all elements greater than 9 and copying:n";
    copy(c2.begin(), c2.end(), output);
    cout << endl;

}
//determine weather argument is greater than 9
bool greater9(int x)
{
    return x > 9;
}

```

运行结果：

```cpp
Vector v before removing all 10s:
10 2 10 4 16 6 14 8 12 10
 Vector v after removing all 10s:
2 4 16 6 14 8 12
2 4 16 6 14 8 12 8 12 10
Vector v2 before removing all 10s and copying:
10 2 10 4 16 6 14 8 12 10
Vector c after removing all 10 s from v2:
2 4 16 6 14 8 12 0 0 0
Vector v3 before removing all elements greater than 9:
10 2 10 4 16 6 14 8 12 10
Vector v3 after removing all elements greater than 9:
2 4 6 8
2 4 6 8 16 6 14 8 12 10
Vector v4 before removing all elements greater than 9 and copying:
10 2 10 4 16 6 14 8 12 10
Vector c2 after removing all elements greater than 9 and copying:
2 4 6 8 0 0 0 0 0 0
```