# Leetcode - 763. Partition Labels (M-)

[Leetcode](https://leetcode.com/problems/partition-labels/)

You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.

Return _a list of integers representing the size of these parts_.

**Example 1:**

> **Input:** s = "ababcbacadefegdehijhklij"
> **Output:** [9,7,8]
> **Explanation:**
> The partition is "ababcbaca", "defegde", "hijhklij".
> This is a partition so that each letter appears in at most one part.
> A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.

**Example 2:**

> **Input:** s = "eccbbbbdec"
> **Output:** [10]

**Constraints:**

-   `1 <= s.length <= 500`
-   `s` consists of lowercase English letters.

---
```java
// Java 4ms(Beats 92.56%), Time O(N), Space O(N)
class Solution {
    public List<Integer> partitionLabels(String s) {
        int n = s.length();
        int[] firstIndex = new int[26];
        int[] lastIndex = new int[26];
        Arrays.fill(firstIndex, -1);
        Arrays.fill(lastIndex, -1);
        for (int i = 0; i < n; i++)
        {
            if (firstIndex[s.charAt(i) - 'a'] == -1)
                firstIndex[s.charAt(i) - 'a'] = i;
            lastIndex[s.charAt(i) - 'a'] = i;
        }

        int[] arr = new int[n];
        for (int i = 0; i < 26; i++)
        {
            if (firstIndex[i] == -1)
                continue;
            arr[firstIndex[i]]++;
            arr[lastIndex[i]]--;
        }

        List<Integer> ret = new ArrayList<>();
        int sum = 0;
        int prev = -1;
        for (int i = 0; i < n; i++)
        {
            sum += arr[i];
            if (sum == 0)
            {
                ret.add(i - prev);
                prev = i;
            }
        }

        return ret;   
    }
```
---

題目要求每種字母只能出現在一個partition內
代表每種字母的第一次出現與最後一次出現必為在一個
```
a b a b c b a c a d e f e g d e h i j h k l i j
a---------------a
  b-------b
        c-----c
                  d---------d
```
明顯可用差分數組來處理
在第一個`a`出現時`sum += 1`, 在最後一個`a`出現時`sum -= 1`
當`sum == 0`時, 代表此段已可切割


###### tags: `Leetcode` `Others` `掃描線/差分陣列`