Having overcome all the difficulties and hardships, Sasha finally decided to marry his girlfriend. To do this, he needs to give her an engagement ring. However, his girlfriend does not like such romantic gestures, but she does like binary search trees $^{\dagger}$. So Sasha decided to give her such a tree.

After spending a lot of time on wedding websites for programmers, he found the perfect binary search tree with the root at vertex $1$. In this tree, the value at vertex $v$ is equal to $val_v$.

But after some time, he forgot the values in some vertices. Trying to remember the found tree, Sasha wondered — how many binary search trees could he have found on the website, if it is known that the values in all vertices are integers in the segment $[1, C]$. Since this number can be very large, output it modulo $998\,244\,353$.

$^{\dagger}$ A binary search tree is a rooted binary tree in which for any vertex $x$, the following property holds: the values of all vertices in the left subtree of vertex $x$ (if it exists) are less than or equal to the value at vertex $x$, and the values of all vertices in the right subtree of vertex $x$ (if it exists) are greater than or equal to the value at vertex $x$.

**Input**

Each test consists of multiple test cases. The first line contains a single integer $t$ ($1 \le t \le 10^5$) — the number of test cases. The description of the test cases follows.

The first line of each test case contains two integers $n$ and $C$ ($2 \leq n \leq 5 \cdot 10^5$, $1 \leq C \leq 10^9$) — the number of vertices in the tree and the maximum allowed value at the vertex.

The next $n$ lines describe the vertices of the tree. The $i$ \-th line contains three integers $L_i, R_i$ and $val_i$ ($-1 \le L_i, R_i \le n$, $-1 \le val_i \le C$, $L_i, R_i, val_i \ne 0$) — the number of the left child, the number of the right child, and the value at the $i$ \-th vertex, respectively. If $L_i = -1$, then the $i$ \-th vertex has no left son. If $R_i = -1$, then the $i$ \-th vertex has no right son. If $val_i = -1$, then the value at the $i$ \-th vertex is unknown.

**Output**

For each test case, output a single integer — the number of suitable binary search trees modulo $998\,244\,353$.



It is guaranteed that at least one suitable binary search tree exists.

It is guaranteed that the sum of $n$ over all test cases does not exceed $5 \cdot 10^5$.

## 题解
我们考虑到我们二叉树中序遍历的性质：我们中序遍历后一定是一个单调递增的序列。于是之后，我们就转换为，在单调递减的序列中，在我们的 $[l,r]$ 范围内，选择 k 个数构成一个单调不低见数列的方案数。这个时候，我们就可以考虑用一种类似于隔板法的转换思路，转换为下面的等式：
$$
x_{1}+x_{2}+..+x_{r-l+1}=k
$$
$$
x_{i}\geq 0
$$
然后让我们求方案数。

于是，我们就可以采用我们的隔板法计算。最后再用乘法原理即可。

```cpp
void solve() {
    int n, m;
    cin >> n >> m;
    vector<int> l(n + 1), r(n + 1), val(n + 1);
    for (int i = 1; i <= n; i++) {
        cin >> l[i] >> r[i] >> val[i];
    }
    vector<int> a;
    a.push_back(1);
    function<void(int)> dfs = [&](int u) {
        if (l[u] != -1) dfs(l[u]);
        a.push_back(val[u]);
        if (r[u] != -1) dfs(r[u]);
    };
    dfs(1);
    a.push_back(m);
    Z ans = 1;
    auto comb = [&](int n, int m) -> Z {
        Z res = 1;
        for (int i = 1; i <= m; i++) {
            res = res * (n + 1 - i) / i;
        }
        return res;
    };
    for (int i = 1, j; i <= n; i = j) {
        if (a[i] != -1) {
            j = i + 1;
        } else {
            for (j = i; a[j] == -1; j++);
            int len = j - i;
            int num = a[j] - a[i - 1] + 1;
            ans *= comb(len + num - 1, len);
        }
    }
    cout << ans << '\n';
}
```