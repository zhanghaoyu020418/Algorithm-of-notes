# STL源码剖析（侯捷）



![image-20210515104933917](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210515104933917.png)

![image-20210515105620509](C:\Users\张昊宇\AppData\Roaming\Typora\typora-user-images\image-20210515105620509.png)

### 6大部件


```cpp
int main()
{
	int ia[6] = {27, 210, 12, 47, 109, 83};
	// 容器     分配器
	vector<int, allocator<int>> vi(ia, ia + 6);// allocator是分配器
	// 迭代器  算法                   
	cout << count_if(vi.begin(), vi.end(), 		        not1(bind2nd(less<int>(), 40)));
	return 0;
}
```

### 前闭后开
```cpp
int main()
{
	int ia[6] = {27, 210, 12, 47, 109, 83};
	vector<int> vec(ia, ia + 6);
	
	vector<int>::iterator ite = vec.begin();// 用迭代器iterator来遍历
	auto it = vec.begin();// 因为vector<int>::iteraotor每次都麻烦，所以用auto就会方便一些
	
	for (; ite != vec.end(); ite++) {
		cout << *ite << ' ';
	}
	return 0;
}
```

### 用for-based循环
```cpp
// 这样循环会比迭代器循环方便一点
for (auto& elem : vec) {
}

for (auto elem : vec) {
}
```

> 容器

**Sequence Container**
---

1.Array（数组）
2.Vector（向量）
3.Deque（双端队列）
4.List（双向链表）
5.Forward_List（单链表）



**array**
```cpp
int main()
{
	array<int, 100> a;
	srand((unsigned)time(NULL));
	for (int i = 0; i < 10; i++)
		a[i] = rand();
	for (int i = 0; i < 10; i++)
		cout << a[i] << endl;
	cout << a.data() << endl;
	return 0;
}
```

**vector**

__注意：可以用vector<int>::iterator pIt换成auto pIt__


```cpp
void test()
{
	srand((unsigned)time(NULL));
	vector<int> v;
	for (int i = 0; i < 10; i++)
		v.push_back(rand() % 200);
	vector<int>::iterator pIt = find(v.begin(), v.end(), 100);
	if (pIt != v.end())
		cout << "Find " << *pIt << endl;
	else
		cout << "No Find " << endl;
}
```

**list**

__注意：list和forward_list中有成员函数sort所以不用<algorithm>中全局函数sort，直接用list定义的对象li.sort()即可__
```cpp
void test2()
{
	srand((unsigned)time(NULL));
	list<int> li;
	for (int i = 0; i < 100000; i++)
		li.push_back(rand());
	
	clock_t timeStart = clock();
	li.sort();
	for (int i = 0; i < 100; i++)
	{
		cout << li.front() << ' ';
		li.pop_front();
	}
	cout << endl;
	clock_t timeEnd = clock();
	cout << timeEnd - timeStart << endl;
}
```
**forward_list**
和list差不多，就不说了，提交特殊的就是`forward_list`只能push_front()不能push_back()，因为froward_list是单向链表

**stack**
**queue**
这两个容器的底层源代码就是用deque进行修改的，所以一般stack和queue焦作容器的adapter



**Associative Contioners**
---



1.Set / Multiset （一般用红黑树）
2.Map / Multimap （一般用红黑树）
3.unordered_set/multiset（一般用hashtable）
4.unordered_map/multimap（一般用hashtable）

1.Set / Multiset 
2.Map / Multimap 

这两个容器底层都是红黑树


3.unordered_set/multiset
4.unordered_map/multimap

这两个容器的底层用的是哈希表，其实这四个容器的使用是差不多的
set是一个集合所以不会有重复的元素
map的key值也是不会重复的，但是value值是会重复的
multimap和multiset是会重复的而且map的insert很特殊要用part<int,int>()河阳每次用一个pair存一对值进去才行


==关联性容器一半使用来查找某个值的，所以这些容器的成员函数的find()比<algorithm>的find()函数要快==





##  OOP和GP的区别

GP中的模板的概念细节和使用（[用Xmind画的图展示](D:\github仓库\Algorithm-of-notes\SourceCode\xmind)）





