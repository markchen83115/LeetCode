# Leetcode - Combination 排列組合

參考[LeetCode/Template/Math/Combination-Number.cpp at master · wisdompeak/LeetCode · GitHub](https://github.com/wisdompeak/LeetCode/blob/master/Template/Math/Combination-Number.cpp)

```java
/*********************************/
//  Version 1: compute along C(n,m) saved in comb
long comb[1000][1000];  
for (int i = 0; i <= n; ++i) 
{
    comb[i][i] = comb[i][0] = 1;            
    if (i == 0) continue;
    for (int j = 1; j < i; ++j) 
    {
        comb[i][j] = comb[i - 1][j - 1] + comb[i - 1][j];
    }
}  

/*********************************/
// Version 2: Compute C(n,m) on demand based on definition
long help(int n, int m)
{
    long cnt = 1;
    for(int i = 0; i < m; i++)
    {
        cnt *= n - i;
        cnt /= i + 1;
    }
    return cnt;
}

/*********************************/
// Version 3: Compute C(m,n) on demand with module mod
long mod = 1e9+7;
long[] factorial = new long[n + 1];

long[] GetFactorial(int n) //階層
{
    long[] rets = new long[n + 1];
    rets[0] = 1;
    for (int i = 1; i <= n; i++)
        rets[i] = rets[i-1] * i % mod;
    return rets;
}

long quickPow(long x, long n) {
    if (n == 0) {
        return 1;
    }
    long y = quickPow(x, n / 2) % mod;
    return n % 2 == 0 ? (y * y % mod) : (y * y % mod * x % mod);
}

long comb(long m, long n)
{
    if (n > m) return 0;
    long a = factorial[m];
    long b = factorial[n] * factorial[m-n] % mod;
    long inv_b = quickPow(b, (mod-2));
    
    return a * inv_b % mod;
}

/*********************************/
// Version 4: Compute C(m,n) on demand with module mod
long MOD = (int)1e9 + 7;
long[] factorial;
long[] GetFactorial(int n)
{
    long[] rets = new long[n + 1];
    rets[0] = 1;
    for (int i = 1; i<=n; i++)
        rets[i] = rets[i-1] * i % MOD;
    return rets;
}

long quickPow(long x, long n) {
    if (n == 0) {
        return 1;
    }
    long y = quickPow(x, n / 2) % MOD;
    return n % 2 == 0 ? (y * y % MOD) : (y * y % MOD * x % MOD);
}

long comb(long m, long n)
{
    if (n > m) return 0;
    long a = factorial[m];
    long b = factorial[n] * factorial[m-n] % MOD;
    long inv_b = quickPow(b, (MOD-2));

    return a * inv_b % MOD;
}

// Version 5
int mod = (int) (1e9 + 7);
long[] factorial;

long[] getFactorial(int n)
{
    long[] rets = new long[n + 1];
    rets[0] = 1;
    for (int i = 1; i<=n; i++)
        rets[i] = rets[i-1] * i % mod;
    return rets;
}

long quickPow(long base, int exp) {
    long result = 1;
    while (exp > 0) {
        if ((exp & 1) == 1) {
            result = (result * base) % mod;
        }
        base = (base * base) % mod;
        exp >>= 1;
    }
    return result;
}

long comb(int n, int k) {
    if (k > n) return 0;
    long numerator = 1;
    long denominator = 1;

    for (int i = 0; i < k; i++) {
        numerator = (numerator * (n - i)) % mod;
        denominator = (denominator * (i + 1)) % mod;
    }

    return (numerator * inverse(denominator)) % mod;
}

long inverse(long a) {
    return quickPow(a, mod - 2);
}

// Version 6 避免TLE 提前計算factorial(階乘), invFactorial
long mod = (int) 1e9 + 7;
long[] factorial;
long[] invFactorial;

void getFactorial(int n)
{
    factorial[0] = invFactorial[0] = 1;
    for (int i = 1; i<=n; i++)
        factorial[i] = factorial[i-1] * i % mod;

    invFactorial[n] = quickPow(factorial[n], mod - 2);
    for (int i = n - 1; i >= 1; i--) {
        invFactorial[i] = invFactorial[i + 1] * (i + 1) % mod;
    }
}

long quickPow(long base, long exp) {
    long result = 1;
    while (exp > 0) {
        if ((exp & 1) == 1) {
            result = (result * base) % mod;
        }
        base = (base * base) % mod;
        exp >>= 1;
    }
    return result;
}

long comb(int n, int k) {
    if (k > n || k < 0) 
        return 0;
    return factorial[n] * invFactorial[k] % mod * invFactorial[n - k] % mod;
}
```

###### tags: `Leetcode` `Templates`