给出 n，m，求出
$$
\sum_{i=1}^n \sum_{j=1}^m lcm(i,j)
$$

## 题解
我们这一题需要我们对原式子进行一波变换，即 $lcm(i,j)=\dfrac{i\times j}{gcd(i,j)}$,

原式子
= $\sum_{i=1}^n \sum_{j=1}^m \dfrac{i\times j}{gcd(i,j)}$
= $\sum_{i=1}^n\sum_{j=1}^m ij\sum_{d=1}^n \frac{[gcd(i,j)=d]}{d}$
= $\sum_{d=1}^n\sum_{i=1}^n \sum_{j=1}^m ij\frac{[gcd(i,j)=d]}{d}$
= $\sum_{d=1}^n\sum_{id=1}^n \sum_{jd=1}^m ijd^2\frac{[gcd(i,j)=d]}{d}$
= $\sum_{d=1}^n\sum_{i=1}^{n/d} \sum_{j=1}^{m/d} ijd\sum_{k|gcd(i,j)}\mu(k)$
= $\sum_{d=1}^n\sum_{i=1}^{n/d} \sum_{j=1}^{m/d} ijd\sum_{k=1}^n\mu(k)\left[ \frac{k}{i} \right]\left[ \frac{k}{j} \right]$
= $\sum_{d=1}^n d\sum_{i=1}^{n/d} i[k|i]\sum_{j=1}^{m/d}j[k|j] \sum_{k=1}^n\mu(k) ]$
= $\sum_{d=1}^n d\sum_{i=1}^{n/dk} i\sum_{j=1}^{m/dk}j \sum_{k=1}^{n/d}\mu(k) k^2$

我们计算这一个式子时，就需要我们进行一个分块套分块了，
我们令
$F(n,m)=\sum_{k=1}^n \mu(k)k^2 \sum_{i=1}^\left[ \frac{n}{k} \right]\sum_{j=1}^{m/k} ij$,
$G(n,m)=\sum_{i=1}^\left[ \frac{n}{k} \right]\sum_{j=1}^{m/k} ij$

此时我们的答案就是