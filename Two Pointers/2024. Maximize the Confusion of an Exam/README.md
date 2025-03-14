# Leetcode - 2024. Maximize the Confusion of an Exam (M)

[Leetcode](https://leetcode.com/problems/maximize-the-confusion-of-an-exam/)

A teacher is writing a test with `n` true/false questions, with `'T'` denoting true and `'F'` denoting false. He wants to confuse the students by **maximizing** the number of **consecutive** questions with the **same** answer (multiple trues or multiple falses in a row).

You are given a string `answerKey`, where `answerKey[i]` is the original answer to the `ith` question. In addition, you are given an integer `k`, the maximum number of times you may perform the following operation:

-   Change the answer key for any question to `'T'` or `'F'` (i.e., set `answerKey[i]` to `'T'` or `'F'`).

Return _the **maximum** number of consecutive_ `'T'`s or `'F'`s _in the answer key after performing the operation at most_ `k` _times_.

**Example 1:**

> **Input:** answerKey = "TTFF", k = 2
> **Output:** 4
> **Explanation:** We can replace both the 'F's with 'T's to make answerKey = "TTTT".
> There are four consecutive 'T's.

**Example 2:**

> **Input:** answerKey = "TFFT", k = 1
> **Output:** 3
> **Explanation:** We can replace the first 'T' with an 'F' to make answerKey = "FFFT".
> Alternatively, we can replace the second 'T' with an 'F' to make answerKey = "TFFF".
> In both cases, there are three consecutive 'F's.

**Example 3:**

> **Input:** answerKey = "TTFTTFTT", k = 1
> **Output:** 5
> **Explanation:** We can replace the first 'F' to make answerKey = "TTTTTFTT"
> Alternatively, we can replace the second 'F' to make answerKey = "TTFTTTTT". 
> In both cases, there are five consecutive 'T's.

**Constraints:**

-   `n == answerKey.length`
-   `1 <= n <= 5 * 104`
-   `answerKey[i]` is either `'T'` or `'F'`
-   `1 <= k <= n`

---
```java
// Java 12m(Beats 75.26%), Time O(N), Space O(1)
class Solution {
    public int maxConsecutiveAnswers(String answerKey, int k) {
        int n = answerKey.length();
        int ret = 0;
        int cntT = 0, cntF = 0, j = 0;
        for (int i = 0; i < n; i++)
        {
            while (j < n && (cntT <= k || cntF <= k))
            {
                if (answerKey.charAt(j) == 'T')
                    cntT++;
                else
                    cntF++;
                j++;

                if (cntT <= k || cntF <= k)
                    ret = Math.max(ret, j - i);
            }

            if (answerKey.charAt(i) == 'T')
                cntT--;
            else
                cntF--;
        }

        return ret;
    }
}
```
---



###### tags: `Leetcode` `Two Pointers` `Sliding window`