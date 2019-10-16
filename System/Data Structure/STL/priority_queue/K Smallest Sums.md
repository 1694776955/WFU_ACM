# K Smallest Sums

Source: [UVA - 11997](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=3148)

## 题意

给定 K 行 K 列数字，要求每行里面的一个数字跟另外几行的一个数字相加，能得出来的最小的 K 个数字。

## 输入

```txt
3
1 8 5
9 2 5
10 7 6
2
1 1
1 2
```

## 输出

```txt
9 10 12
2 2
```

## 思路

两行两行相加，直到加完为止。每次两两相加的时候都取出最小的三个数字，再进行下一次的两两相加。

## 结构体定义

```c++
struct Item
{
    int q, p;
    Item(int q, int p): q(q), p(p) {};
    bool operator < (const Item& item) const{
        return q > item.q;
    }
};
```

q 用于储存相加得的值，p 用于标记当前的列以及计数所用。

## 变量定义

```c++
int a[Len][Len]; // 存储输入的数字
int b[Len];      // 存储每次两两相加获得的最小值
```

## 输入代码

```c++
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
                cin >> a[i][j];
            sort(a[i], a[i] + n); // 对本行数据进行排序
        }
```

关键点在于每输入一行数字，就要对该行的数字进行排序

## 两两合并代码

```c++
// 以第一行为基础，对其余行进行两两合并，并且每次合并后都将得到的最小的三个数更新到第一行中
        for (int i = 1; i < n; i++)
        {
            merge(a[0], a[i], b, n);
            for (int j = 0; j < n; j++)
                a[0][j] = b[j]; //合并完成之后，将第一行数据更新为合并的结果，并继续下一次的合并
        }

// *x: 第一行数据
// *y：要合并的行
// *z：储存合并后的结果
void merge(int *x, int *y, int *z, int n)
{
    priority_queue<Item> q; //声明自定义结构体的优先队列，队列中每次弹出来的和都是最小的
    for (int i = 0; i < n; i++)
        q.push(Item(x[i] + y[0], 0)); // 储存第一行每个数据与目标行第一个数字相加得到的和，并标记当前操作行为第 0 列（用人话讲是第一列）
    for (int i = 0; i < n; i++)
    {
        Item item = q.top(); //获得目前队列中最小的和
        q.pop();
        z[i] = item.q; //储存相加之后的值到 b 数组中
        int b = item.p; //列标识
        if (b + 1 < n)
            q.push(Item(z[i] - y[b] + y[b + 1], b + 1)); //更新：比如第一行第一个元素和第二行第一个元素相加后，更新为第一行第一个元素与第二行第二个元素相加，并压入队列 q 中。计数 +1，更新目标列为第二列
    }
}
```

## 输出最终结果

```c++
// 合并后的最终结果被更新到第一行中，所以输出第一行的数即可
        for (int i = 0; i < n; i++)
        {
            if (i == 0)
                cout << a[0][i];
            else
            {
                cout << " " << a[0][i];
            }
        }
        cout << endl;
```

## 完整源代码

```c++
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

struct Item
{
    int q, p;
    Item(int q, int p) : q(q), p(p){};
    bool operator<(const Item &item) const
    {
        return q > item.q;
    }
};

const int Len = 800;
int a[Len][Len];
int b[Len];

void merge(int *x, int *y, int *z, int n)
{
    priority_queue<Item> q;
    for (int i = 0; i < n; i++)
        q.push(Item(x[i] + y[0], 0));
    for (int i = 0; i < n; i++)
    {
        Item item = q.top();
        q.pop();
        z[i] = item.q;
        int b = item.p;
        if (b + 1 < n)
            q.push(Item(z[i] - y[b] + y[b + 1], b + 1));
    }
}

int main(void)
{
    int n;
    while (cin >> n)
    {
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
                cin >> a[i][j];
            sort(a[i], a[i] + n);
        }
        for (int i = 1; i < n; i++)
        {
            merge(a[0], a[i], b, n);
            for (int j = 0; j < n; j++)
                a[0][j] = b[j];
        }
        for (int i = 0; i < n; i++)
        {
            if (i == 0)
                cout << a[0][i];
            else
            {
                cout << " " << a[0][i];
            }
        }
        cout << endl;
    }
}
```
