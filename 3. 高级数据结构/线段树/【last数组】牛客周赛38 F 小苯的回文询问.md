小苯有一个长度为 $n$ 的数组 $a$，他定义一个数组是好数组，当且仅当该数组是一个回文数组，且长度**严格大于** $2$。  
  
他现在进行了 $q$ 次询问，每次询问都给出一段区间 $[l, r]$，他想知道 $a$ 在这一段区间中是否存在一个**子序列**是一个好数组，请你帮帮他吧。

## 输入：
输入包含 $q + 2$ 行。  
第一行两个正整数 $n, q\ (1 \leq n \leq 10^5),\ (1 \leq q \leq 2 \times 10^5)$，以空格分隔，分别表示小苯拥有的数组的长度，以及他的询问次数。  
第二行 $n$ 个正整数 $a_i\ (1 \leq a_i \leq 10^9)$，表示数组 $a$ 的元素。  
接下来 $q$ 行，每行两个正整数 $[l, r]\ (1 \leq l \leq r \leq n)$，以空格分隔，表示小苯每次询问的区间。

## 输出：
输出包含 $q$ 行，如果对于当前询问的区间，存在一个好子序列是一个好数组，则输出 "YES"，否则输出 "NO"。（不含双引号）

## 样例：
```
9 3
1 1 2 1 1 3 1 1 2
1 4
2 7
1 2
```

```
YES
YES
NO
```

## 题解
对于这种要求我们的回文串种类的，我们不妨先去找一找规律，例如我们之前的 [[【回文串】CF1944D Non-Palindromic Substring]]，就是通过我们的找规律找到结论来帮助我们做题的。
在这里，首先，我们有下面的性质：如果一个字符串是我们的回文字符串，那么它的子字符串也一定是我们的回文串，因此，我们只需要处理我们 $len=3$ 和 $len=4$ 的情况。
对于 $len=3$ 的情况，我们只需要找到一个区间，当中某一个字母出现的次数大于等于 3，或者出现次数大于等于 2，但是我们两者之间的间隔不能为 0.
我们发现，上面的信息我们可以考虑通过一个 $last$ 数组来表示我们位置 $i$ 对应的上一个能对他有影响的位置是多少。显然，如果我们上一个相同颜色的字符距离我们当前位置为 1，那么我们的 `last` 应该设置为上上个相同颜色的字符的位置。否则，我们直接把我们的 `last` 设置为 $lastcol[col[i]]$ 即可。

之后，我们每一次就只需要动态更新我们的 $lastcol$ 和我们的 $seccol$ 即可完成我们的维护。

再然后，我们一个区间内有没有相同的颜色出现了两次以上，就可以通过我们的 last 来得到，如果有一种颜色出现了两次以上，那么我们一定有：$max(last[l\dots r])\geq l$,这里，我们就可以通过我们的线段树+离线快速的求出我们的上面的结果。

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 2e5 + 5, INF = 2e9;
typedef long long ll;
int n, q;
ll a[N], nans = LLONG_MIN;
map<int,int> last;
map<int,int> lastcol;//col[i]->上一个位置
map<int,int> seccol;//i->上一个位置
class SegmentTree
{
public:
	struct Node
	{
		int l, r;
		ll maxx;
		
	}tr[N << 2];
	Node merge(Node x, Node y)
	{
		if (x.l == -1) return y;
		if (y.l == -1) return x;
		Node z;
		z.l=min(x.l,y.l);
		z.r=max(x.r,y.r);
		z.maxx=max(x.maxx,y.maxx);
		return z;
	}
	void pushup(int u)
	{
		tr[u] = merge(tr[u << 1], tr[u << 1 | 1]);
	}
	void build(int u, int l, int r)
	{
		tr[u] = {l, r, 0};
		if (l == r) return;
		int mid = l + r >> 1;
		build(u << 1, l, mid);
		build(u << 1 | 1, mid + 1, r);
		pushup(u);
	}
	void update(int u, int p, ll x)
	{
		if (tr[u].l == tr[u].r)
		{
			tr[u].maxx=x;
			return;
		}
		int mid = tr[u].l + tr[u].r >> 1;
		if (p <= mid) update(u << 1, p, x);
		else update(u << 1 | 1, p, x);
		pushup(u);
	}
	Node query(int u, int l, int r)
	{
		if (tr[u].l >= l and tr[u].r <= r) 
		{
			return tr[u];
		}
		int mid = tr[u].l + tr[u].r >> 1;
		Node ret;
		ret.l = -1;
		if (l <= mid) ret = query(u << 1, l, r);
		if (r > mid) ret = merge(ret, query(u << 1 | 1, l, r));
		return ret;
	}
}t;
struct ask{
	int l,r,id,ans;	
}Ask[N];
int main()
{
	cin>>n>>q;
	for (int i = 1; i <= n; i++){
		cin>>a[i];
	}
	t.build(1,1,n);
	for(int i=1;i<=q;i++){
		int l,r;
		cin>>l>>r;
		Ask[i]={l,r,i};
	}
	sort(Ask+1,Ask+1+q,[](ask a,ask b){
		return a.r<b.r;
	});
	for(int i=1,j=1;i<=q;i++){
		int r=Ask[i].r;
		while(j<=r){
			int col_now=a[j];
			//先更新我们的last[j]=lastcol[a[j]],如果我们满足lastcol[a[j]]与j之间的间隔大于1 
			// 否则，我们的last[j]就是我seccol[a[j]]
			//last[j]表示上一个和j颜色相同的有效位置在哪里 
			if(lastcol[col_now]==j-1){
				last[j]=seccol[col_now];
				seccol[col_now]=j-1;
				lastcol[col_now]=j;
			}else{
				last[j]=lastcol[col_now];
				seccol[col_now]=lastcol[col_now];
				lastcol[col_now]=j;
			}
			t.update(1,j,last[j]);
            j++;
		}
		//cout<<t.query(1,Ask[i].l,r).maxx;
		Ask[i].ans=(t.query(1,Ask[i].l,r).maxx>=Ask[i].l);
	}
	sort(Ask+1,Ask+1+q,[](ask a,ask b){
		return a.id<b.id;
	});
	for(int i=1;i<=q;i++){
        if(Ask[i].ans){
            cout<<"YES"<<endl;
        }else{
            cout<<"NO"<<endl;
        }
	}
	
}
```