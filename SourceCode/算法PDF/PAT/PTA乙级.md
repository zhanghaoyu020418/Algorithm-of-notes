# PTA乙级



### **1001 害死人不偿命的(3n+1)猜想 (15 分)**

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int main ()
{
    int num;
    cin >> num;
    int ans = 0;
    while (num != 1)
    {
        if (num & 1)
            num = (3 * num + 1) / 2;
        else
            num /= 2;
        ans ++;
    }
    cout << ans << endl;
    return 0;
}
```





### **1002 写出这个数 (20 分)**

```cpp
#include <iostream>
#include <string>
#include <cstring>

using namespace std;

string s[10] = {"ling", "yi", "er", "san", "si", "wu", "liu", "qi", "ba", "jiu"};

int main ()
{
    string str;
    cin >> str;
    int sum = 0;
    // stio是将字符串转换为int类型的函数不能直接stoi(str[i])因为这样是转换一个字符了，所以用substr()函数
    // substr()函数，可以这样用，第一个参数是字符串中的位置，第二个参数是位置后面的多少范围内的字符串内容
    for (int i = 0; i < str.size(); i ++ ) {
        sum += stoi(str.substr(i, 1));
    }
    
    string num = to_string(sum);
    for (int i = 0; i < num.size(); i ++) {
        if (i) 
            cout << ' ';
        cout << s[num[i] - '0'];
    }
    
    return 0;
}
```





### **1003 我要通过！ (20 分)**

```cpp
```



### **1004 成绩排名 (20 分)**

（排序）

```cpp
#include <iostream>
#include <string>
#include <algorithm>

using namespace std;

const int N = 100010;

struct Stu
{
    string name;
    string id;
    int s;
    bool operator< (const Stu& stu) {
        return s > stu.s;
    }
}stu[N];

int main ()
{
    int n;
    cin >> n;
    for (int i = 0; i < n; i ++ ) {
        cin >> stu[i].name >> stu[i].id >> stu[i].s;
    }
    sort(stu, stu + n);
    cout << stu[0].name << ' ' << stu[0].id << endl;
    cout << stu[n - 1].name << ' ' << stu[n - 1].id << endl;
    return 0;
}
```

### 1005 继续(3n+1)猜想 (25 分)

（模拟）（哈希表）

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 100010;
int a[N];
int num[N];

int main ()
{
    int cnt;
    cin >> cnt;
    for (int i = 0; i < cnt; i ++ ){
        cin >> num[i];
    }
    
    for (int i = 0; i < cnt; i ++){
        int tmp = num[i];// 用tmp暂时存放num[i]
        while (tmp != 1) {
            a[tmp] ++;// 将tmp在变成1的过程中所欲的数存放在数组的对应位置累加起来
            if (tmp & 1) {
                tmp = (3 * tmp + 1) / 2;
            } else {
                tmp /= 2;
            }
        }
    }
    
    vector<int> ans;
    for (int i = 0; i <= cnt; i ++ ) {
        if (a[num[i]] == 1) // 如果num[i]对应的位置上的数，只出现了一次说明是关键数
            ans.push_back(num[i]);
    }
    sort(ans.begin(), ans.end(), greater<int>());// 从大到小排序
    for (int i = 0; i < ans.size() - 1; i ++) {
        cout << ans[i] << ' ';
    }
     cout << ans[ans.size() - 1] << endl;
    return 0;
}
```

### 1006 换个格式输出整数 (15 分)

（模拟）

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int main ()
{
    int num;
    cin >> num;
    int n1 = num / 100;// 取出百位
    int n2 = num / 10 % 10;// 取出十位
    int n3 = num % 10;// 取出个位
    for (int i = 0; i < n1; i ++) {
        cout << "B";
    }
    for (int i = 0; i < n2; i ++) {
        cout << "S";
    }
    for (int i = 1; i <= n3; i ++) {
        cout << i;
    }
    cout << endl;
    return 0;
}
```



### **1007 素数对猜想 (20 分)**

（数学）（筛素数）

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;
int prime[N];
bool st[N];

void getPrimes(int n) {// 获取2~n之间的素数表
    int cnt = 0;
    for (int i = 2; i <= n; i ++) {
        if (!st[i]) {
            prime[cnt ++] = i;
            for (int j = i + i; j < N; j += i) {
                st[j] = true;
            }
        }
    }
}

int main () {
    int n;
    cin >> n;
    getPrime(n);
    
    int i = 0;
    int ans = 0;
    while (prime[i + 1] <= n) {
        if (prime[i + 1] - prime[i] == 2) {// 因为素数之间的插一定>1
            ans ++;                        // 所以只要比较前后两个素数的插是否=2
        }
        i ++;
    }
    cout << ans << endl;
    return 0;
}
```



