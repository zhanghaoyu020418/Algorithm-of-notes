# 深度优先搜索

### 全排列数字
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10;
int n;
int path[N];//保留搜索路径
bool st[N];//判断数字是否已经用过了

void dfs(int u)
{
    if(u == n)//已经搜索到最后一层，可以直接输出了
    {
         //数字放到路径是从0 ~ n - 1,但是下面dfs的是从1 ~ n枚举（易错要注意）
        for (int i = 0; i < n; i ++)
            cout << path[i] << ' ';
        cout << endl;
        return ;
    }
    
    //如果没有到最后一层，则将没有出现过的数字放到对应层数的位置
    for (int i = 1; i <= n; i ++)
        if(!st[i])
        {
            //将i放到path路径中，并标识i已经用过了
            path[u] = i;
            st[i] = true;
            //进入下一层
            dfs(u + 1);
            //回复现场
            path[u] = 0;
            st[i] = false;
        }
}

int main ()
{
    cin >> n;//排列从1到n
    
    dfs(0);//从第一层暴搜
    
    return 0;
}
```

### n皇后
```cpp
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 20;
int col[N], dg[N], udg[N];//dg[u + i]是正对角线,udg[i - u + n]是负对角线
int n;
char a[N][N];

void dfs(int u)//u是行数,暴搜行数
{
    if(u == n)
    {
        for (int i = 0; i < n; i ++)//如果搜索到最后一层就输出结果
            puts(a[i]);//puts(a[i]) == cout << a[i] << endl;直接输出一行
        cout << endl;
        return ;
    }
    
    //如果不能让行和正负对角线，满足要求，则直接跳过dfs，所以只要全部填完，就是最后的结果
    for (int i = 0; i < n; i ++)
        if(!col[i] && !dg[u + i] && !udg[i - u + n])//如果该列和正负对角线都没有皇后就放上去
        {
            a[u][i] ='Q';
            col[i] = dg[u + i] = udg[i - u + n] = true;
            dfs(u + 1);
            col[i] = dg[u + i] = udg[i - u + n] = false;
            a[u][i] = '.';
        }
}

int main ()
{
    //设置棋盘的长宽
    cin >> n;
    
    //填充棋盘内容
    for (int i  = 0; i < n; i ++)
        for (int j = 0; j < n; j ++)
            a[i][j] = '.';
            
    dfs(0);//暴搜行数
    
    return 0;
}
```