# Leetcode - 2528. Maximize the Minimum Powered City (H-)

[Leetcode](https://leetcode.com/problems/maximize-the-minimum-powered-city/description/)

You are given a **0-indexed** integer array `stations` of length `n`, where `stations[i]` represents the number of power stations in the `ith` city.

Each power station can provide power to every city in a fixed **range**. In other words, if the range is denoted by `r`, then a power station at city `i` can provide power to all cities `j` such that `|i - j| <= r` and `0 <= i, j <= n - 1`.

-   Note that `|x|` denotes **absolute** value. For example, `|7 - 5| = 2` and `|3 - 10| = 7`.

The **power** of a city is the total number of power stations it is being provided power from.

The government has sanctioned building `k` more power stations, each of which can be built in any city, and have the same range as the pre-existing ones.

Given the two integers `r` and `k`, return _the **maximum possible minimum power** of a city, if the additional power stations are built optimally._

**Note** that you can build the `k` power stations in multiple cities.

**Example 1:**
```
Input: stations = [1,2,4,5,0], r = 1, k = 2
Output: 5
Explanation: 
One of the optimal ways is to install both the power stations at city 1. 
So stations will become [1,4,4,5,0].
- City 0 is provided by 1 + 4 = 5 power stations.
- City 1 is provided by 1 + 4 + 4 = 9 power stations.
- City 2 is provided by 4 + 4 + 5 = 13 power stations.
- City 3 is provided by 5 + 4 = 9 power stations.
- City 4 is provided by 5 + 0 = 5 power stations.
So the minimum power of a city is 5.
Since it is not possible to obtain a larger power, we return 5.
```
**Example 2:**
```
Input: stations = [4,4,4,4], r = 0, k = 3
Output: 4
Explanation: 
It can be proved that we cannot make the minimum power of a city greater than 4.
```
**Constraints:**

-   `n == stations.length`
-   `1 <= n <= 105`
-   `0 <= stations[i] <= 105`
-   `0 <= r <= n - 1`
-   `0 <= k <= 109`

---

```java
// Java (49 ms)
class Solution {
    public long maxPower(int[] stations, int r, int k) {
        int n = stations.length;
        long left = 0, right = k;
        for (int station : stations)
            right += station;
        long[] sta = new long[n];
        while (left < right) {
            long mid = right - (right - left) / 2;   // guess mid can be answer
            for (int i = 0; i < n; i++)
                sta[i] = stations[i];
            
            // check
            if (isPossible(sta, r, k, mid)) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }

    public boolean isPossible(long[] stations, int r, int k, long ans) {
        int n = stations.length;
        long power = 0;
        for (int i = 0; i < r; i++)
            power += stations[i];
		
        for (int i = 0; i < n; i++) {
            if (i + r < n)	// add new station power
                power += stations[i + r];
            if (i - r - 1 >= 0)	// remove out of range station power
                power -= stations[i - r - 1];
            if (power >= ans)	// satisfy min power >= ans
                continue;
            long diff = ans - power;	// need to add how many station
            if (diff > k)
                return false;
            power += Math.max(0, diff);
            stations[Math.min(n - 1, i + r)] += diff;
            k -= diff;
        }
        return true;
    }
}
```

```java
// Java (66ms)
class Solution {
    public long maxPower(int[] stations, int r, int k) {
        int n = stations.length;
        long left = 0, right = k;
        for (int station : stations)
            right += station;
        long[] sta = new long[n];
        while (left < right) {
            long mid = right - (right - left) / 2;   // guess mid can be answer
            for (int i = 0; i < n; i++)
                sta[i] = stations[i];
            
            long power = 0, use = 0;
            for (int i = 0; i < r; i++)
                power += sta[i];

            for (int i = 0; i < n; i++) {
                if (i + r < n)  // add distance is <= r station power
                    power += sta[i + r];
                if (i - r - 1 >= 0) // remove distance is >r station power
                    power -= sta[i - r - 1];
                if (power >= mid)   // satisfy min power is mid
                    continue;
                long diff = Math.max(0, mid - power);   // need to add how many station
                int farestSta = Math.min(n - 1, i + r);
                power += diff;
                sta[farestSta] += diff;
                use += diff;
            }
            // check
            if (use <= k) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
}
```

---

[wisdompeak/Youtube解說](https://www.youtube.com/watch?v=rn0yE0gC8Vw)

由題目得知求**Maximize the Minimum**
設定一組上下限, 利用binary search去猜答案
若此答案可以, 則left = mid去尋找更大的答案
若此答案不行, 則right = mid - 1去尋找更小的答案

當發現`stations[i]`的power不夠時, 
將station新增在`stations[i+r]`的地方, 使得新station可供應更多的station


###### tags: `Leetcode` `Binary Search` `Binary Search by Value`
