# Leetcode - 781. Rabbits in Forest (M+)

[Leetcode](https://leetcode.com/problems/rabbits-in-forest/)

There is a forest with an unknown number of rabbits. We asked n rabbits **"How many rabbits have the same color as you?"** and collected the answers in an integer array `answers` where `answers[i]` is the answer of the `ith` rabbit.

Given the array `answers`, return _the minimum number of rabbits that could be in the forest_.

**Example 1:**

> **Input:** answers = [1,1,2]
> **Output:** 5
> **Explanation:**
> The two rabbits that answered "1" could both be the same color, say red.
> The rabbit that answered "2" can't be red or the answers would be inconsistent.
> Say the rabbit that answered "2" was blue.
> Then there should be 2 other blue rabbits in the forest that didn't answer into the array.
> The smallest possible number of rabbits in the forest is therefore 5: 3 that answered plus 2 that didn't.

**Example 2:**

> **Input:** answers = [10,10,10]
> **Output:** 11

**Constraints:**

-   `1 <= answers.length <= 1000`
-   `0 <= answers[i] < 1000`

---
```java
// Java 3ms(Beats 82.48%), Time O(N), Space O(N)
class Solution {
    public int numRabbits(int[] answers) {
        int ret = 0;
        int[] freq = new int[10001];
        for (int ans : answers)
            freq[ans]++;
        
        for (int i = 0; i <= 1000; i++)
            ret += (freq[i] + i) / (i + 1) * (i + 1);

        return ret;
    }
}
```
---

如果answers裡面含有元素`m`，那麼我們最多允許它出現`m+1`次，使得這`m+1`個報告的兔子是屬於共同顏色的
如果出現了第`m+2`隻兔子同樣報告了`m`，那麼它只能是另一種顏色，即必然另外`m+1`隻兔子屬於另一個顏色圈

更一般的，如果有n隻兔子都報告了m，那麼「顏色圈」的數量最少是`k = (n-1) / (m+1) + 1`
那麼對應有`(m+1) * k`隻兔子



###### tags: `Leetcode` `Greedy`