### ***1008 数组元素循环右移问题 (20 分)**

（字符串处理）

```cpp
#include <iostream>
#include <deque>
#include <algorithm>

using namespace std;

const int N = 110;

int a[N];

int main () {
    int N, M;
    cin >> N >> M;
    M %= N;// M可能>=N，但是当M>=N的时候其实就没有必要右旋这么多位了，直接预处理一下M %= N
    deque<int> ans;
    for (int i = 0; i < N; i ++) {
        cin >> a[i];
        if (i < N - M) {  // 将前面N-M个数从后面插入ans中
            ans.push_back(a[i]);
        } else {
            ans.push_front(a[i]);// 将右旋M位的数字从前面插入到ans中
        }
    }
    reverse(ans.begin(), ans.begin() + M);// 因为从后面插入是反向的，所以将前面的M位反转一下
    for (int i = 0; i < N; i ++) {
        if (i)
            cout << ' ';
        cout << ans[i];
    }
    cout << endl;
}
```



```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 110;

int main () {
    int N, M;
    cin >> N >> M;
    M %= N;// M可能>=N，但是当M>=N的时候其实就没有必要右旋这么多位了，直接预处理一下M %= N
    vector<int> ans;
    for (int i = 0; i < N; i ++) {
        int num;
        cin >> num;
        ans.push_back(num);
    }
    
    reverse(ans.begin(), ans.begin() + N - M);// 将前面的N-M个数反转
    reverse(ans.begin() + N - M, ans.end());// 将后面的数都反转
    reverse(ans.begin(), ans.end());// 反转整个数组
    
    for (int i = 0; i < N; i ++) {
        if (i)
            cout << ' ';
        cout << ans[i];
    }
    cout << endl;
}
```



### ***1009 说反话 (20 分)**

（字符串）（双指针）

```cpp
#include <iostream>
#include <algorithm>
#include <string>

using namespace std;

int main () {
    string str;
    getline(cin, str);// 读入一行字符串
    reverse(str.begin(), str.end());// 反转一行的字符串
    
    int len = str.size();
    for (int i = 0; i < len; i ++) {
        int j = i;
        while (j < len && str[j] != ' ')// 如果j指针没有走到空格或者字符串结尾的话就一直往后走
            j ++;
        reverse(str.begin() + i, str.begin() + j);// 反转一小段字符串
        i = j;// 更新i的位置
    }
    
    cout << str << endl;
    return 0;
}
```



```cpp
#include <iostream>
#include <algorithm>
#include <stack>

using namespace std;

int main () 
{
    stack<string> ans;
    string str;
    while (cin >> str)// 输入字符串然后入栈，因为入栈后，再从栈中取出，天然就是逆序的
        ans.push(str);
    
    cout << ans.top();// 先出栈一个字符串
    ans.pop();
    while (!ans.empty())// 当栈不空就一直出栈
    {
        cout << ' ' << ans.top();
        ans.pop();
    }
    cout << endl;
    return 0;
}
```







### ***1035 插入与归并 (25 分)**

（排序）

1）如何判断一个数组是插入排序的中间过程

