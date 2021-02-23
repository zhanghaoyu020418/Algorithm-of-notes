string 类型的长度可以用 str.size() 或者 str.length() 来求

vector类型定义的数组的长度只能用 a.size() 来求数组的长度

string类型的字符串和==结构体==要用sort(a, a + n)来排序

vector类型的数组要用迭代器sort(a.begin(), a.end())来排序



## sort函数对一维vector和二维vector排序

### 一维vector排序
```cpp
	vector<int> a;
	a = { 2,4,3,7,5,4 };
```
`	sort(a.begin(), a.end());` ==默认是降序==

==升序要在第三个参数上加一个比较函数==

```	cpp
	sort(a.begin(), a.end(), [](int a, int b){
		return a > b;
});
​```
```


### 二维vector排序
二维vector只有两列，也就是说有两个关键字，sort函数默认是对第一个关键字降序，若是要升序排列要在sort函数的第三个参数上加一个比较函数
```cpp
int main()
{
	vector<vector<int>> a;
	a = { {1,3}, {2, 5}, {6, 4}, {0, 0} };
	
	sort(a.begin(), a.end());

	//二维vector的遍历，a.size()是行数，每列只有2个元素
	for (int i = 0; i < a.size(); i++)
	{
		for (int j = 0; j < 2; j++)
			cout << a[i][j] << ' ';
		cout << endl;
	}
	return 0;
}
```
==对第二个关键字降序==
```cpp
	sort(a.begin(), a.end(), [](auto &a, auto &b) {
		return a[1] > b[1];
	});
```
==对第一个关键字降序==
```cpp
	sort(a.begin(), a.end(), [](vector<int> a, vector<int> b){
		return a[0] > b[0]
});
```

