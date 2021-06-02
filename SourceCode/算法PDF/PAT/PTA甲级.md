#  PTA甲级



## **[1001 A+B Format (20 分)]([题目详情 (pintia.cn)](https://pintia.cn/problem-sets/994805342720868352/problems/994805528788582400))**



```cpp
#include <iostream>
#include <algorithm>
#include <string>

using namespace std;

int main()
{
	int a, b;
	cin >> a >> b;
	int res = a + b;
	if (res < 0)// 如果结果是负数就直接直接添加一个负号，然后将数字变成正数
	{
		cout << '-';
		res = res * -1;
	}

    // 转换成字符串然后，每三个字符就添加一个逗号
	string str = to_string(res);    // 因为字符串要首先满足，从后往前的三位数字，添加负号，
	reverse(str.begin(), str.end());// 但是那样遍历太复杂，所以就先将整个字符串反转一遍
	int len = str.length();// 求出str的长度
	int index = 1;
	for (int i = 1; i <= len; i++)
	{
		if (i % 3 == 0 && i != len)
		{
			str.insert(index, ",");// 在3的整数倍都添加逗号
			index++;// 因为添加了一个逗号，所以要再++一下
		}
		index++;// 字符串的索引
	}
	reverse(str.begin(), str.end());
	cout << str << endl;
    
	return 0;
}
```





```cpp
#include <iostream>
using namespace std;

int main() {
    int a, b;
    cin >> a >> b;
    string s = to_string(a + b);
    int len = s.length();
    for (int i = 0; i < len; i++) {
        cout << s[i];
        if (s[i] == '-') continue;
        if ((i + 1) % 3 == len % 3 && i != len - 1) cout << ",";
    }
    return 0;
}
```





