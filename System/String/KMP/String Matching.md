# String Matching

## 题目

String matching is a common type of problem in computer science. One string matching problem is as following:

Given a string s[***0…len−1***], please calculate the length of the longest common prefix of s[***i…len−1***] and s[***0…len−1***] for each i>0.

I believe everyone can do it by brute force.
The pseudo code of the brute force approach is as the following:

![Sample Image](https://vj.ti12z.cn/7dc667a229be739b423147cd5977924a?v=1567868649)

We are wondering, for any given string, what is the number of compare operations invoked if we use the above algorithm. Please tell us the answer before we attempt to run this algorithm.

## 输入

The first line contains an integer T, denoting the number of test cases.
Each test case contains one string in a line consisting of printable ASCII characters except space.

* **1≤T≤30**

* string length **≤106** for every string

## 输出

For each test, print an integer in one line indicating the number of compare operations invoked if we run the algorithm in the statement against the input string.

## 输入输出样例

### Input

```txt
3
_Happy_New_Year_
ywwyww
zjczzzjczjczzzjc
```

### Output

```txt
17
7
32
```

## 题解

问：题目中的 if 执行多少次

解：如果不是由 while 跳出循环，则会执行到第一个不符合的位置，判断一下最终匹配的位置即可。

算法：扩展 KMP

## 代码

```c++
#include <iostream>
#include <cstdio>
#include <cstring>

using namespace std;

//扩展kmp:求t每一个后缀字符串匹配s最长前缀长度

int const Length = 1000100;
int nex[Length];
char cont[Length];

long long getnext()
{
    int lent = strlen(cont);
    long long ans = 0;
    int start = 0, pre = 0; //p是最长的匹配到的位置，a是它开始匹的位置
    nex[0] = lent;
    for (int i = 1; i < lent; i++)
    {
        if (i >= pre || i + nex[i - start] >= pre)
        {
            if (i >= pre)
                pre = i;
            while (pre < lent && cont[pre] == cont[pre - i])
                pre++;
            nex[i] = pre - i;
            start = i;
        }
        else
            nex[i] = nex[i - start];
        if (i + nex[i] == lent)
            ans += nex[i];
        else
            ans += nex[i] + 1;
    }
    return ans;
}

int main(void)
{
    int T;
    cin >> T;
    while (T--)
    {
        scanf("%s", cont);
        cout << getnext() << endl;
    }
}
```
