# Leetcode - 1718. Construct the Lexicographically Largest Valid Sequence (H-)

[Leetcode](https://leetcode.com/problems/construct-the-lexicographically-largest-valid-sequence/)

Given an integer `n`, find a sequence that satisfies all of the following:

-   The integer `1` occurs once in the sequence.
-   Each integer between `2` and `n` occurs twice in the sequence.
-   For every integer `i` between `2` and `n`, the **distance** between the two occurrences of `i` is exactly `i`.

The **distance** between two numbers on the sequence, `a[i]` and `a[j]`, is the absolute difference of their indices, `|j - i|`.

Return _the **lexicographically largest** sequence__. It is guaranteed that under the given constraints, there is always a solution._

A sequence `a` is lexicographically larger than a sequence `b` (of the same length) if in the first position where `a` and `b` differ, sequence `a` has a number greater than the corresponding number in `b`. For example, `[0,1,9,0]` is lexicographically larger than `[0,1,5,6]` because the first position they differ is at the third number, and `9` is greater than `5`.

**Example 1:**

> **Input:** n = 3
> **Output:** [3,1,2,3,2]
> **Explanation:** [2,3,2,1,3] is also a valid sequence, but [3,1,2,3,2] is the lexicographically largest valid sequence.

**Example 2:**

> **Input:** n = 5
> **Output:** [5,3,1,4,3,5,2,4,2]

**Constraints:**

-   `1 <= n <= 20`

---
```java
// Java 0ms(Beats 100.00%), Time O(2^(2N-1)), Space O(2N - 1)
class Solution {
    int[] arr;
    int n;
    boolean[] used = new boolean[21];
    public int[] constructDistancedSequence(int n) {
        this.n = n;
        arr = new int[2 * n - 1];
        dfs(0);
        return arr;
    }

    boolean dfs(int pos)
    {
        if (pos == 2 * n - 1)
            return true;
        if (arr[pos] > 0)
            return dfs(pos + 1);
        for (int i = n; i >= 1; i--)
        {
            if (used[i])
                continue;
            if (i > 1 && (pos + i >= 2 * n - 1 || arr[pos + i] > 0))
                continue;
            arr[pos] = i;
            if (i > 1) arr[pos + i] = i;
            used[i] = true;
            
            if (dfs(pos + 1))
                return true;
            
            arr[pos] = 0;
            if (i > 1) arr[pos + i] = 0;
            used[i] = false;
        }

        return false;
    }
}
```
---

參考[【每日一题】1718. Construct the Lexicographically Largest Valid Sequence - YouTube](https://youtu.be/e3n3DBH0C68)

本題的原型來自於 [https://leetcode-cn.com/leetbook/read/interesting-algorithm-puzzles-for-programmers/9umkfd/](https://leetcode-cn.com/leetbook/read/interesting-algorithm-puzzles-for-procnmers/9umkf/d

這種題並沒有特殊的技巧，就是貪心暴搜。我們希望這個數最大，那麼自然希望最高位最大，我們第一步貪心地在ret[0]放置n，根據題意要求在ret[n]也放置n；第二步，我們嘗試在ret[1]放置第二大的元素，也就是n-1，那麼根據題意ret[n]也必須放置n-1，但發現該位置已經放置了n-1，那麼根據題意，nret 2，發現可以繼續推進...

就是這樣從高位到低位不斷地嘗試，每次位置優先嘗試放置較大的元素。直到我們找到一個方法能把`2*n-1`個位置都填充滿，自然就是我們所能構造的最大答案。


###### tags: `Leetcode` `DFS`