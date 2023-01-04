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

(2) size\_type erase (const value\_type& val);

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
6. 如果该容器是一个associative container，使用asso\_con::erase成员函数或者remove\_copy_if结合swap等方式
7. 有一些比较特殊的容器具现，比如vector等，暂不考虑。

更多信息，可以参考《Effective STL》

综上一些信息，可以发现，STL提供给我们的“删除”语义并非真正统一，至少未达到最高层次的统一。有时候从一种容器换为另外一种容器，修修改改总少不了。

下面，提供一个统一的接口，来删除一个容器中的元素，原理较简单，使用编译器通过type deduce获知容器的类型，然后通过type traits在编译器就可以决定函数派送决定。比如，编译器知道当前容器是list，那么就会调用list:remove相关的成员函数，性能？inline当然少不了！代码来源是一个STL的教学视频上得之，做了些自以为是的简单修改，当然，我的修改可能让代码“恶”了，自己简单用了些容器做测试，程序行为正确，用了trace工具跟踪代码，足迹符合预期，当然，重在思想的运用，真正的代码使用还需要经过多次严格测试。
~~~ cpp

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

当然，既然选择了C++，就代表选择了折腾（这不也是种乐趣么！），如果容器内是raw pointer呢，你如果想删除，那还得手动去释放资源，万一又有异常发生，呃……好吧，使用auto\_ptrs，可以么？（COAP！当然，也可以冒险使用之，注意auto\_ptrs的行为特性）。嗯，使用shared_ptrs，较安全，c++ox或者boost。有时候，不得不用指针，因为我想虚多态动绑定。