2）如何用sort()函数模拟实现非递归归并排序

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int main() {
    int n, a[100], b[100], i, j;
    
    cin >> n;
    for (int i = 0; i < n; i++)
        cin >> a[i];
    for (int i = 0; i < n; i++)
        cin >> b[i];
    
    // 判断是不是插入排序
    for (i = 0; i < n - 1 && b[i] <= b[i + 1]; i++);
    for (j = i + 1; a[j] == b[j] && j < n; j++);
    
    if (j == n) 
    {
        cout << "Insertion Sort" << endl;
        sort(a, a + i + 2);// 将前i+1个数排好序
    } 
    else // 不是插入排序就是归并排序
    {
        cout << "Merge Sort" << endl;
        int k = 1, flag = 1;
        while(flag) 
        {
            flag = 0;
            // 判断是否和b数组一样，如果一样就退出，不一样就在进行一轮归并，知道和b数相等为止
            for (i = 0; i < n; i++) 
            {
                if (a[i] != b[i])
                    flag = 1;
            }
            
            k = k * 2;// 每个归并的小区间的长度扩大两倍
            
            for (i = 0; i < n / k; i++)
                sort(a + i * k, a + (i + 1) * k);// 用sort函数直接模拟了递归版本的归并的一层递归合并两个数组的过程
            sort(a + n / k * k, a + n);// 因为数组中的个数不一定数k的倍数，所以要将最后的在k的最大倍数的后面的数排序
        }
    }
    
    // 打印数组
    for (j = 0; j < n; j++) 
    {
        if (j)
            cout << ' ';
        cout << a[j];
    }
    cout << endl;
    return 0;
}
```





### ***1045 快速排序 (25 分)**

（排序）

==**超时版本O(N^2^)**==

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 100010;
int a[N];
int n;

bool isMain(int* a, int index)// a[index]当做key值
{
    int l = 0, r = n - 1;
    while (r > index) 
    {
        if (a[r] < a[index])
            return false;
        r --;
    }
    while (l < index)
    {
        if (a[l] > a[index])
            return false;
        l ++;
    }
    return true;
}

int main ()
{
    cin >> n;
    for (int i = 0; i < n; i ++ )
        cin >> a[i];
    
    int res = 0;
    vector<int> ans;
    for (int i = 0; i < n; i ++ )
        if (isMain(a, i))// 判断a[i]是不是主元
        {
            res ++;
            ans.push_back(a[i]);
        }
    cout << res << endl;
    for (int i = 0; i < ans.size(); i ++ )
    {
        if (i)
            cout << ' ';
        cout << ans[i];
    }
    cout << endl;
    return 0;
}
```



优化版

只要和排好序的数组比较即可，满足两个要求

1）数值要在自己的位置上

2）保证左边的数都比自己要小，（吐过条件满足了，那右边自然就都比这个数要大）

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 100010;
int a[N], b[N];
int n;

int main ()
{
    cin >> n;
    for (int i = 0; i < n; i ++ )
    {
        cin >> a[i];
        b[i] = a[i];
    }
    
    // 只要数字已经在自己的对应位置上，就主元了
    // 所以只要用辅助数组排序之后，比对原数组就可以了
    sort(b, b + n);
    
    int maxVal = 0;// 当前数左边的最大的数
    vector<int> ans;
    for (int i = 0; i < n; i ++)
    {
        if (a[i] == b[i] && a[i] > maxVal)// 如果当前的数和排好序的数相同并且大于它左边的最大数，说明是答案
            ans.push_back(a[i]);
        if (maxVal < a[i])
            maxVal = a[i];
    }
    
    cout << ans.size() << endl;
    for (int i = 0; i < ans.size(); i ++ )
    {
        if (i)
            cout << ' ';
        cout << ans[i];
    }
    cout << endl;
    return 0;
}
```

### 1010 一元多项式求导 (25 分)

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int main ()
{
    vector<int> ans;
    int a, b;
    while (cin >> a >> b) 
    {
        if (b != 0)// 如果指数不为0就将导数求出
        {
            ans.push_back(a * b);
            ans.push_back(b - 1);
        }
    }
    for (int i = 0; i < ans.size(); i ++)
    {
        if (i)
            cout << ' ';
        cout << ans[i];
    }
    if (!ans.size())// 如果所有数的系数和指数都为0，也就是ans中没有元素
        cout << "0 0";
    cout << endl;
    return 0;
}
```



```cpp
#include <iostream>

using namespace std;

int main ()
{
    int a, b, flag = 0;
    while (cin >> a >> b)
    {
        if (b != 0)// 注意x的零次方就是一个常数，然后在求导就变成0了，所以就不用算了
        {
            if (flag)// 只有输出过值。才输出空格
                cout << ' ';
            cout << a * b << ' ' << b - 1;
            flag = 1;
        }
    }
    if (flag == 0)// 如果整个表达式都是0，就是零多项式
        cout << "0 0";
    cout << endl;
    return 0;
}
```

### 1011 A+B 和 C (15 分)

```cpp
#include <iostream>

using namespace std;

int main ()
{
    int n;
    cin >> n;
    int index = 1;
    while (n -- )
    {
        long long a, b, c;// 注意要用long long因为两个数最大数都是int的边界，所以当两个数相加的时候会爆int
        cin >> a >> b >> c;
        printf("Case #%d: ", index);
        if (a + b > c)
            cout << "true" << endl;
        else
            cout << "false" << endl;
        index ++;
    }
    return 0;
}
```

### *1012 数字分类 (20 分)

