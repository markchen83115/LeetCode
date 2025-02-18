# Leetcode - 2375. Construct Smallest Number From DI String (M)

[Leetcode](https://leetcode.com/problems/construct-smallest-number-from-di-string/)

You are given a **0-indexed** string `pattern` of length `n` consisting of the characters `'I'` meaning **increasing** and `'D'` meaning **decreasing**.

A **0-indexed** string `num` of length `n + 1` is created using the following conditions:

-   `num` consists of the digits `'1'` to `'9'`, where each digit is used **at most** once.
-   If `pattern[i] == 'I'`, then `num[i] < num[i + 1]`.
-   If `pattern[i] == 'D'`, then `num[i] > num[i + 1]`.

Return _the lexicographically **smallest** possible string _`num`_ that meets the conditions._

**Example 1:**

**Input:** pattern = "IIIDIDDD"
**Output:** "123549876"
**Explanation:** At indices 0, 1, 2, and 4 we must have that num[i] < num[i+1].
At indices 3, 5, 6, and 7 we must have that num[i] > num[i+1].
Some possible values of num are "245639871", "135749862", and "123849765".
It can be proven that "123549876" is the smallest possible num that meets the conditions.
Note that "123414321" is not possible because the digit '1' is used more than once.

**Example 2:**

**Input:** pattern = "DDD"
**Output:** "4321"
**Explanation:**
Some possible values of num are "9876", "7321", and "8742".
It can be proven that "4321" is the smallest possible num that meets the conditions.

**Constraints:**

-   `1 <= pattern.length <= 8`
-   `pattern` consists of only the letters `'I'` and `'D'`.

---
```java
class Solution {
    public String smallestNumber(String pattern) {
        pattern = "I" + pattern;
        int n = pattern.length();
        StringBuilder sb = new StringBuilder();
        int max = 0;
        for (int i = 0; i < n; i++)
        {
            int j = i + 1;
            while (j < n && pattern.charAt(j) == 'D')
                j++;

            int count = j - i;
            
            for (int k = max + count; k >= max + 1; k--)
                sb.append(k);
            
            max = max + count;
            i = j - 1;
        }

        return sb.toString();
    }
}
```
---

參考[【每日一题】LeetCode 2375. Construct Smallest Number From DI String (aka LC 484. Find Permutation) - YouTube](https://youtu.be/7QXAWuEfWnI)

首先我們知道，必須將盡量小的字元放在前面使用

我們將`pattern`前面加上一個`I`，這樣`pattern`的長度就與字串相等
我們發現，將每個`IDD...D`視為一個`section`，那麼後一個`section`必然要完全高於前一個`section`
我們虛擬地令目前的最大字元是`0`，然後把後續整個字串的相對走勢都表達出來後（必然都大於0），得到的就是用`1~9`組成的字典序最小的字串。

註：本題是`942. DI String Match`的follow-up，且和`484.Find-Permutation`一模一樣



###### tags: `Leetcode` `Greedy` `DI Sequence`