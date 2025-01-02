# Leetcode - Quick Pow 快速冪

參考[LeetCode/Template/Math/QuickPow.cpp at master · wisdompeak/LeetCode · GitHub](https://github.com/wisdompeak/LeetCode/blob/master/Template/Math/QuickPow.cpp)

```java
class Solution {
long MOD = 1e9+7;
public:
    long quickMul(long x, long N) {
        if (N == 0) {
            return 1;
        }
        LL y = quickMul(x, N / 2) % MOD;
        return N % 2 == 0 ? (y * y % MOD) : (y * y % MOD * x % MOD);
    }

    double quickMul(double x, long N) {
        if (N == 0) {
            return 1.0;
        }
        double y = quickMul(x, N / 2);
        return N % 2 == 0 ? y * y : y * y * x;
    }

    // n can be negative.
    double myPow(double x, int n) {        
        return n >= 0 ? quickMul(x, n) : 1.0 / quickMul(x, -n);
    }
};
```


###### tags: `Leetcode` `Templates`