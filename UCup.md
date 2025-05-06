# [The 2nd UCup Stage 11 Nanjing](https://qoj.ac/contest/1435)

## E

如果是一般图，则有一个 $O(nk)$ 个点，$O((n+m)k)$ 条边的网络流做法，不能通过。

但是本题图为平面图，考虑平面图最大流等于对偶图最短路，平面图的对偶图的对偶图皆为其本身，我们将对偶图建出，每次操作相当于给某条边容量加一，最小操作数使最大流增加 $k$，给每条边多建一个费用为 $1$，容量无限的复制即可，复杂度 $O((nm)^3+knm\log(nm))$。

# [The 2nd UCup Stage 12 Hefei](https://qoj.ac/contest/1440)

## H

当 $n\ge 13$，$f(n),g(n)$ 式子中对 $n$ 取 $\max$ 的部分可以删去。

考虑 $f^\prime (n)=f(n)-f(n-1),g^\prime(n)=g(n)-g(n-1)$，则当 $n\ge 14$：
$$
f^\prime(n)=[2|n]g^\prime(n/2)+[3|n]g^\prime(n/3)+[5|n]g^\prime(n/5)+[7|n]g^\prime(n/7)\\
g^\prime(n)=[2|n]f^\prime(n/2)+[3|n]f^\prime(n/3)+[4|n]f^\prime(n/4)+[5|n]f^\prime(n/5)\\
$$
考虑 $n=xk$，其中 $\gcd(2\times 3\times 5\times 7,x)=1$，可以归纳证明，假如 $x\ge 14$，则 $f^\prime (n)=g^\prime(n)=0$。

所以对于 $f,g$ 的差分，只有 $O(\log^4)$ 个位置有值，分别求出，每次询问时二分查询前缀和即可，复杂度 $O(\log^4 V+T\log(\log^4V))$，其中 $V=10^{15}$。

# [The 2nd UCup Stage 13 Shenyang](https://qoj.ac/contest/1449)

## L

假如存在一行没有车，那么必然每列都有车，我们对于每列都分别二分，用没有车那一行作为指示位即可 $\lceil \log_2n\rceil$ 次获得 $n$ 个车的位置。存在一列没车同理。

那么我们先询问 $\{(i,1),(i,i)\mid 2\le i\le n\}$，此时若 $c_{1,1}=0$，则第 $1$ 行无车，用上述方法解决。若 $c_{i,i}=1$ 则 $(i,1),(i,i)$ 中必有一车，否则 $(i,1),(i,i)$ 都没车。

若不存在 $i$ 使得 $c_{i,i}=0$，则询问 $\{(i,i)\mid 1\le i\le n\}$，则此时对于 $i\ge 2$，每行都找到了一个车，同时我们直到第 $1$ 行有车，用 $\lceil\log_2 n\rceil$ 次二分即可。

否则，我们询问 $\{(i,i),(1,i)\mid c_{i,i}=0\}\cup\{(i,1)\mid c_{i,i}=1\}$，则若新的 $c^\prime_{i,1}=1\land c_{i,i}=0$，则第 $i$ 行无车，用上述方法解决。若 $c^\prime_{i,i}=1\land c_{i,i}=0\land i\ge 2$，则 $(1,i)$ 必有车，若这样的 $i$ 不存在，且 $c^\prime_{1,i}=1\land c_{i,i}=0\land i\ge 2$ 存在，则 $(1,1)$ 必有车，否则 $(1,1)$ 必没车。

此时，对于每一行，要么已知车存在于 $(i,1),(i,i)$ 之一，要么已知该行有车且 $(i,1)$ 定无车。

对于后者，我们可以使用类似一行没有车的做法。同时，在二分的过程中，我们可以把满足前者的那些行在主对角线的元素插入在二分时的询问中，实现仔细的话，可以在 $\lceil \log_2 n\rceil$ 次询问中把每行的车的位置全都确定。

总询问次数 $2+\lceil \log_2 n\rceil$。

# [The 3rd UCup Stage 31 Wroclaw](https://qoj.ac/contest/1924)

## E

对于一个由简化后的语法写的一个函数，当输入全部取 $1$ 时，输出一定为 $1$。故 $f(1,\dots,1)=1$ 必须成立，这也是充分条件。

首先构造一个形如 $\texttt{min (a <= a) (min (b <= b) (...))}$ 的函数，这个函数永远输出 $1$。记为 $res$。

接下来对于每个 $f(v_a,v_b,\dots,v_j)=0$，构造一个函数 $h$ 使得只在这个取值上为 $0$，在其他取值都是 $1$，那么令让 $res$ 变成 $\min(res,h)$。

令 $S_0=\{x|v_x=0\},S_1=\{x|v_x=1\}$，那么对于 $S_0=\{x_0,x_1,\dots,x_{m-1}\}$，构造 $\min\{x_i \le x_{(i+1)\bmod m}\}\le x_0$，这个函数只在 $x_i$ 相当全为 $0$ 时输出 $0$，否则输出 $1$，记为 $g$。

对于 $\forall x\in S_1$，依次令 $g$ 变为 $x\le g$。那么只有当 $g=0$ 且 $S_1$ 中元素全为 $1$ 时输出 $0$，否则输出 $1$，恰为我们需要的 $h$。

函数长度为 $O(n2^n)$。

# [The 3rd UCup Stage 33 India](https://qoj.ac/contest/1954)

## C

令 $mx(n)$ 表示长度为 $n$ 的字符串中，$\texttt{uwu}$ 的个数最多为多少。

那么 $mx(3n)=n^3,mx(3n+1)=n^2(n+1),mx(3n+2)=n(n+1)^2$。

那么我们断言，当 $K$ 足够大时，找到最小的 $n$ 使得 $mx(n)\ge K$，那么存在长度等于 $n$ 的合法串。

具体的，先构造形如 $\texttt{u}^a\texttt{w}^b\texttt{u}^a$，其中 $2a+b=n,|a-b|\le 1$。考虑如果把一个 $\texttt{w}$ 移动到第 $a-i$ 个 $\texttt{u}$ 的右边，那么它的贡献会从 $a^2$ 变成 $(a-i)(a+i)$，即少了 $i^2$。

那么相当于我们要用 $i_1^2,\dots,i_b^2$ 凑出 $mx(n)-K$，其中 $0\le i_j\le a$。

注意到此时 $mx(n)-K < a^2$，且如果 $x^2\le c < (x+1)^2$，那么 $c-x^2\le 2x$，故我们每次贪心令 $i_j$ 为可选的最大值，至多需要 $O(\log\log a)$ 步。

当 $n > 15$ 时，$b\ge O(\log\log a)$，采用上述方案构造。否则爆搜。