# 剑指offer



## [剑指 Offer 64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)



用静态成员函数将静态成员数据重新初始化

```cpp
class Add
{
public:
    Add()
    {
        _ans += _n;
        _n ++;
    }
    static int GetAns()
    {
        return _ans;
    }
    static void Init()
    {
        _n = 1;
        _ans = 0;
    }
private:
    static int _n;
    static int _ans;
};

int Add::_n = 1;
int Add::_ans = 0;

class Solution {
public:
    int sumNums(int n) {
        Add::Init();
        Add ans[n];
        return Add::GetAns();
    }
};
```





将静态成员设置为public，在类外可以直接访问重新初始化

```cpp
class Add
{
public:
    Add()
    {
        _ans += _n;
        _n ++;
    }
    static int GetAns()
    {
        return _ans;
    }
    static int _n;
    static int _ans;
};

int Add::_n = 1;
int Add::_ans = 0;

class Solution {
public:
    int sumNums(int n) {
        Add::_n = 1;
        Add::_ans = 0;
        Add ans[n];
        return Add::GetAns();
    }
};
```



用友元类来重新初始化静态成员



```cpp
class Add
{
    friend class Solution;
public:
    Add()
    {
        _ans += _n;
        _n ++;
    }
    static int GetAns()
    {
        return _ans;
    }
private:
    static int _n;
    static int _ans;
};

int Add::_n = 1;
int Add::_ans = 0;

class Solution {
public:
    int sumNums(int n) {
        Add::_n = 1;
        Add::_ans = 0;
        Add ans[n];
        return Add::GetAns();
    }
};
```

