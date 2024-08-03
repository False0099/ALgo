# 知识点
  ## [[动态规划]]   [[树形DP]] [[求最值]]
# 题目
 给定一棵树，树中包含 $n$ 个结点（编号 $1-n$ ）和 $n−1$  条无向边，每条边都有一个权值。

现在请你找到树中的一条最长路径。

换句话说，要找到一条路径，使得使得路径两端的点的距离最远。

注意：路径中可以只包含一个点。

# 思路
·我们分别计算以 x 节点为路径最高点的路径的最大值，相当于 x 是是我们钉上的一颗螺丝钉，只有挂在 x 上的我们才去计算。
·要找到挂在 x 上的上的最长的锁链，我们要求出他的各个路线上的最大长度，和第二大长度，然后挂在 x 上的最大线段就是最大路线长度和第二大路线长度
·
# AC 代码
```cpp
#include <iostream>
using namespace std;
const int N=1e4+10,M=2*N;
int h[N],ne[M],e[M],w[M],idx;
void add(int a,int b,int c){
	w[idx]=c;
	e[idx]=b;
	ne[idx]=h[a];
	h[a]=idx++;
}
void dfs(int u,int father){
	int dist=0,d1=0,d2=0;
	for(int i=h[u];i!=-1;i=ne[i]){
		int j=e[u];
		if(j==father){
		continue;
		}
		int d=dfs(j,u)+w[i];
		dist=max(dist,d);
		if(d>d1){
			d2=d1;
			d1=d;
		}else if(d>d2){
			d2=d;
		}
	}
	ans=max(ans,d1+d2);
	return dist;
}
int main(){
	int n;
	cin>>n;
	memset(h,-1,sizeof h);
	for(int i=0;i<n-1;i++){
		int a,b,c;
		cin>>a>>b>>c;
		add(a,b,c);
		add(b,a,c);
	}
	dfs(1,-1);
	
	cout<<ans<<endl;
}
```
# 备注
