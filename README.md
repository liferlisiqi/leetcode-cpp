# leetcode刷题总结(C++)


# 1 basics
## 1.1 data structure

- ### vector


vector是封装动态数组的顺序容器，元素顺序存储，可以通过迭代器和指针访问元素。vector在内存中是自动管理的，按需扩张和收缩。通常占用多于静态数组的空间，以便分配更多内存用于管理将来的增长，占用空间在额外内存即将耗尽时重新分配。

基础API

```c++
#include <iostream>
#include <vector>

using namespace std;

//构造函数
vector<int> vec1; //无参数
vector<int> vec2(3); //n个元素
vector<int> vec3(3,5); //n个val值
vevtor<int> vec4(vec3); //vector对象
vector<int> vec5(vec3.begin(), vec3.end()); //迭代器

//元素访问
cout << vec1[0] << endl; //[]下标访问
cout << vec1.at(0) << endl; //at()下标访问，有越界检查
cout << vec1.front() << endl; //第一个元素
cout << vec1.back() << endl; //最后一个元素

//元素增删
vec1.push_back(3); //将元素添加到末尾
vec1.pop_back(); //移除末尾元素，没有返回值
vec1.insert(vec1.begin(), 3); //在迭代器位置前插入元素，注意insert可能导致迭代器非法话

//属性
cout << vec1.empty() << endl; //返回是否为空
cout << vec1.size() << endl; //容纳元素数量
cout << vec1.capacity() << endl; //占用存储空间可容纳元素数量
```

- ### string

c++从c继承的字符串概念仍然是以 '\0' 为结束符的char数组，STL中的std::string是std::basic_string的一个特化，可以将字符串作为一个类型，实现复制、赋值和比较等操作，不必担心内存大小和占用内存实际长度等具体问题。

基础API

```c++
#include<iostream>
#include<string>
#include<sstream>

using namespace std;

//构造函数
string str1; //无参初始化
string str2 = "abc"; //最简单的字符串初始化
char *s = "abc";
string str3(s); //用c字符串初始化
string str4(3, 'c'); //用n个字符初始化
string str5(str4); //全部复制

//标准输入
cin >> str1; //读取以空格或回车为结尾的字符序列
cout << str1 <<endl;
getline(cin, str1); //读取整行的字符，遇到分界符、回车符、文件结束符时终止操作
cout << str1 << endl;
getline(cin, str1, " "); //参数3指定分界符
cout << str1 << endl;

//元素访问
cout << str1[0] << endl; //[]返回下标位置的字符
cout << str1.at(0) << endl; //at()返回下标位置字符，提供越界检查，超出范围抛出out_of_range异常
cout << str1.front() << endl; //返回第一个字符
cout << str1.back() << endl; //返回最后一个字符
cout << str1.c_str() << endl; //返回c字符数组

//属性
cout << str1.empty() << endl; //字符串是否未空
cout << str1.size() << " " << str1.length() << endl; //字符串长度

//增，三种方法: +=, push_back, append，建议基础操作使用+=,复杂操作使用append
cout << str2 + "def" << endl; //使用+号增加字符串
cout << str2 + 'd' + 'e' + 'f' << endl; //使用+号增加字符
str2 += {'d', 'e', 'f'};
cout << str2 << endl; //使用+号增加

str2.push_back('d'); //push_back只能在结尾添加一个字符，功能单一
str2.push_back('e');
str2.push_back('f');

str2.append("def");
str2.append(3, 'd');
str2.append(s);

//查，
str3 += "bc";
cout << str3.find('a') << endl; //find()寻找首个等于给定 字符序列 的字串
cout << str3.find("bc") << endl; 
cout << str3.find_first_of("bc") << endl; //find_first_of()寻找等于给定字符序列中字符之一的首个字符                                      
```

