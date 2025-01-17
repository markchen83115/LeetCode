# Leetcode - 085. Maximal Rectangle (H-)

[Leetcode](https://leetcode.com/problems/maximal-rectangle/)

Given a `rows x cols` binary `matrix` filled with `0`'s and `1`'s, find the largest rectangle containing only `1`'s and return _its area_.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2020/09/14/maximal.jpg)
> 
> **Input:** matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
> **Output:** 6
> **Explanation:** The maximal rectangle is shown in the above picture.

**Example 2:**

> **Input:** matrix = [["0"]]
> **Output:** 0

**Example 3:**

> **Input:** matrix = [["1"]]
> **Output:** 1

**Constraints:**

-   `rows == matrix.length`
-   `cols == matrix[i].length`
-   `1 <= row, cols <= 200`
-   `matrix[i][j]` is `'0'` or `'1'`.

---
```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        int ret = 0;
        int m = matrix.length, n = matrix[0].length;
        int[] nums = new int[n];
        for (int i = 0; i < m; i++)
        {
            for (int j = 0; j < n; j++)
            {
                if (matrix[i][j] == '0')
                    nums[j] = 0;
                else
                    nums[j] += 1;
            }
            ret = Math.max(ret, maxRectangle(nums));
        }

        return ret;
    }

    int maxRectangle(int[] nums)
    {
        int ret = 0;
        int n = nums.length;
        int[] preSmall = new int[n];
        int[] postSmall = new int[n];
        preSmall[0] = -1;
        postSmall[n-1] = n;
        for (int i = 1; i < n; i++)
        {
            int p = i - 1;
            while (p >= 0 && nums[p] >= nums[i])
                p = preSmall[p];
            preSmall[i] = p;
        }

        for (int i = n - 2; i >= 0; i--)
        {
            int p = i + 1;
            while (p < n && nums[p] >= nums[i])
                p = postSmall[p];
            postSmall[i] = p;
        }

        for (int i = 0; i < n; i++)
            ret = Math.max(ret, nums[i] * (postSmall[i] - preSmall[i] - 1));
        
        return ret;
    }
}
```
---

此題是Leetcode第84題的二維擴展，轉換為求maximal histgram.

```java
for (int i = 0; i < m; i++)
    for (int j = 0; j < n; j++)
    {
        if (matrix[i][j] == '0')
            nums[j] = 0;
        else
            nums[j] += 1;
    }
```


###### tags: `Leetcode` `Stack` `monotonic stack: other usages`