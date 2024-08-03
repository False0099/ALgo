

## 题目描述
给出 n, m, k，求：
$$
\sum_{i=1}^n \sum_{j=1}^m [gcd(i,j)=k]
$$
的和

## 输入格式

共一行，包含三个整数 $n,m,k$

## 输出格式

一行，表示 $a$ 和 $b$ 的最大公约数。

## 样例 #1

### 样例输入 #1

```
12
54
```

### 样例输出 #1

```
6
```

## 提示

#### 数据规模与约定
- 对于 $20\%$ 的数据，有 $0<a,b\le 10^{18}$。
- 对于 $100\%$ 的数据，有 $0<a,b\le 10^{10000}$。

## 题解
我们可以首先对我们的和式进行一定的变化
1. 当我们的 k=1 时，我们可以简单的得到：
$\sum_{i=1}^n \sum_{j=1}^m [gcd(i,j)=1]=\sum_{i=1}^n \sum_{j=1}^m \sum_{d|gcd(i,j)} \mu(d)$
$=\sum_{i=1}^n \sum_{j=1}^m \sum_{d=1}^n [d|i][d|j]\mu(d)$
= $\sum_{d=1}^n \mu(d)\sum_{i=1}^n [d|u]\sum_{j=1}^m [d|j]$
= $\sum_{d=1}^n \mu(d)\left[ \dfrac{n}{d} \right]\left[ \dfrac{m}{d} \right]$

我们的这一个式子，可以通过我们的**整除分块**来进行快速的计算

2. 当我们的 k！=1 时，我们可以通过变换我们的下表，交换我们的求和次序等方法，得到我们的最后答案为：

$\sum_{d=1}^{n/d} \mu(d)\left[ \dfrac{n}{kd} \right]\left[ \dfrac{m}{kd} \right]$

这一个部分，我们同样也可以通过我们的**整除分块**来进行一个快速的计算

```cpp
void init(int n){
	mu[1]=1;
	for(int i=2;i<=n;i++){
		if(!vis[i]){
			p[++cnt]=i,mu[i]=-1;
		}
		for(int j=1;i*p[j]<N;j++){
			vis[i*p[j]]=1;
			if(i%p[j]==0) break;
			mu[i*p[j]]=-mu[i];
		}
	}
	for(int i=1;i<N;i++){
		mu[i]+=mu[i-1];
	}
}
long long calc(int n,int m,int t){
	if(n>m) swap(n,m);
	n=n/k,m=m/k;
	ll ans=0;
	for(int l=1,r;l<=n;l=r+1){
		r=min(n/(n/l),m/(m/l));
		ans+=1ll*(mu[r]-mu[i-1])*(n/l)*(m/l);
	}
	return ans;
}
```
