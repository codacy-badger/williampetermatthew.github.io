---
title: 【转载】C++ 迭代器，了解一下？
date: 2018-08-27 09:42:20
tags: 
  - 洛谷日报
---


------------
# C++ 迭代器，了解一下？
------------

> 声明：本文为转载文章，转载自[洛谷日报#34](https://poi.blog.luogu.org/cpp-iterator)，原作者为Xeonacid。

------------
由于作者是个C++鶸，如果本文有任何错误，烦请不吝指出。（有的地方已知描述是不完整的，但是与指针行为一致，或鉴于多数看这篇文章的都是算法竞赛选手，抑或考虑实际，刻意忽略掉辣）

## 迭代器是个啥？

迭代器(Iterator)：一种“能够迭代某序列内所有元素”的对象

![](https://i.loli.net/2018/08/11/5b6ea9ab74ecb.jpg)

指针都知道吧？不知道的先出门左转了解一下

迭代器：指针的抽象，指针是迭代器的子集
```cpp
#include <iterator>
```
## 迭代器能干啥？

所有迭代器都能做的操作：
```cpp
int main(){
    /*
    * suppose:
    * T t;
    * container<T> v1{t}, v2;
    * container<T>::iterator
    * container<T>::begin()
    * container<T>::end()
    * have been defined here
    */
    container<T>::iterator ita = v1.begin(), itb = v2.begin();
    *ita;
    using std::swap;
    swap(ita, itb);
    ++ita;
}
```
emmmm...这些跟指针没什么差别对吧。

![思考熊](http://img.uoj.ac/utility/bear-thinking.gif)真的没差别吗？

其实是有的...

指针不可能随便解引用或者交换一下，甚至你什么都没做，就非法化了对吧。但是就“迭代器”本身，不加任何限制的情况下，其只是一个可以做上述操作的类而已啦，随时有可能被非法化。

```*it```返回什么是未指定的，返回```void```也合~~fa♂ 三去~~法，意味着在这里你可能什么也做不了2333333

@[捂脸熊](http://img.uoj.ac/utility/bear-facepalm.jpg)那我~~咬着~~要这迭代器有何用？

别急别急，迭代器还是有几个定义好的分类的，标准库内的迭代器何时会非法化也有严格限制。

在说这个之前，先提一嘴迭代器的一个辅助类型```std::iterator_traits```，其一般化定义是长这个样子滴：
```cpp
template< class Iterator >
struct iterator_traits {
    typedef typename Iterator::difference_type difference_type;
    typedef typename Iterator::value_type value_type;
    typedef typename Iterator::pointer pointer;
    typedef typename Iterator::reference reference;
    typedef typename Iterator::iterator_category iterator_category;
};
```
很简单对吧，直接从迭代器类型本身拿来了这几个成员，标准库提供了对于指针的特化，毕竟指针没有这些成员。

只有使```std::iterator_traits<T>```合法且含有上述五个成员，且能执行前面列出的那些迭代器操作的```T```类型才满足迭代器概念。

什么？你问我这五个鬼东西干啥用的？~~把他们翻译成中文就好了~~ ←真的如此，STL的命名还是挺清楚的哇

其中标准库自带了五个用于```iterator_category```的空类型，分别对应下方的前五种迭代器：
```cpp
struct output_iterator_tag { };
struct input_iterator_tag { };
struct forward_iterator_tag : public input_iterator_tag { };
struct bidirectional_iterator_tag : public forward_iterator_tag { };
struct random_access_iterator_tag : public bidirectional_iterator_tag { };
```
## 迭代器分类

- 输出迭代器(OutputIterator)
- 输入迭代器(InputIterator)
- 向前迭代器(ForwardIterator)
- 双向迭代器(BidirectionalIterator)
- 随机访问迭代器(RandomAccessIterator)
- 相接迭代器(ContiguousIterator)
- 可变迭代器(MutableIterator)

迭代器分类的依据是其可以进行的操作

![鼓掌熊](http://img.uoj.ac/utility/bear-applaud.gif)由上至下越来越像指针，~~越来越正常~~

## 输出迭代器
```cpp
typedef output_iterator_tag iterator_category;
```
可自增（前置/后置、无操作）

可```operator*```解引用（空操作）

返回的还是这个迭代器本身，所以不要做出```a = *it```这种操作，~~伦家明明是输出迭代器不要把窝搞成输入嘛~~~

实际上这个不是输出迭代器标准定义，但是STL中输出迭代器的实例就是这样的

仅资瓷单趟算法

啥是单趟算法？只需要遍历这个序列一次的算法，不需要把当前位置迭代器记存一个副本，等以后再使用。

因为该序列上同一时间可能只有一个位置的迭代器是合法的

![](https://i.loli.net/2018/08/11/5b6ecf17aea62.jpg)输出迭代器这个名字看起来就是用于输出内容的，STL有两个小玩意叫做```std::ostream_iterator```和```std::ostreambuf_iterator```，把输出流包装了一下
```cpp
// 构造一个输出T类型的输出迭代器
// 第一个参数为绑定到的流
// 第二个参数为每次输出后输出的字符串，可省
std::ostream_iterator<T> it(std::cout, " ");
T t;
it = t; //这些都等同于 std::cout << t << " "
*it = t;
it++ = t;
++it = t;
*it++ = t;

// 跟上面那个一样，不过变成输出字符类型，也没有第二个参数了
std::ostreambuf_iterator<char> it(std::cout);

it = 'A'; // 等同于 std::cout << 'A'
*it = 'A';
...
```
STL还有几个用于插入序列的迭代器适配器，以及配套的为了方便的函数模板
```cpp
std::deque<int> q;

std::back_insert_iterator< std::deque<int> > it1(q);
// 等同于 auto it1 = std::back_inserter(q);
it1 = 1; // 等同于 q.push_back(1)
*it1 = 1;
...

std::front_insert_iterator< std::deque<int> > it2(q);
// 等同于 auto it2 = std::front_inserter(q);
it2 = 1; // 等同于 q.push_front(1)
*it2 = 1;
...

std::insert_iterator< std::deque<int> > it3(q, q.begin());
// 等同于 auto it3 = std::inserter(q);
it3 = 1; // 等同于 q.insert(q.begin(), 1)
*it3 = 1;
...
```
输入迭代器

~~这玩意从名字看上去就与上面那个有许多相似之处，实际上也是~~
```cpp
typedef input_iterator_tag iterator_category;
```
可后置自增

可默认构造

实际上输入迭代器标准定义不可默认构造，向前迭代器才可以，但是STL中输入迭代器的实例都是可以的

```operator*```返回```reference```，为可转换为```value_type```的引用

可以读取啦，但是不一定可以写入，因为返回的引用可能是```const```的（下面的输入流迭代器就如此）

为什么是“可转换为```value_type```的引用”呢？

~[](https://i.loli.net/2018/08/11/5b6ecf17aea62.jpg)```std::vector<bool>```为了节约空间，每一个 0101 位占一个bit而非一个byte，但是没有办法返回一个bit的对象，只能返回一个包装好的代理类辣，所以```std::vector<bool>::iterator::reference```是代理类的引用而非位引用或```bool&```

PS:```std::vector<bool>```不是容器，不满足容器的要求，下文提到```std::vector```时均不包含```std::vector<bool>```

~~C++真是一门难学的语言~~![](https://i.loli.net/2018/08/11/5b6eca17075cd.jpg)

可比较相等和不等

```operator*```返回当前元素

可使用```operator->```访问成员

仅资瓷单趟算法

![](https://i.loli.net/2018/08/11/5b6ecf17aea62.jpg)把输出迭代器的栗子的输出改成输入就好了

emmmmm...等等，那EOF咋判断？![思考熊](http://img.uoj.ac/utility/bear-thinking.gif)

默认构造的输入流迭代器就代表EOF，判一下相等/不等就好了
```cpp
std::vector<int> v;
std::istream_iterator<int> i1(std::cin), i2;
while(i1 != i2) v.push_back(*i1++);
```
同一个位置的元素可以读多次，不过不能倒回来读
```cpp
std::istream_iterator<int> i1(std::cin);
int a = *i1, b = *i1, c = *++i1, d = *i1++; // 前提未EOF
```
```std::istreambuf_iterator```同理，只不过读入的变成了字符

![](https://i.loli.net/2018/08/12/5b6fc6e27d42a.jpg)那这个输入/输出迭代器比直接用```std::cin/cout```还麻烦啊！！！有啥用啊！！！

别急着骂我，主要是配合各种STL函数食用的

![](https://i.loli.net/2018/08/11/5b6ecf17aea62.jpg)众所周知，```std::vector```有这样一个构造函数，接收一对迭代器，将 ```[begin,end)``` 范围内元素拷贝入容器（```std::vector<>::assign()```也是）

所以上面的代码可以改写成这样
```cpp
std::vector<int> v(std::istream_iterator<int>(std::cin), std::istream_iterator<int>());
```
一行结束，简单多了对吧。输出的也同理

~~别问我这个在算法竞赛中的应用是什么，我知道你们都不会在比赛中用流式输入输出的~~，但是这些都是C++这门语言的重要组成部分，毕竟C++不是只用来算法竞赛的对吧

## 向前迭代器

~~终于到比较正常的~~用的比较多的了
```cpp
typedef forward_iterator_tag iterator_category;
```
是输入迭代器

```reference```为引用

提供多趟保证

可以放心地把迭代器存起来辣

不会因为解引用并赋值导致迭代器非法化

自增```it```的副本不改变解引用```it```得到的值

保证若```ita == itb```则

- 要么二者都不可解引用，要么指向同一对象
- ```++ita == ++itb```
可以看成不可自减和随机访问的指针

![](https://i.loli.net/2018/08/11/5b6ecf17aea62.jpg)```std::forward_list```和无序关联容器（哈希容器）的```iterator```

## 双向迭代器
```cpp
typedef bidirectional_iterator_tag iterator_category;
```
是向前迭代器

可自减

行为与自增都类似

可以看成不可随机访问的指针

![](https://i.loli.net/2018/08/11/5b6ecf17aea62.jpg)```std::list```和有序关联容器的```iterator```

## 随机访问迭代器
```cpp
typedef random_access_iterator_tag iterator_category;
```
是向前迭代器

可以在常量时间内移动任意位置

可以做加减法

可以比较大小

可以使用```operator[]```

指向数组元素的指针会用吧？一样的

![](https://i.loli.net/2018/08/11/5b6ecf17aea62.jpg)```std::vector, std::array, std::deque, std::string```的```iterator```、指向数组元素的指针

## 相接迭代器

是迭代器

其所指向的逻辑相邻元素也在内存中物理上相邻

任意合法的```*(a + n)```等价于```*(std::addressof(*a) + n)```

顺便提一句，为什么是```std::addressof(*a)```而不是```&(*a)```呢

因为```operator&```是可以重载的...它可以返回任何奇怪的东西

所以C++11引入了```std::addressof```函数，专门用于返回一个对象的地址，其实现用了一些小trick，C++11及之后所有的标准库实现取地址用的都是这个函数而不是```operator&```

那么问题来了，如何在C++11之前将一个重载了```operator&```的类放入标准库容器呢？

![](https://i.loli.net/2018/08/12/5b6fd12ac0062.jpg)

~~C++真是一门难学的语言~~![](https://i.loli.net/2018/08/11/5b6eca17075cd.jpg)

![](https://i.loli.net/2018/08/11/5b6ecf17aea62.jpg)```std::vector, std::array, std::basic_string_view```的```iterator```、指向数组元素的指针

## 可变迭代器

是输入迭代器

是输出迭代器

也就是可以读也可以写的

![](https://i.loli.net/2018/08/11/5b6ecf17aea62.jpg)所有STL容器的```iterator```（```const_iterator```除外）、指针（常量指针除外）

## 类型总结

![](https://i.loli.net/2018/08/11/5b6edd403bc5f.png)

来源cppreference

## 相对用的比较多的迭代器适配器

```std::reverse_iterator```

反向迭代器，原迭代器+变-、-变+，提供原迭代器提供的所有功能

![](https://i.loli.net/2018/08/11/5b6ecf17aea62.jpg)STL容器的```rbegin(), rend()```返回的就是这个

```std::move_iterator```

原迭代器需至少为输入迭代器，其```reference```为右值引用

关于```std::move```和右值引用...那应该是另一篇文章的内容了

迭代器非法化

See https://zh.cppreference.com/w/cpp/container#.E8.BF.AD.E4.BB.A3.E5.99.A8.E9.9D.9E.E6.B3.95.E5.8C.96

适配器均取决于其底层容器

（溜了溜了）

emmmm...还是说几个常见的吧

所有只读操作无

```std::vector```扩大重分配、```std::deque```插入+扩大重分配+非首尾擦除：全部

```std::vector```插入/擦除（无重分配）：插入位置及其后

```std::deque```首尾擦除（无重分配）：首尾

链表+有序关联容器：插入无，擦除仅被擦除元素

（毕竟它们是节点形式出现的）

哈希容器：

插入导致重哈希：全部

插入未导致重哈希：无

擦除：仅被擦除元素

## 一点废话

在不需要返回值的情况下尽可能使用前置递增/递减，而非后置

>　后置比前置慢

这个对于内置类型是假的，开不开优化都一样

![](http://img.uoj.ac/utility/emoticon-1.jpg)但是对于非内置类型就不一定了

像迭代器这种实现相对比较简单的类（绝大部分容器的迭代器底层都只是一个或几个指针），开了优化可能会被优化成一样

但是如果是复杂一点的就不一定了2333333

小建议：（求不喜勿喷）干脆全改成前置好了，反正不会慢对吧，要不然写的时候还要想一下是不是内置类型，也挺麻烦的![](http://static.tieba.baidu.com/tb/editor/images/client/image_emoticon25.png)

使用```std::array```而非原生数组

窝个人觉得，OOP的封装性的优势在这体现地淋漓尽致，不用管底层怎么搞的，用就好辣

其成员函数什么的都是STL的命名格式，会用一个STL容器就会用其他的_(:з」∠)_

一维数组还好办，二维及以上的话指针、```sizeof```什么的比封装好的麻烦多了（表喷窝TAT，再熟练也改变不了它麻烦的事实呀）

常数这个无需担心，```std::array```只是把原生数组封装了一下，效率没有任何差别，成员函数都是内联的![鼓掌熊](http://img.uoj.ac/utility/bear-applaud.gif)

然鹅...这个东西是C++11才有的，C++98的话也可以自己封装一下嘛，几分钟就写完了（逃）

参考资料

[ISO/IEC 14882:2017 Programming languages -- C++](https://www.iso.org/standard/68564.html):952-986.

[cppreference.com](https://en.cppreference.com/w/).

（德）约祖蒂斯（Josuttis,N.M.）著；侯捷译.C++标准库：第2版[M].北京：电子工业出版社，2015.6：433-474.