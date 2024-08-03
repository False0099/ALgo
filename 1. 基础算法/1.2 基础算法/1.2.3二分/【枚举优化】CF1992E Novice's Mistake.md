# Novice's Mistake

## 题目描述

One of the first programming problems by K 1 o 0 n looked like this: "Noobish\_Monk has $ n $ $ (1 \le n \le 100) $ friends. Each of them gave him $ a $ $ (1 \le a \le 10000) $ apples for his birthday. Delighted with such a gift, Noobish\_Monk returned $ b $ $ (1 \le b \le \min (10000, a \cdot n)) $ apples to his friends. How many apples are left with Noobish\_Monk?"

K 1 o 0 n wrote a solution, but accidentally considered the value of $ n $ as a string, so the value of $ n \cdot a - b $ was calculated differently. Specifically:

- When multiplying the string $ n $ by the integer $ a $ , he will get the string $ s=\underbrace{n + n + \dots + n + n}_{a\ \text{times}} $
- When subtracting the integer $ b $ from the string $ s $ , the last $ b $ characters will be removed from it. If $ b $ is greater than or equal to the length of the string $ s $ , it will become empty.

Learning about this, ErnKor became interested in how many pairs $ (a, b) $ exist for a given $ n $ , satisfying the constraints of the problem, on which K 1 o 0 n's solution gives the correct answer.

"The solution gives the correct answer" means that it outputs a non-empty string, and this string, when converted to an integer, equals the correct answer, i.e., the value of $ n \cdot a - b $ .

## 输入格式

The first line contains a single integer $ t $ ( $ 1 \le t \le 100 $ ) — the number of test cases.

For each test case, a single line of input contains an integer $ n $ ( $ 1 \le n \le 100 $ ).

It is guaranteed that in all test cases, $ n $ is distinct.

## 输出格式

For each test case, output the answer in the following format:

In the first line, output the integer $ x $ — the number of bad tests for the given $ n $ .

In the next $ x $ lines, output two integers $ a_i $ and $ b_i $ — such integers that K 1 o 0 n's solution on the test " $ n $ $ a_i $ $ b_i $ " gives the correct answer.

## 样例 #1

### 样例输入 #1

```
3
2
3
10
```

### 样例输出 #1

```
3
20 18 
219 216 
2218 2214 
1
165 162 
1
1262 2519
```

## 提示

In the first example, $ a = 20 $ , $ b = 18 $ are suitable, as " $ \text{2} $ " $ \cdot 20 - 18 = $ " $ \text{22222222222222222222} $ " $ - 18 = 22 = 2 \cdot 20 - 18 $

## 题解
本题，我们发现，我们的一大思路是枚举我们的 $a,b$ 但是注意到我们直接枚举，我们的时间复杂度是 $o(V^2)$ ，显然不满足我们的条件的，于是我们考虑是否有其他的方法，注意到，我们最后的**结果的位数**，我们可以发现，我们最后的位数一定是不会超过 10 位的，这是因为我们的 $ax-b$ 最后一定不会超过我们的 10 位，于是我们可以考虑**枚举我们的最后的位数，和我们的 a**，这样，我们也能够求出我们的 $b$ 是多少。

这个时候，我们只需要去模拟验证就行了。
```
#include <bits/stdc++.h>
#define int long long
int INF=0x3f3f3f3f3f;
using namespace std;
int get_digit(int n){
    int tmp=0;
    while(n){
        tmp++;
        n/=10;
    }
    return tmp;
}
typedef pair<int,int> PII;
void solve(){
    int n;
    cin>>n;
    string tmp=to_string(n);
    int digit=get_digit(n);
    string res2="";
    int cnt=0;
    vector<PII> res;
    for(int a=1;a<=10000;a++){
        res2+=tmp;
        for(int b=digit*a-10;dgit*a-b>=0&&b<=min(10000ll,a*n);b++){
            i(a*n-b<=0||(get_digit(a*n-b)!=(digit*a-b))){
                continue;
            }
            int res1=a*n-b;
            int digits=get_digit(a*n-b);
            string res22=res2.substr(0,digits);
            int ress=stoi(res22);
//            cerr<<ress<<endl;
            if(res1==ress){
                if(b==0){
                    continue;
                }
                res.push_back({a,b});
            }
        }
    }
    cout<<res.size()<<endl;
    for(auto u:res){
        cout<<u.first<<" "<<u.second<<endl;
    }
}
signed main(){
    ios::sync_with_stdio(false),cin.tie(0);
    int t;
    cin>>t;
    while(t--){
        solve();
    }
}
```