# Leetcode - 2551. Put Marbles in Bags (M+)

[Leetcode](https://leetcode.com/problems/put-marbles-in-bags/description/)

You have `k` bags. You are given a **0-indexed** integer array `weights` where `weights[i]` is the weight of the `ith` marble. You are also given the integer `k.`

Divide the marbles into the `k` bags according to the following rules:

-   No bag is empty.
-   If the `ith` marble and `jth` marble are in a bag, then all marbles with an index between the `ith` and `jth` indices should also be in that same bag.
-   If a bag consists of all the marbles with an index from `i` to `j` inclusively, then the cost of the bag is `weights[i] + weights[j]`.

The **score** after distributing the marbles is the sum of the costs of all the `k` bags.

Return _the **difference** between the **maximum** and **minimum** scores among marble distributions_.

**Example 1:**
```
Input: weights = [1,3,5,1], k = 2
Output: 4
Explanation: 
The distribution [1],[3,5,1] results in the minimal score of (1+1) + (3+1) = 6. 
The distribution [1,3],[5,1], results in the maximal score of (1+3) + (5+1) = 10. 
Thus, we return their difference 10 - 6 = 4.
```
**Example 2:**
```
Input: weights = [1, 3], k = 2
Output: 0
Explanation: The only distribution possible is [1],[3]. 
Since both the maximal and minimal score are the same, we return 0.
```
**Constraints:**

-   `1 <= k <= weights.length <= 105`
-   `1 <= weights[i] <= 109`

---
```java
// Java
class Solution {
    public long putMarbles(int[] weights, int k) {
        int n = weights.length;
        long ans = 0;
        long[] arr = new long[n-1];
        for (int i = 0; i < n - 1; i++)
            arr[i] = weights[i] + weights[i + 1];
        Arrays.sort(arr);
        for (int i = 0; i < k - 1; i++) {
            ans += arr[arr.length- 1 - i] - arr[i];
        }
        return ans;
    }
}

// [x x x][x x x x][x x x x x x x][x x x x]
```

---
將題目的選取結果畫出
```
[x x x][x x x x][x x x x x x x][x x x x]
     ----     ----           ----
```

不論取最大或最小, 頭尾皆會被取到,
因此題目可視為選出最大/最小的相鄰數字

需要有k個袋子, 因此只需要k-1個隔板 => k-1個相鄰數字



###### tags: `Leetcode` `Greedy`