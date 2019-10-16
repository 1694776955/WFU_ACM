# Period

Source: [HDU 1358](http://acm.hdu.edu.cn/showproblem.php?pid=1358)

## 题目

For each prefix of a given string S with N characters (each character has an ASCII code between 97 and 126, inclusive), we want to know whether the prefix is a periodic string. That is, for each i (2 <= i <= N) we want to know the largest K > 1 (if there is one) such that the prefix of S with length i can be written as A K ,that is A concatenated K times, for some string A. Of course, we also want to know the period K.

## 输入

The input consists of several test cases. Each test case consists of two lines. The first one contains N (2 <= N <= 1 000 000) – the size of the string S.The second line contains the string S. The input file ends with a line, having the
number zero on it.

## 输出

For each test case, output "Test case #" and the consecutive test case number on a single line; then, for each prefix with length i that has a period K > 1, output the prefix size i and the period K separated by a single space; the prefix sizes must be in increasing order. Print a blank line after each test case.

## 输入输出样例

### Input

```txt
3
aaa
12
aabaabaabaab
0
```

### Output

```txt
Test case #1
2 2
3 3

Test case #2
2 2
6 2
9 3
12 4
```

## 题解

给定一个字符串，求这个字符串到第 i 个字符为止的循环节的次数。

```txt
比如aabaabaabaab，长度为12。
到第二个a时，a出现2次，输出2。
到第二个b时，aab出现了2次，输出2。
到第三个b时，aab出现3次，输出3。
到第四个b时，aab出现4次，输出4。
```

## 代码

```c++
#include <iostream>
#include <cstring>
#include <cstdio>

using namespace std;

const int MAXN = 1e6 + 10;
char str1[MAXN];
int next[MAXN];
int len;

void getnext()
{
    len = strlen(str1);
    int j = 0;
    int k = next[0] = -1;
    while (j < len)
    {
        if (k == -1 || str1[j] == str1[k])
        {
            j++;
            k++;
            next[j] = k;
        }
        else
            k = next[k];
    }
}

int main(void)
{
    int T;
    int index = 1;
    while (cin >> T && T)
    {
        scanf("%s", str1);
        cout << "Test case #" << index++ << endl;
        getnext();
        for (int i = 1; i < len; i++)
        {
            int k = i + 1;
            if (k % (k - next[k]) == 0 && k / (k - next[k]) != 1)
                cout << k << " " << k / (k - next[k]) << endl;
        }
        cout << endl;
    }
}
```
