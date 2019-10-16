# Crazy Search

Source: [POJ 1200](http://poj.org/problem?id=1200)

## 题目

Many people like to solve hard puzzles some of which may lead them to madness. One such puzzle could be finding a hidden prime number in a given text. Such number could be the number of different substrings of a given size that exist in the text. As you soon will discover, you really need the help of a computer and a good algorithm to solve such a puzzle.
Your task is to write a program that given the size, N, of the substring, the number of different characters that may occur in the text, NC, and the text itself, determines the number of different substrings of size N that appear in the text.

As an example, consider N=3, NC=4 and the text "daababac". The different substrings of size 3 that can be found in this text are: "daa"; "aab"; "aba"; "bab"; "bac". Therefore, the answer should be 5.

## 输入

The first line of input consists of two numbers, N and NC, separated by exactly one space. This is followed by the text where the search takes place. You may assume that the maximum number of substrings formed by the possible set of characters does not exceed 16 Millions.

## 输出

The program should output just an integer corresponding to the number of different substrings of size N found in the given text.

## 输入输出样例

### Input

```txt
3 4
daababac
```

### Output

```txt
5
```

### Hint

Huge input,scanf is recommended.

## 题解

Hash 题，给每个字符分配一个唯一的 Hash 值，计算每个子串 cnt 进制的值并储存相关结果。

## 代码

```C++
#include <iostream>
#include <cstdio>
#include <cstring>

using namespace std;

const int Len = 16000003;
int N, NC;
int Hash[Len], Num[300];
char ch[Len];

int main(void)
{
    while (~scanf("%d%d%s", &N, &NC, ch))
    {
        int len = strlen(ch);
        int cnt = 0, ans = 0;
        Num[ch[0]] = cnt; //储存每个字符的 Hash 值，以 cnt 的值为基础
        for (int i = 1; i < len; i++)
        {
            if (Num[ch[i]] == 0)
                Num[ch[i]] = ++cnt; //储存其他字符的值，每次 cnt+1，以分配不同的 Hash 值
        }
        for (int i = 0; i <= len - N; i++)
        {
            int sum = 0;
            for (int j = 0; j < N; j++)
            {
                sum = sum * cnt + Num[ch[i + j]]; //将每个子串储存为 cnt 进制的数字
            }
            if (!Hash[sum])
            {
                //如果这个子串没出现过，计数+1，将结果储存进 Hash 数组中
                ans++;
                Hash[sum] = true;
            }
        }
    cout << ans << endl;
    }
}
```
