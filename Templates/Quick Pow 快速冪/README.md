# Leetcode - Quick Pow 快速冪

參考[LeetCode/Template/Math/QuickPow.cpp at master · wisdompeak/LeetCode · GitHub](https://github.com/wisdompeak/LeetCode/blob/master/Template/Math/QuickPow.cpp)

```java
class Solution {
long mod = 1e9+7;
public:
    long quickPow(long x, long k) {
        if (k == 0) {
            return 1;
        }
        long y = quickPow(x, k / 2) % mod;
        return k % 2 == 0 ? (y * y % mod) : (y * y % mod * x % mod);
    }

    double quickPow(double x, long k) {
        if (k == 0) {
            return 1.0;
        }
        double y = quickPow(x, k / 2);
        return k % 2 == 0 ? y * y : y * y * x;
    }

    // n can be negative.
    double myPow(double x, int n) {        
        return n >= 0 ? quickPow(x, n) : 1.0 / quickPow(x, -n);
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
};
```


###### tags: `Leetcode` `Templates`