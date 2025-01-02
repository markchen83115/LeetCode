# Leetcode - Combination 排列組合

參考[LeetCode/Template/Math/Combination-Number.cpp at master · wisdompeak/LeetCode · GitHub](https://github.com/wisdompeak/LeetCode/blob/master/Template/Math/Combination-Number.cpp)

```java
/*********************************/
//  Version 1: compute along C(n,m) saved in comb
long comb[1000][1000];  
for (int i = 0; i <= n; ++i) 
{
    comb[i][i] = comb[i][0] = 1;            
    if (i==0) continue;
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
// Version 3: Compute C(m,n) on demand with module M
long M = 1e9+7;
long[] factorial = new long[N+1];

long[] GetFactorial(long N) //階層
{
    long[] rets[N+1];
    rets[0] = 1;
    for (int i=1; i<=N; i++)
        rets[i] = rets[i-1] * i % M;
    return rets;
}

long quickPow(long x, long N) {
    if (N == 0) {
        return 1;
    }
    long y = quickPow(x, N / 2) % M;
    return N % 2 == 0 ? (y * y % M) : (y * y % M * x % M);
}

long comb(long m, long n)
{
    if (n>m) return 0;
    long a = factorial[m];
    long b = factorial[n] * factorial[m-n] % M;
    long inv_b = quickPow(b, (M-2));
    
    return a * inv_b % M;
}

/*********************************/
// Version 4: Compute C(m,n) on demand with module M
long MOD = 1e9 + 7;
long[] factorial;
long[] GetFactorial(long N)
{
    long[]rets(N+1);
    rets[0] = 1;
    for (int i=1; i<=N; i++)
        rets[i] = rets[i-1] * i % MOD;
    return rets;
}

long quickPow(long x, long N) {
    if (N == 0) {
        return 1;
    }
    long y = quickPow(x, N / 2) % MOD;
    return N % 2 == 0 ? (y * y % MOD) : (y * y % MOD * x % MOD);
}

long comb(long m, long n)
{
    if (n>m) return 0;
    long a = factorial[m];
    long b = factorial[n] * factorial[m-n] % MOD;
    long inv_b = quickPow(b, (MOD-2));

    return a * inv_b % MOD;
}
```

###### tags: `Leetcode` `Templates`