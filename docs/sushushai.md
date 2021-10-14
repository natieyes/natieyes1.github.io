# 素数筛
## 前言
~~没啥好说的~~
## 正文
### 前置芝士：素数
很简单，指除了 1 和本身，没有任何数能够整除的特殊数。
### 暴力筛
思路很简单，既然除了 1 和本身不能被任何数整除，那就直接从 2 模拟到 n-1 看能否被整除如果能，返回 `false`，不能返回 `ture`，时间复杂度 O(n^2)（区间），代码如下：
```cpp
void baoli(int n) {
    for (int i=1;i<=n;i++) {
        if (n==1 or n==0) {//特判
            prime[i]=0;
            break;
        if (n==2) {//还是特判
            prime[i]=1;
            break;
        }
        for (int i=2;i<n;i++) {
            if (n%i==0) {
                prime[i]=0;
            }
        }
        prime[i]=1;
    }
}
```
### 朴素筛
为暴力筛的优化，在暴力筛中，如果求一个区间素数有哪些，时间复杂度非常大，那我们想一下优化。

设一个合数为 x，令 x 表示为 a * b，再令 m=min(a,b)，所以暴力筛在循环时，会把 a 和 b 都计算一遍，会花费二倍的时间，所以我们可以直接枚举到 sqrt n，为什么呢因为 m=min(a,b)，也就为 min(x/b,b),所以 b 越小（但是不低于 a），m 就会越大。

时间复杂度为 O(n sqrt n)，代码为：
```cpp
void pusu(int n){
    for (int i=1;i<=n;i++) {
        if (n==1 or n==0) {
            prime[i]=0;
            break;
        if (n==2) {
            prime[i]=1;
            break;
        }
        for (int i=2;i<=sqrt(n);i++) {
            if (n%i==0) {
                prime[i]=0;
            }
        }
        prime[i]=1;
    }
}
```
### 埃氏筛
埃氏筛，为素数筛中代码适中，时间复杂度适中，思维适中的算法，十分实用，其发明者为埃拉托斯特尼（~~E 总~~）。

其算法主要思想为，如果一个数为素数，那么倍数必然为合数，那么只需找到第一个素数，自然就可以推出其他合数。

时间复杂度 O(n log n)，代码为：
```cpp
void aishi() { 
    for (int i=1;i<=n;i++) {
        prime[i]=1;
    }
    prime[1]=0;
    for (int i=2;i<=n;i++) {
        if (prime[i]) {
            q[++cnt]=i;
            for (int j=2;i*j<=n;j++) {
                prime[i*j]=0;
            }
        }
    }
}
```
### 线性筛
线性筛，又名欧拉筛，为伟大的数学巨人欧拉发明，时间复杂度 O(n)。

先上代码：
```cpp
void oula() {
    for (int i=1;i<=n;i++) {
        prime[i]=1;
    }
    prime[1]=0;
    for (int i=2;i<=n;i++) {
        if (prime[i]) {
            q[++cnt]=i;
        }
        for (int j=2;j<=cnt and i*j<=n and i%q[i]!=0;j++) {
             prime[i*q[j]]=0;
        }
    }
}
```
我们可以拿它和埃氏筛做一下对比，发现只有内循环变了。

首先条件多了 `j<=cnt` 和 `i%q[i]!=0`，并且循环内容变成了 `prime[i*q[j]]=0;`，我们先来看第一个循环条件和循环内容，相信大家都可以看懂，就是把 i 的素数倍标为合数，但是只快了那么一点点。

重要的为新增的第二个循环条件，想一想什么情况下 i mod prime_j=0 呢？

1. i=prime_j

2. i=n * prime_j

先来看 1，很明显既然 i=prime_j，也就说明 i 也为素数，所以 prime_{cnt}=i，而循环条件中有一句话 `j<=cnt`，所以循环也该结束了。

再来看 2，i=n * prime_j，这时候 i * prime_{j+1} 就等于 n * prime_j * prime_{j+1}，再用一下小学时候的乘法分配律，令 tmp=n * prime_{j+1}，所以原式为 prime_{j} * tmp，很明显 prime_{j}<prime_{j+1}，又因为 n 为正整数，所以 n * prime_j<tmp，也就是 i<tmp，所以在 i=tmp 时，还会再筛一次所以现在跳出循环，以后再筛。
## finally
素数筛一般不会单独考，通常结合其他知识综合考察。
