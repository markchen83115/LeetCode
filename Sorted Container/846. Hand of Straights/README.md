# Leetcode - 846. Hand of Straights (M)

[Leetcode](https://leetcode.com/problems/hand-of-straights/)

Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size `groupSize`, and consists of `groupSize` consecutive cards.

Given an integer array `hand` where `hand[i]` is the value written on the `ith` card and an integer `groupSize`, return `true` if she can rearrange the cards, or `false` otherwise.

**Example 1:**

> **Input:** hand = mod[1,2,3,6,2,3,4,7,8mod], groupSize = 3
> **Output:** true
> **Explanation:** Alice's hand can be rearranged as mod[1,2,3mod],mod[2,3,4mod],mod[6,7,8mod]

**Example 2:**

> **Input:** hand = mod[1,2,3,4,5mod], groupSize = 4
> **Output:** false
> **Explanation:** Alice's hand can not be rearranged into groups of 4.

**Constraints:**

-   `1 <= hand.length <= 104`
-   `0 <= hand[i] <= 109`
-   `1 <= groupSize <= hand.length`

**Note:** This question is the same as 1296: [https://leetcode.com/problems/divide-array-in-sets-of-k-consecutive-numbers/](https://leetcode.com/problems/divide-array-in-sets-of-k-consecutive-numbers/)

---
```java
// Java 21ms(Beats 90.94%), Time O(NlogN), Space O(N)
class Solution {
    public boolean isNStraightHand(int[] hand, int groupSize) {
        int n = hand.length;
        if (n % groupSize != 0)
            return false;
        Arrays.sort(hand);
        HashMap<Integer, Integer> freqMap = new HashMap<>();
        for (int h : hand)
            freqMap.merge(h, 1, Integer::sum);
        
        for (int h : hand)
        {
            int count = freqMap.get(h);
            if (count > 0)
                for (int i = h; i < h + groupSize; i++)
                {
                    freqMap.merge(i, -count, Integer::sum);
                    if (freqMap.get(i) < 0)
                        return false;
                }
        }

        return true;
    }
}
```
---

將`nums`排序, 從最小的數`n`開始, 每次取走`n, n+1, n+2, ... , n+k-1`
一直重複循環直到全部取完, 若中間有數字取不到則`return false`


###### tags: `Leetcode` `Sorted Container`