# Compress Words

## 题面翻译

Amugae 有 $n$ 个单词, 他想把这个 $n$ 个单词变成一个句子，具体来说就是从左到右依次把两个单词合并成一个单词. 合并两个单词的时候, 要找到最大的 $i(i\ge 0)$，满足第一个单词的长度为 $i$ 的后缀和第二个单词长度为 $i$ 的前缀相等, 然后把第二个单词第 $i$ 位以后的部分接到第一个单词后面。输出最后那个单词。

## 题目描述

Amugae has a sentence consisting of $ n $ words. He want to compress this sentence into one word. Amugae doesn't like repetitions, so when he merges two words into one word, he removes the longest prefix of the second word that coincides with a suffix of the first word. For example, he merges "sample" and "please" into "samplease".

Amugae will merge his sentence left to right (i.e. first merge the first two words, then merge the result with the third word and so on). Write a program that prints the compressed word after the merging process ends.

## 输入格式

The first line contains an integer $ n $ ( $ 1 \le n \le 10^5 $ ), the number of the words in Amugae's sentence.

The second line contains $ n $ words separated by single space. Each words is non-empty and consists of uppercase and lowercase English letters and digits ('A', 'B', ..., 'Z', 'a', 'b', ..., 'z', '0', '1', ..., '9'). The total length of the words does not exceed $ 10^6 $ .

## 输出格式

In the only line output the compressed word after the merging process ends as described in the problem.

## 样例 #1

### 样例输入 #1

```
5
I want to order pizza
```

### 样例输出 #1

```
Iwantorderpizza
```

## 样例 #2

### 样例输入 #2

```
5
sample please ease in out
```

### 样例输出 #2

```
sampleaseinout
```


## 题解
直接哈希

每次判断当前串的每一个前缀是否是答案的后缀，取最长的满足条件的前缀

如何判断？

按照哈希的方法：

```cpp
hans1[ansl] = ((ll)hans1[ansl-1]*b1 + s[i])%mod1;//b1是随机出来的进制
```

只要这样：

```cpp
(h1 + (ll)hans1[ansl-i]*pb1[i])%mod1
```

即：用当前“位数”为 i 的哈希值(`h1`)去 “替换” 当前答案的后i位

如果这个值与当前答案相等，那么它就是当前答案的后缀

使用了双哈希

```cpp
#include<cstdio>
#include<cstring>
#include<cstdlib>
#include<ctime>
typedef long long ll;
const int MAXL = 1e6 + 5;
const int mod1 = 19260817;
const int mod2 = 19491001;

int b1,pb1[MAXL],b2,pb2[MAXL];
int hans1[MAXL],hans2[MAXL];

char ans[MAXL],s[MAXL];

int main(void)
{
	srand(time(0));
	
	b1 = rand()%321 + 233;
	pb1[0] = 1;
	for(int i=1; i<MAXL; ++i) pb1[i] = (ll)pb1[i-1]*b1 %mod1;
	
	b2 = rand()%233 + 321;
	pb2[0] = 1;
	for(int i=1; i<MAXL; ++i) pb2[i] = (ll)pb2[i-1]*b2 % mod2;
	
	int n;
	scanf("%d",&n);
	
	int ansl=0;
	for(int j=1; j<=n; ++j)
	{
		scanf("%s",s+1);
		int sl = strlen(s+1);
		
		int h1=0, h2=0, len=0;
		for(int i=1; i<=sl && i<=ansl; ++i)
		{
			h1 = ((ll)h1*b1%mod1 + s[i]) %mod1;
			h2 = ((ll)h2*b2%mod2 + s[i]) %mod2;
			
			if(hans1[ansl] == (h1 + (ll)hans1[ansl-i]*pb1[i])%mod1 && hans2[ansl] == (h2 + (ll)hans2[ansl-i]*pb2[i])%mod2)
				len=i;
		}
		
		for(int i=len+1; i<=sl; ++i)
		{
			ans[++ansl] = s[i];
			hans1[ansl] = ((ll)hans1[ansl-1]*b1 + s[i])%mod1;
			hans2[ansl] = ((ll)hans2[ansl-1]*b2 + s[i])%mod2;
		}
	}
	printf("%s",ans+1);
	return 0;
}
```