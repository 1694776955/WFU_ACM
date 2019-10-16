# String Problem

Source: [HDU 3374](http://acm.hdu.edu.cn/showproblem.php?pid=3374)

## 题目

Give you a string with length N, you can generate N strings by left shifts. For example let consider the string “SKYLONG”, we can generate seven strings:

```txt
String Rank
SKYLONG 1
KYLONGS 2
YLONGSK 3
LONGSKY 4
ONGSKYL 5
NGSKYLO 6
GSKYLON 7
```

and lexicographically first of them is GSKYLON, lexicographically last is YLONGSK, both of them appear only once.

Your task is easy, calculate the lexicographically fisrt string’s Rank (if there are multiple answers, choose the smallest one), its times, lexicographically last string’s Rank (if there are multiple answers, choose the smallest one), and its times also.

## 输入

Each line contains one line the string S with length N (N <= 1000000) formed by lower case letters.

## 输出

Output four integers separated by one space, lexicographically fisrt string’s Rank (if there are multiple answers, choose the smallest one), the string’s times in the N generated strings, lexicographically last string’s Rank (if there are multiple answers, choose the smallest one), and its times also.

## 输入输出样例

### Input

```txt
abcder
aaaaaa
ababab
```

### Output

```txt
1 1 6 1
1 6 1 6
1 3 2 3
```

## 题解

算法：最大最小表示法 + KMP

题意：给你一字符串，问最小字典序还有最大字典序的最小下标，以及包含最小字典序和最大字典序的串的开始的不同下标个数。

## 代码

```c++
#include <iostream>
#include <cstring>
#include <cstdio>

using namespace std;

const int Length = 1000005;

int Next[Length];
char s[Length];

void Init(char ch[])
{
    int i, j;
    i = 0, j = -1;
    Next[0] = -1;
    while (ch[i])
    {
        if (j == -1 || ch[i] == ch[j])
        {
            i++;
            j++;
            Next[i] = j;
        }
        else
            j = Next[j];
    }
}

int Get_min(bool flag, char ch[])
{
    int n = strlen(ch);
    int i = 0, j = 1, k = 0;
    int t;
    //表示从i开始k长度和从j开始k长度的字符串相同
    while (i < n && j < n && k < n)
    {
        t = ch[(i + k) % n] - ch[(j + k) % n];
        //t用来计算相对应位置上那个字典序较大
        if (!t)
            k++; //字符相等的情况
        else
        {
            if (flag == false)
            {
                if (t > 0)
                    i += k + 1;
                //i位置大,最大表示法: j += k+1
                else
                    j += k + 1;
                //j位置大,最大表示法: i += k+1
            }
            else
            {
                if (t > 0)
                    j += k + 1;
                else
                    i = i + k + 1;
            }
            if (i == j)
                j++;
            k = 0;
        }
    }
    return min(i, j);
}

int main(void)
{
    int i, j, n, t, len, min, max;

    while (~scanf("%s", s))
    {
        Init(s);
        len = strlen(s);
        t = 1;
        if (len % (len - Next[len]) == 0) //判断是否为循环串
            t = len / (len - Next[len]);
        min = Get_min(false, s) + 1;
        max = Get_min(true, s) + 1;
        printf("%d %d %d %d\n", min, t, max, t);
    }
}
```