[std::string::append vs std::string::push_back() vs Operator += in C++](https://www.geeksforgeeks.org/stdstringappend-vs-stdstringpush_back-vs-operator-c/)

- ### deque


deque（双端队列）是有下标顺序容器，允许在首尾两端快速地插入及删除，且不会非法化指向其他元素的指针或引用。deque的元素不是相接存储的，典型的实现是用单独分配的固定大小数组序列，外加额外的登记，所以下标访问需要二次指针解引用。

基础API

```c++
#include<iostream>
#include<deque>

using namespace std;

//构造函数
deque<int> que1;
deque<int> que2 = { 3,5,7,9,2,4,6,8,3 };

//元素访问,deque的访问操作慢于vector，由于内部组织元素的方式不同
cout << que2[0] << endl;
cout << que2.at(0) << endl;
cout << que2.front() << endl;
cout << que2.back() << endl;

//增删，这里是deque的独特API
que2.push_back(0); //在队尾元素增加元素
que2.pop_back(); //删除队尾元素
que2.push_front(0); //在队首增加元素
que2.pop_front(); //删除队首元素

//属性，由于存储方式不同，deque的容器大小总是和总量相等，所以没有定义capacity()
cout << que2.empty() << endl;
cout << que2.size() << endl;

```

- ### list


list（双向链表）是支持常数时间内从容器任何位置插入和移除元素的容器，不支持快速随机访问，通常实现为双向链表。在list内添加、移除和移动元素不会非法化迭代器或引用，仅在对应元素被删除时非法化。list<T>容器的每个T对象通常都被包装在一个内部节点对象中，节点维护了两个指针，一个指向前一个节点，一个指向后一个节点，这些指针将节点连接成一个链表。

基础API

```c++
#include <iostream>
#include <list>

using namespace std;

//构造函数
list<int> lst1;
list<int> lst2 = {3, 5, 7, 2, 8, 4};

//元素访问，复杂度O(n),不能通过下标访问
cout << lst1.front() << endl;
cout << lst2.back() << endl;
cout << *lst2.begin() << endl; //通过迭代器访问
cout << *(lst2.begin() + 4) << endl;
cout << *lst2.end() << endl; //指向最后一个元素下一个，直接用会报错
cout << *(--lst2.end()) << endl; //指向最后一个元素

//增删，复杂度O(1)
lst2.push_front(0); //常见的首位添加和删除操作
lst2.pop_front();
lst2.push_back(0);
lst2.pop_back();

lst2.remove(3); //按值删除元素
lst2.remove_if([](int n){return n == 5}); //按lamba表达式删除元素

lst2.insert(--lst2.end(), 8);
lst2.unique(); //比较并删除连续的重复元素，留下其中一个
```

- ### set

定义set的模板有四种：set、multiset、unordered_set、unordered_multiset。其中两种（set multiset）使用比较函数对元素排序。另外两种（unordered_set unordered_multiset）使用哈希函数来保存元素。set是关联容器，含有key类型对象的已排序集，搜索、移除和插入拥有对数复杂度，通常以红黑树实现。

基础API-set/multiset

```c++
#include <iostream>
#include <set>

using namespace std;

//构造函数
set<int> set1;
set<int> set2 = { 3, 5, 8, 1, 4, 9 };

//元素访问
auto it = set2.find(1); //如果find函数找到和参数匹配的元素，则返回对应的迭代器
cout << *it << endl;
it = set2.find(2); //如果find函数找不到匹配元素，则返回结束迭代器，下面的打印就会报错
cout << *it << endl;
cout << set2.count(4) << endl;
cout << set2.count(3) << endl;

//元素增删
auto it = set2.insert(3);
cout << *it.first << endl; //inset返回一个pair<iterator, bool>, 插入元素的迭代器
set2.erase(3); //erase删除匹配的元素
set2.erase(++set2.begin()); //删除迭代器对应的元素
```

[【C++ STL学习之五】容器set和multiset](https://blog.csdn.net/xiajun07061225/article/details/7459206?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-18&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-18)

基础API-unordered_set/unordered_multiset

```c++
#include <iostream>
#include <unordered_set>

```




- ### map

  



## 1.2 search algorithm 查找算法

| 查找算法 | 查找复杂度（平均/最坏） | 插入复杂度（平均/最坏） | 删除复杂度（平均/最坏） | 空间复杂度 |
| :------: | :------: | :------: | :------: | :------: |
| 顺序查找 | O(n/2) | O(n) | O(n/2) | O(1) |
| 二分查找 | O(logn) | O(n/2) | O(n/2) | O(nlogn) |
| 插值查找 | O(log(logn)) | O(n^2) | O(n) | O(1) |
| 二叉查找树 | O(logn) | O(logn) | O(n^0.5) | O(1) |
| 2-3树 | O(clogn) | O(clogn) | O(clogn) | O(1) |
| 红黑树 | O(logn) | O(logn) | O(logn) | O(1) |
| B树 | O(logn) | O(logn) | O(logn) | O(n) |
| 哈希函数 | O(1) | O(1) | O(1) | O(1) |

## 1.3 sort algorithm 排序算法

| 排序算法 | 平均时间复杂度 | 最坏时间复杂度 | 最好时间复杂度 | 空间复杂度 | 稳定性 |
| :------: | :------: | :------: | :------: | :------: | :------: |
| 冒泡排序 | O(n^2) | O(n^2) | O(n) | O(1) | 稳定 |
| 快速排序 | O(nlogn) | O(n^2) | O(nlogn) | O(nlogn) | 不稳定 |
| 插入排序 | O(n^2) | O(n^2) | O(n) | O(1) | 稳定 |
| 希尔排序 | O(nlogn) | O(n^1.5) | 增量序列决定 | O(1) | 不稳定 |
| 选择排序 | O(n^2) | O(n^2) | O(n^2) | O(1) | 不稳定 |
| 堆排序 | O(nlogn) | O(nlogn) | O(nlogn) | O(1) | 不稳定 |
| 归并排序 | O(nlogn) | O(nlogn) | O(nlogn) | O(n) | 稳定 |
| 计数排序 | O(n+k) | O(n+k) | O(n+k) | O(n+k) | 稳定 |
| 桶排序 | O(n+k) | O(n+k) | O(n+k) | O(n+k) | 稳定 |
| 基数排序 | O(n) | O(n) | O(n) | O(n) | 稳定 |


# 2 leetcode problem

## 2.1 Array

| 题目 | Tag | 难度 | 题解 |
| :------: | :------: | :------: | :------: |
| 最优页面置换算法（OPT） | 置换在未来最长时间不访问的页面 | 计算内存中每个逻辑页面的下次访问时间，选择未来最长时间不访问的页面 | 缺页最少，理想情况，无法实现，可作为性能评价依据 |


## 2.2 Dynamic programming

| 题目 | Tag | 难度 | 题解 |
| :------: | :------: | :------: | :------: |
| 最优页面置换算法（OPT） | 置换在未来最长时间不访问的页面 | 计算内存中每个逻辑页面的下次访问时间，选择未来最长时间不访问的页面 | 缺页最少，理想情况，无法实现，可作为性能评价依据 |


## 2.3 String

| 题目 | Tag | 难度 | 题解 |
| :------: | :------: | :------: | :------: |
| 最优页面置换算法（OPT） | 置换在未来最长时间不访问的页面 | 计算内存中每个逻辑页面的下次访问时间，选择未来最长时间不访问的页面 | 缺页最少，理想情况，无法实现，可作为性能评价依据 |


## 2.4 Math

| 题目 | Tag | 难度 | 题解 |
| :------: | :------: | :------: | :------: |
| 最优页面置换算法（OPT） | 置换在未来最长时间不访问的页面 | 计算内存中每个逻辑页面的下次访问时间，选择未来最长时间不访问的页面 | 缺页最少，理想情况，无法实现，可作为性能评价依据 |


## 2.5 Tree

| 题目 | Tag | 难度 | 题解 |
| :------: | :------: | :------: | :------: |
| 最优页面置换算法（OPT） | 置换在未来最长时间不访问的页面 | 计算内存中每个逻辑页面的下次访问时间，选择未来最长时间不访问的页面 | 缺页最少，理想情况，无法实现，可作为性能评价依据 |


## 2.6 Hash table

| 题目 | Tag | 难度 | 题解 |
| :------: | :------: | :------: | :------: |
| 最优页面置换算法（OPT） | 置换在未来最长时间不访问的页面 | 计算内存中每个逻辑页面的下次访问时间，选择未来最长时间不访问的页面 | 缺页最少，理想情况，无法实现，可作为性能评价依据 |


## 2.7 Deep-first search

| 题目 | Tag | 难度 | 题解 |
| :------: | :------: | :------: | :------: |
| 最优页面置换算法（OPT） | 置换在未来最长时间不访问的页面 | 计算内存中每个逻辑页面的下次访问时间，选择未来最长时间不访问的页面 | 缺页最少，理想情况，无法实现，可作为性能评价依据 |


## 2.8 Binary search

| 题目 | Tag | 难度 | 题解 |
| :------: | :------: | :------: | :------: |
| 最优页面置换算法（OPT） | 置换在未来最长时间不访问的页面 | 计算内存中每个逻辑页面的下次访问时间，选择未来最长时间不访问的页面 | 缺页最少，理想情况，无法实现，可作为性能评价依据 |


## 2.9 Binary search

| 题目 | Tag | 难度 | 题解 |
| :------: | :------: | :------: | :------: |
| 最优页面置换算法（OPT） | 置换在未来最长时间不访问的页面 | 计算内存中每个逻辑页面的下次访问时间，选择未来最长时间不访问的页面 | 缺页最少，理想情况，无法实现，可作为性能评价依据 |


## 2.10 Two pointers

| 题目 | Tag | 难度 | 题解 |
| :------: | :------: | :------: | :------: |
| 最优页面置换算法（OPT） | 置换在未来最长时间不访问的页面 | 计算内存中每个逻辑页面的下次访问时间，选择未来最长时间不访问的页面 | 缺页最少，理想情况，无法实现，可作为性能评价依据 |


## 2.11 Breadth-first search

| 题目 | Tag | 难度 | 题解 |
| :------: | :------: | :------: | :------: |
| 最优页面置换算法（OPT） | 置换在未来最长时间不访问的页面 | 计算内存中每个逻辑页面的下次访问时间，选择未来最长时间不访问的页面 | 缺页最少，理想情况，无法实现，可作为性能评价依据 |


## 2.12 Greedy

| 题目 | Tag | 难度 | 题解 |
| :------: | :------: | :------: | :------: |
| 最优页面置换算法（OPT） | 置换在未来最长时间不访问的页面 | 计算内存中每个逻辑页面的下次访问时间，选择未来最长时间不访问的页面 | 缺页最少，理想情况，无法实现，可作为性能评价依据 |
