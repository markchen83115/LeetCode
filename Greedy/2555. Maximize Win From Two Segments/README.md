# Leetcode - 2555. Maximize Win From Two Segments (M+)

[Leetcode](https://leetcode.com/problems/maximize-win-from-two-segments/description/)

There are some prizes on the **X-axis**. You are given an integer array `prizePositions` that is **sorted in non-decreasing order**, where `prizePositions[i]` is the position of the `ith` prize. There could be different prizes at the same position on the line. You are also given an integer `k`.

You are allowed to select two segments with integer endpoints. The length of each segment must be `k`. You will collect all prizes whose position falls within at least one of the two selected segments (including the endpoints of the segments). The two selected segments may intersect.

-   For example if `k = 2`, you can choose segments `[1, 3]` and `[2, 4]`, and you will win any prize i that satisfies `1 <= prizePositions[i] <= 3` or `2 <= prizePositions[i] <= 4`.

Return _the **maximum** number of prizes you can win if you choose the two segments optimally_.

**Example 1:**
```
Input: prizePositions = [1,1,2,2,3,3,5], k = 2
Output: 7
Explanation: In this example, you can win all 7 prizes by selecting two segments [1, 3] and [3, 5].
```
**Example 2:**
```
Input: prizePositions = [1,2,3,4], k = 0
Output: 2
Explanation: For this example, one choice for the segments is `[3, 3]` and `[4, 4],` and you will be able to get `2` prizes. 
```
**Constraints:**

-   `1 <= prizePositions.length <= 105`
-   `1 <= prizePositions[i] <= 109`
-   `0 <= k <= 109`
-   `prizePositions` is sorted in non-decreasing order.

---

```java
// Java ThreePass 4ms
class Solution {
    public int maximizeWin(int[] prizePositions, int k) {
        int n = prizePositions.length;
        if (prizePositions[n - 1] - prizePositions[0] <= 2 * k) 
            return n;
        
        int[] pre = new int[n];
        int[] post = new int[n];
        int i = 0;
        int max = 0;
        for (int j = 0; j < n; j++) {
            while (prizePositions[j] - prizePositions[i] > k)
                i++;
            int len = j - i + 1;
            max = Math.max(max, len);
            pre[j] = max;
        }
        
        int y = n - 1;
        max = 0;
        for (int x = n - 1; x >= 0; x--) {
            while (prizePositions[y] - prizePositions[x] > k)
                y--;
            int len = y - x + 1;
            max = Math.max(max, len);
            post[x] = max;
        }
        
        int ret = 0;
        for (int m = 0; m < n - 1; m++) {
            ret = Math.max(ret, pre[m] + post[m + 1]);
        }
        
        return ret;
        
    }
}
```

```java
// Java DP 3ms
class Solution {
    public int maximizeWin(int[] prizePositions, int k) {
        int n = prizePositions.length;
        if (prizePositions[n - 1] - prizePositions[0] <= 2 * k) 
            return n;
        
        int[] dp = new int[n+1];
        int i = 0;
        int max = 0;
        for (int j = 0; j < n; j++) {
            while (prizePositions[j] - prizePositions[i] > k)
                i++;
            int len = j - i + 1;
            dp[j + 1] = Math.max(dp[j], len);
            max = Math.max(max, dp[i] + len);
        }

        return max;
        
    }
}
```

---

> Three Pass

[wisdompeak/YouTube](https://www.youtube.com/watch?v=0Tjuy464sP8)
????????????????????????, ?????????k????????????
??????`pre[k]`??????`0 -> k`????????????, 
??????`post[k]`??????`n-1 -> k`????????????,
?????????`pre[k]+post[k]`????????????
```
--------------k----------------
   -------j   |    i-------  
Max(pre[k]    |  +  post[k])
              |
```



> DP

[DP + Sliding Segment](https://leetcode.com/problems/maximize-win-from-two-segments/solutions/3141449/java-c-python-dp-sliding-segment-o-n/?orderBy=most_votes)

`dp[k]`?????????`0 -> k`????????????
????????????????????????sliding window?????????????????????`[i, j]`, 
????????????????????????`dp[i]`


???????????????????????? + ????????????`dp[i]`
?????? = `(j - i + 1) + dp[i]`






###### tags: `Leetcode` `Greedy` `Three Pass` `Dynamic Programming`