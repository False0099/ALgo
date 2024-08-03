![[Pasted image 20231128142254.png]]
![[Pasted image 20231128142301.png]]


## 题解
我们可以发现，我们一个操作最多执行 $\frac{n}{2}$ 次，因为我们每一次执行操作，都会消去我们两边的方块。那么我们就可以讨论，一个方块是因为变成了两边被消去，还是自己消去了。
我们用 $dp_{1}[i]$ 表示我们考虑左边，第 $i$ 个方块在 $dp[i]$ 次操作后被消去。我们的转移就可以写成：$dp_{1}[i]=min(dp_{1}[i-1]+1,h[i])$,我们的右侧转移也是同理，我们取个 $min$ 就是第 $i$ 个方块消去的操作数。最后我们记录下可以坚持最长时间的那个方块。
```cpp
void slove() {
	cin >> n;
	for (int i = 1; i <= n; i++)cin >> h[i];
	for (int i = 1; i <= n; i++)dp1[i] = min(dp1[i - 1] + 1, h[i]);
	for (int i = n; i >= 1; i--)dp2[i] = min(dp2[i + 1] + 1, h[i]);
	int ans = 0;
	for (int i = 1; i <= n; i++)ans = max(ans, min(dp1[i], dp2[i]));
	cout << ans << endl;
}
```