```cpp
#include <iostream>
#include <vector>
using namespace std;
int main() {
    int n, A1 = 0, A2 = 0, A5 = 0;
    double A4 = 0.0;
    cin >> n;
    vector<int> v[5];// 数组的第一维是存放余数，第二维这个数
    
    for (int i = 0; i < n; i++) 
    {
        int num;
        cin >> num;
        v[num % 5].push_back(num);
    }
    
    for (int i = 0; i < 5; i++) 
    {
        for (int j = 0; j < v[i].size(); j++) 
        {
            if (i == 0 && v[i][j] % 2 == 0) A1 += v[i][j];
            if (i == 1 && j % 2 == 0) A2 += v[i][j];// 用j的奇偶来正负加减
            if (i == 1 && j % 2 == 1) A2 -= v[i][j];
            if (i == 3) A4 += v[i][j];
            if (i == 4 && v[i][j] > A5) A5 = v[i][j];
        }
    }
    
    for (int i = 0; i < 5; i++) 
    {
        if (i != 0) printf(" ");
        // 因为余数为0的数可能是奇也可是偶数，所以A1要特判，其余的数直接看该数组这一行有没有数字即可
        if ((i == 0 && A1 == 0) || (i != 0 && v[i].size() == 0))
        {
            printf("N"); 
            continue;
        }
        if (i == 0) printf("%d", A1);
        if (i == 1) printf("%d", A2);
        if (i == 2) printf("%d", v[2].size());
        if (i == 3) printf("%.1f", A4 / v[3].size());
        if (i == 4) printf("%d", A5);
    }
    return 0;
}
```

### **1013 数素数 (20 分)**

（数学）（筛素数）

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 10010;

bool isprime(int n)
{
    for (int i = 2; i <= n / i; i ++)
        if (n % i == 0)
            return false;
    return true;
}

int main()
{
	int l, r;
	cin >> l >> r;
    int cnt = 0, num = 2;
    vector<int> v;// 存放范围内的素数
    while (cnt < r)
    {
        if (isprime(num))
        {
            cnt ++;
            if (cnt >= l) v.push_back(num);
        }
        num ++;
    }
    
    cnt = 0;
    for (int i = 0; i < v.size(); i ++)
    {
        cnt ++;
        if (cnt % 10 != 1)// 不是第一个数就输出空格
            cout << ' ';
        cout << v[i];
        if (cnt % 10 == 0)// 如果这一行已经10个数了，就要换行
            cout << endl;
    }
	return 0;
}
```

### **1014 福尔摩斯的约会 (20 分)**

（打表）

（字符串处理）

（isdigit()函数的使用和isalpha()函数的使用）

```cpp
#include <iostream>
#include <algorithm>
#include <string>

using namespace std;

string weekday[7] = { "MON", "TUE", "WED", "THU", "FRI", "SAT", "SUN" }; // 打表星期

int main()
{
	string str[4];
	for (int i = 0; i < 4; i++) {
		cin >> str[i];
	}
    // 处理前两个字符串
	char s[2];
	int index = 0;
	for (int i = 0; i < str[0].size() && str[1].size(); i++)
	{
		if (index == 0)
		{
			if (str[0][i] == str[1][i] && (str[0][i] <= 'G' && str[0][i] >= 'A'))// 相同大写英文字符
				s[index++] = str[0][i];
		}
		else if (index == 1)
		{
			if (str[0][i] == str[1][i]
				&& ((str[0][i] <= 'N' && str[0][i] >= 'A') || isdigit(str[0][i])))// 相同的字符
				s[index++] = str[0][i];
		}
		else if (index == 2)
			break;
	}
    // 出来后两个字符串
	int sec;
	for (sec = 0; sec < str[2].size() && sec < str[3].size(); sec++)
	{
		if (str[2][sec] == str[3][sec] && isalpha(str[2][sec]))// 相同英文字符的位置
			break;
	}
    // 输出结果
	int t = s[1] <= '9' ? s[1] - '0' : s[1] - 'A' + 10;
	cout << weekday[(s[0] - 'A')];
	printf(" %02d:%02d", t, sec);
	return 0;
}
```

### ***1015 德才论 (25 分)**

注意：struct中重载<比cmp()函数要慢，所以要使用重载<会超时

当数组中的数要按多种情况排序的时候就要重载小于号或者写一个cmp比较函数

但是这题还要分类排序，在不同的类别中排序，所以就要再在结构体中设置一个权重，来达到分类的效果

（排序）

（分类排序）

```cpp
#include <iostream>
#include <algorithm>
#include <string>

