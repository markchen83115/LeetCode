# Leetcode - 2580. Count Ways to Group Overlapping Ranges (M)

[Leetcode](https://leetcode.com/problems/count-ways-to-group-overlapping-ranges/)

You are given a 2D integer array `ranges` where `ranges[i] = [starti, endi]` denotes that all integers between `starti` and `endi` (both **inclusive**) are contained in the `ith` range.

You are to split `ranges` into **two** (possibly empty) groups such that:

-   Each range belongs to exactly one group.
-   Any two **overlapping** ranges must belong to the **same** group.

Two ranges are said to be **overlapping** if there exists at least **one** integer that is present in both ranges.

-   For example, `[1, 3]` and `[2, 5]` are overlapping because `2` and `3` occur in both ranges.

Return _the **total number** of ways to split_ `ranges` _into two groups_. Since the answer may be very large, return it **modulo** `109 + 7`.

**Example 1:**
```
Input: ranges = [[6,10],[5,15]]
Output: 2
Explanation: 
The two ranges are overlapping, so they must be in the same group.
Thus, there are two possible ways:
- Put both the ranges together in group 1.
- Put both the ranges together in group 2.
```
**Example 2:**
```
Input: ranges = [[1,3],[10,20],[2,5],[4,8]]
Output: 4
Explanation: 
Ranges [1,3], and [2,5] are overlapping. So, they must be in the same group.
Again, ranges [2,5] and [4,8] are also overlapping. So, they must also be in the same group. 
Thus, there are four possible ways to group them:
- All the ranges in group 1.
- All the ranges in group 2.
- Ranges [1,3], [2,5], and [4,8] in group 1 and [10,20] in group 2.
- Ranges [1,3], [2,5], and [4,8] in group 2 and [10,20] in group 1.
```
**Constraints:**

-   `1 <= ranges.length <= 105`
-   `ranges[i].length == 2`
-   `0 <= starti <= endi <= 109`

---

```java
// Java - contest code
class Solution {
    public int countWays(int[][] ranges) {
        long mod = (long)1e9 + 7;
        Arrays.sort(ranges, (a, b) -> a[0] - b[0]);
        int counts = 1;
        int max = ranges[0][1];
        for (int i = 1; i < ranges.length; i++) {
            int start = ranges[i][0], end = ranges[i][1];
            if (start <= max) {   //overlap
                max = Math.max(max, end);
            } else {
                max = end;
                counts++;
            }
        }
        
        long ret = 1, p = 2;
        while (counts > 0) {
            if ((counts & 1) == 1)
                ret = ret * p % mod;
            p = p * p % mod;
            counts >>= 1;
        }
        return (int) (ret % mod);
    }
}
```

```java
// Java
class Solution {
    public int countWays(int[][] ranges) {
        int mod = (int)1e9 + 7;
        Arrays.sort(ranges, (a, b) -> a[0] - b[0]);
        int ret = 1;
        int max = -1;
        for (int[] range : ranges) {
            if (max < range[0])
                ret = ret * 2 % mod;
            max = Math.max(max, range[1]);
        }
        return ret % mod;
    }
}

```

---

計算出有多少個區間, 答案為`(2 ^ 區間個數) % (1e7 + 9)`
因為每個區間可選擇放`Group 1`或`Group 2`, 2種選擇


###### tags: `Leetcode` `Greedy` `Intervals`
