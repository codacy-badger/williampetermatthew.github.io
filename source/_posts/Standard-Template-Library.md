---
title: STL的一些总结
date: 2018-09-18 19:54:40
tags: 
  - 科技
---
# stdC++ STL|C++标准STL
## Containers|容器
### 简单容器
#### pair
定义于<stl_pair.h>中，需要使用命名空间std。

pair是一个 对 ，用于存储一对类型可能不同的量。

**使用方法**是
```cpp
pair<typename A,typename B>p;//即定义了一个拥有两个可以不同类型的对
```
相当于定义了一个p[2]，其中p[0]的类型为A，p[1]的类型为B。

**访问方式**是
```cpp
p.first p.second
```
p.first相当于p[0]，p.second相当于p[1]。

**赋值方式**是
```cpp
pair<typename A,typename B>p(a,b);
//或
p=make_pair(a,b);
//或
p.first=a;
p.second=b;
```
这样就相当于把一个类型为A的量a赋值到p.first，把一个类型为B的量b复制到p.second。

**其他**

pair间支持逻辑运算符（两个pair的A、B类型必须对应相同）
```cpp
!= < <= == > >=
```
支持函数
```cpp
void swap
```
swap可以交换两个pair（两个pair的A、B类型必须对应相同），是将两个pair中的first互换，second互换。

写到其他的类型里（不过此时要注意两个'>'不要挨着否则会被判CE），例：
```cpp
vector<pair<string,int> >v;
queue<pair<int,int> >q;
```
而且，pair也支持写到pair的类型A、B里，例如：
```cpp
pair<int,pair<pair<pair<int,int>,int>,int> >p;
```
### 序列式容器
#### vector
定义于<stl_vector.h>中，需要使用命名空间std。

vector的实质就是一个数组，下标从0开始，只不过这个数组是一个空间动态的数组，一个空的vector当插入第一个数时空间为1，插入第二个数时空间为2，插入第三个数时空间为4，插入第四个数时空间为4，插入第五个数时空间为8……其空间每次需要开新的空间时倍增，所以不能直接访问无空间缓存的下标，但是可以通过初始化直接开对应大小，向后插入需要新空间时倍增。

**使用方法**是
```cpp
vector<typename A>v;
//构造函数
vector<typename A>v();
vector<typename A>v(size);//定义了一个大小为size，存储类型为A的vector（相当于v[size]）
vector<typename A>v(size,a);//定义了一个大小为size，存储类型为A，初始值为a的vector（相当于v[size]并且fill了）
vector<typename A>v(_v);//是将_v这个vector复制到v里（A必须统一）
vector<typename A>v(begin,end);//是将_v中的[begin,end)区间复制到v里，注意不是(begin,end]，begin和end都是iterator
```
首先的一个是定义了一个存储类型为A的空vector。

**访问方式**是
```cpp
v[i]
```
对，就是这样，不是说了就是一个数组么。。。

**插值方式**是
```cpp
v.push_back(a);//向数组末尾插入一个a
v.insert(it,a);//向数组it位置前插入一个a
v.insert(it,n,a);//向数组it位置前插入n个a
v.insert(it,begin,end);//向数组it位置前插入[begin,end)区间的值
```

**删值方式**是
```cpp
v.pop_back();//将数组末尾数删除
v.clear();//将数组清空
v.erase(it);//将数组it位置的数删除
v.erase(begin,end);//将数组[begin,end)区间的数删除
```

**遍历方式**是
```cpp
v.at(pos);//返回pos位置的数
v.front();//返回首位数
v.back();//返回末尾数
v.begin();//返回首位位置
v.end();//返回末尾位置
v.rbegin();//反向迭代器，相当于返回末尾位置
v.rend();//反向迭代器，相当于返回首位位置
```

**其他**

如何判断一个vector是否为空？
```cpp
v.empty();//空则返回true，非空则返回false
```
vector的大小？
```cpp
v.size();//返回已有元素个数
v.capacity();//当前vector的最大元素值
v.max_size();//返回最大容量
```
另：重新分配函数：（Cr.[剑西楼](https://blog.csdn.net/q_l_s/article/details/52946769)）
```cpp
v.resize(n);
v.resize(n,a);
v.reserve(n);
```
> resize重新分配大小，改变容器大小，并且创建对象。
> 当n小于当前size()值时候，vector首先会减少size()值 保存前n个元素，然后将超出n的元素删除(remove and destroy)。
> 当n大于当前size()值时候，vector会插入相应数量的元素，使得size()值达到n，并对这些元素进行初始化，如果调用上面的第二个resize函数，指定a，vector会用a来初始化这些新插入的元素。
> 当n大于capacity()值的时候，会自动分配重新分配内存存储空间。
> 
> reserver函数用来给vector预分配存储区大小，即capacity的值，但是没有> 给这段内存进行初始化。reserve的参数n是推荐预分配内存的大小，实际分> 配的可能等于或大于这个值，即n大于capacity的值，就会reallocate内存 capacity的值会大于或者等于n。这样，当ector调用push_back函数使得size 超过原来的默认分配的capacity值时 避免了内存重分配开销。
> 需要注意的是：reserve 函数分配出来的内存空间，只是表示vector可以利用这部分内存，但vector不能有效地访问这些内存空间，访问的时候就会出现越界现象，导致程序崩溃。

vector间支持逻辑运算符（两个vector的A类型必须对应相同）
```cpp
!= < <= == > >=
```
支持函数
```cpp
void swap
void assign(n,a)//设置vector中第n个元素的值为a
void assign(begin,end)//将vector中[begin,end)中元素设置成当前vector元素
```
swap可以交换两个vector（两个vector的A类型必须对应相同），是将两个vector所有数互换。

二维或更多维
```cpp
vector<vector<int> >v(r,vector<int>(c,0));
vector<vector<int> >v;
v.resize(r);
for(int k=0;k<r;k++)
	v[k].resize(c);
```
这便定义了一个r行$\times$c列大小的v数组。

#### list
定义于<stl_list.h>中，需要使用命名空间std。

**使用方法**是
```cpp
list<typename A>l;
//构造函数
list<typename A>l(size);//定义了一个大小为size，存储类型为A的list（相当于l[size]）
list<typename A>l(size,a);//定义了一个大小为size，存储类型为A，初始值为a的list（相当于l[size]并且fill了）
list<typename A>l(_l);//是将_l这个list复制到l里（A必须统一）
list<typename A>l(begin,end);//是将_l中的[begin,end)区间复制到l里，注意不是(begin,end]，begin和end都是iterator
```



**其他**

list间支持逻辑运算符（两个list的A类型必须相同）
```cpp
!= < <= == > >=
```
list的iterator（迭代器）间支持逻辑运算符（两个list的A类型必须相同）
```cpp
!= ==
```
支持函数
```cpp
void swap
```
swap可以交换两个list（两个list的A类型必须相同），是将两个list互换。

#### slist
#### deque(double-ended queue)
### 适配器容器
#### stack
#### queue
#### priority_queue
### 关联式容器
#### set
#### multiset
#### map
#### multimap
#### hash_set
#### hash_multiset
#### hash_map
#### hash_multimap
### 其他类型容器
#### bitset
#### valarray
## Iterators|迭代器
### utility
### iterator
### memory
## Allocator|空间配置器
## Adapters|配接器
## Algorithms|算法
### algorithm
### numeric
### functional
## Functors|仿函数
# extC++ STL|C++扩展STL

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。