using namespace std;

const int N = 100010;
int n, l, h;

struct Stu
{
    string id;
    int d, c;
    int flag;
    //1) 德才都>=H,按总分排序
    //2) 德到了，但是才没有到
    //3) 德才都没到H,德的权重比才大
    // 总的来说就是按总分排序，然后德分比才分大
}stu[N];

int cmp(Stu p1, Stu p2)
{
    if (p1.flag != p2.flag)// 类别排序
        return p1.flag < p2.flag;
    int sum = p1.d + p1.c, sums = p2.d + p2.c;
    if (sum != sums) return sum > sums;// 总分大排序
    if (p1.d != p2.d) return p1.d > p2.d;// 总分相同按德分排序
    return p1.id < p2.id;// 按学号排序
}

int main ()
{
    cin >> n >> l >> h;
    int cnt = 0;
    for (int i = 0; i < n; i ++)
    {
        cin >> stu[i].id >> stu[i].d >> stu[i].c;
        if (stu[i].d >= l && stu[i].c >= l)
        {
            cnt ++;
            if (stu[i].d >= h && stu[i].c >= h) // 才德全尽
                stu[i].flag = 1;
            else if (stu[i].d >= h)// 德胜才
                stu[i].flag = 2;
            else if (stu[i].d >= stu[i].c)// 才德兼亡”但尚有“德胜才”
                stu[i].flag = 3;
            else 
                stu[i].flag = 4;
        }
        else 
            stu[i].flag = 5;
    }
    
    sort(stu, stu + n, cmp);
    
    cout << cnt << endl;
    for (int i = 0; i < cnt; i ++)
    {
        cout << stu[i].id << ' ' << stu[i].d << ' ' << stu[i].c << endl;
    }
    
    return 0;
}

```



### **1016 部分A+B (15 分)**

（模拟）（字符串处理）



```cpp
#include <iostream>
#include <algorithm>
#include <string>

using namespace std;

int main()
{
	string str1, str2;
	int n1, n2;
	cin >> str1 >> n1 >> str2 >> n2;
	int res1 = 0, res2 = 0;
	for (int i = 0; i < str1.size(); i++)
	{
		if ((str1[i] - '0') == n1)// 遇到相同数字就累加
			res1 = res1 * 10 + n1;
	}
	for (int i = 0; i < str2.size(); i++)
	{
		if ((str2[i] - '0') == n2)
			res2 = res2 * 10 + n2;
	}
	cout << res1 + res2 << endl;
	return 0;
}
```

### ***1017 A除以B (20 分)**

（大数除法）

```cpp
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

vector<int> div(vector<int> num, int b, int& r)// 大数除法
{
    vector<int> ans;
    for (int i = 0; i < num.size(); i ++)
    {
        r = r * 10 + num[i];// 每次将数字累加到余数r身上
        ans.push_back(r / b);// 将除完的结果放到数组中
        r %= b;// 余数的位数变高一位
    }
    // 去掉前导0
    reverse(ans.begin(), ans.end());// 倒置一下这样可以利用pop_back()来使前导变成后导
    while (ans.size() > 1 && ans.back() == 0) ans.pop_back();
    reverse(ans.begin(), ans.end());
    return ans;
}

int main ()
{
    string str;
    int b;
    cin >> str >> b;
    vector<int> num;
    for (int i = 0; i < str.size(); i ++)// 将字符串转换为数组存放
        num.push_back(str[i] - '0');
    
    int r = 0;// 余数
    vector<int> ans = div(num, b, r);// 大数除法
    
    // 输出
    for (int i = 0; i < ans.size(); i ++ )
        cout << ans[i];
    cout << ' ' << r << endl;
    return 0;
}
```

### ***1018 锤子剪刀布 (20 分)**

（循环相克）（模拟）

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

int tA[3], tB[3];// A和B的输赢次数
int hA[3], hB[3];// 赢在哪个动作上

int change(char c)
{
    if (c == 'B') return 0;
    else if (c == 'C') return 1;
    else return 2;
}

int main ()
{
    char ch[3] = {'B', 'C', 'J'};
    int n;
    cin >> n;
    for (int i = 0; i < n; i ++)
    {
        char a, b;
        cin >> a >> b;
        int ch1 = change(a);
        int ch2 = change(b);
        // 判断输赢
        if ((ch1 + 1) % 3 == ch2)
        {
            tA[0] ++;
            tB[2] ++;
            hA[ch1] ++;
        }
        else if (ch1 == ch2)
        {
            tA[1] ++;
            tB[1] ++;
        }
        else 
        {
            tA[2] ++;
            tB[0] ++;
            hB[ch2] ++;
        }
    }
    cout << tA[0] << ' ' << tA[1] << ' ' << tA[2] << endl;
    cout << tB[0] << ' ' << tB[1] << ' ' << tB[2] << endl;
    
    int maxi1 = 0, maxi2 = 0;
    for (int i = 0; i < 3; i ++)
    {
        if (hA[i] > hA[maxi1])
            maxi1 = i;
        if (hB[i] > hB[maxi2])
            maxi2 = i;
    }
    cout << ch[maxi1] << ' ' << ch[maxi2] << endl;
    return 0;
}
```





