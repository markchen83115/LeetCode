# Leetcode - Inverse_Element(逆元)

參考[LeetCode/Template/Inverse_Element at master · wisdompeak/LeetCode · GitHub](https://github.com/wisdompeak/LeetCode/tree/master/Template/Inverse_Element)


一般地，我們說如果a和b滿足這樣的性質：

```java
x / a ≡ x * b (mod M)
```

從形式來看，b好像就與1/a（同餘的意義）等價，所以我們稱b為a的逆元，記做 b = inv(a)（反之也成立）。
對a而言，存在逆元的充要條件是a與M互質。當然，我們做題時M一般都已經是質數。

#### 逆元的計算方法

##### 方法1：快速冪法

根據費馬小定理，我們有

```java
inv(a) ≡ a ^ (M-2) (mod M)
```

顯然，我們需要利用快速冪來計算這個數。

##### 方法2：線性求逆元

如果我們想求1,2,3...N 每個數的逆元：

```java
const long N = 1e6+7, M = 998244353;
long inv[N];
int i;
for(inv[1] = 1, i = 2; i < N; i++)
    inv[i] = (M - M / i) * inv[M % i] % M
```

#### 逆元的一些性質

逆元的計算有如下的性質：

```java
x1/y1 + x2/y2 ≡ x1 * y1^-1 + x2 * y2^-1 (mod M)
x1/y1/y2 ≡ x1 * y1^-1 * y2^-1 (mod M)
```

#### Template

```java
long N = 1_000_000_007, MOD = 998244353;

/*********************************/
// Linear method to compute inv[i]
long[] compute_inv(int n)
{    
    long[] inv = new long[n + 1];
    inv[1] = 1;
    for (int i = 2; i < n; i++)
        inv[i] = (MOD - MOD / i) * inv[MOD % i] % MOD;
    
    return inv;
}

/*********************************/
// Qucik Pow Method, based on Fermat's little theorem

long quickPow(long x, long N) 
{
    if (N <= 0) {
        return 1;
    }
    long y = quickPow(x, N / 2) % MOD;
    return N % 2 == 0 ? (y * y % MOD) : (y * y % MOD * x % MOD);
}

long inv(long x) 
{
    return quickPow(x, MOD - 2);
}

/*****************************/

long compute_inv(int x) 
{
    long s = 1;
    for (; x > 1; x = MOD % x)
      s = s * (MOD - MOD / x) % MOD;
    return s;
}
```

###### tags: `Leetcode` `Templates`