### **1019 数字黑洞 (20 分)**

（字符串处理）

str.insert(it, size, string)函数使用

在it的位置，插入size个string字符串

```cpp
#include <iostream>
#include <algorithm>
#include <string>

using namespace std;

int main ()
{
    string str;
    cin >> str;
    str.insert(0, 4 - str.size(), '0');// 从第一个位置插，插入4-str.size()个'0'
    do
    {
        sort(str.begin(), str.end(), greater<int>());// 非升序排序
        int a = stoi(str);
        sort(str.begin(), str.end());// 非降序排序
        int b = stoi(str);
        int res = a - b;
        str = to_string(res);// 结果转化成字符串
        str.insert(0, 4 - str.size(), '0');
        printf("%04d - %04d = %04d\n", a, b, res);
    }while (str != "6174" && str != "0000");
    return 0;
}
```



### **1020 月饼 (25 分)**

（排序）（模拟）（贪心）

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 1010;

struct mcake
{
    float mount, sum;// 数量，销售量
    float unit;// 单价
    bool operator< (const mcake& m) const 
    {
        return unit > m.unit;
    }
}cake[N];

int main ()
{
    int N, need;
    cin >> N >> need;
    for (int i = 0; i < N; i ++)
        cin >> cake[i].mount;
    for (int i = 0; i < N; i ++)
        cin >> cake[i].sum;
    for (int i = 0; i < N; i ++)
        cake[i].unit = cake[i].sum /cake[i].mount;
    sort(cake, cake + N);// 按性价比排序大—>小
    
    float ans = 0.0;
    for (int i = 0; i < N; i ++)
    {
        if (need > cake[i].mount)
            ans += cake[i].sum;
        else
        {
            ans += cake[i].unit * need;
            break;
        }
        need = need - cake[i].mount;
    }
    printf("%.2f\n", ans);
    
    return 0;
}
```

### 1021 个位数统计 (15 分)
（哈希）

```cpp
#include <iostream>
#include <string>

using namespace std;

int count[10];// 统计数字的出现次数

int main ()
{
    string str;
    cin >> str;
    for (int i = 0; i < str.size(); i ++)
        count[str[i] - '0'] ++;
    for (int i = 0; i < 10; i ++)
        if (count[i])
            printf("%d:%d\n", i, count[i]);
    return 0;
}
```

### **1022 D进制的A+B (20 分)**

（模拟）（进制转换）

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main ()
{
    int n1, n2;
    cin >> n1 >> n2;
    long long num = n1 + n2;// 因为n1和n2都是int的最大范围，所以要用long long 接受防止爆int
    int b;
    cin >> b;
    vector<int> ans;
    while (num >= b)
    {
        ans.push_back(num % b);// 将每次的余数放入ans中
        num /= b;
    }
    ans.push_back(num);// 将最后一位放入ans中
    reverse(ans.begin(), ans.end());// 逆置一下，因为我是从后往前放入的
    for (int i = 0; i < ans.size(); i ++)
        cout << ans[i];
    cout << endl;
    return 0;
}
```



### **1023 组个最小数 (20 分)**

（贪心）

```cpp
#include <iostream>
#include <string>

using namespace std;

int main ()
{
    int a[10];
    for (int i = 0; i < 10; i ++)
        cin >> a[i];
    int start;
    for (start = 1; start < 10; start ++)// 找到第一个不是0的数
        if (a[start])
            break;
    string ans = to_string(start);
    a[start] --;
    // 将所有的0接在ans的后面
    while (a[0] --)
        ans += "0";
    // 从前往后将每个数转化成字符串接在后面
    for (int i = 1; i < 10; i ++)
        while (a[i] --)
            ans += to_string(i);
    cout << ans << endl;
    return 0;
}
```

