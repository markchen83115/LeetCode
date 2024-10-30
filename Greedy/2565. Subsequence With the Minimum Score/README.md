# Leetcode - 2565. Subsequence With the Minimum Score (H-)

[Leetcode](https:leetcode.comproblemssubsequence-with-the-minimum-scoredescription)

You are given two strings `s` and `t`.

You are allowed to remove any number of characters from the string `t`.

The score string is `0` if no characters are removed from the string `t`, otherwise:

-   Let `left` be the minimum index among all removed characters.
-   Let `right` be the maximum index among all removed characters.

Then the score of the string is `right - left + 1`.

Return _the minimum possible score to make _`t`_ a subsequence of _`s`_._

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

**Example 1:**
```
Input: s = "abacaba", t = "bzaa"
Output: 1
Explanation: In this example, we remove the character "z" at index 1 (0-indexed).
The string t becomes "baa" which is a subsequence of the string "abacaba" and the score is 1 - 1 + 1 = 1.
It can be proven that 1 is the minimum score that we can achieve.
```
**Example 2:**
```
Input: s = "cde", t = "xyz"
Output: 3
Explanation: In this example, we remove characters "x", "y" and "z" at indices 0, 1, and 2 (0-indexed).
The string t becomes "" which is a subsequence of the string "cde" and the score is 2 - 0 + 1 = 3.
It can be proven that 3 is the minimum score that we can achieve.
```
**Constraints:**

-   `1 <= s.length, t.length <= 105`
-   `s` and `t` consist of only lowercase English letters.

---
```java
//Java Time: O(N)
class Solution {
    public int minimumScore(String s, String t) {
        int sLen = s.length(), tLen = t.length();
        // suffix
        int[] suffix = new int[tLen];
        Arrays.fill(suffix, -1);
        int k = tLen - 1;
        for (int i = sLen - 1; i >= 0 && k >= 0; i--) {
            if (s.charAt(i) == t.charAt(k)) {
                suffix[k] = i;
                k--;
            }
        }
        // because k--, need go back to last suffix k where s.charAt(i) == t.charAt(k)
        k = k + 1;
        int ans = k;
        // k == 0 represent t is already a subsequence of s
        if (k == 0)
            return 0;
        // prefix
        for (int i = 0, j = 0; i < sLen && j < tLen; i++) {
            if (s.charAt(i) == t.charAt(j)) {
                //suffix index can't overlap index i
                for (; k < tLen && suffix[k] <= i; k++);
                j++;
                ans = Math.min(ans, k - j);
            }
        }

        return ans;
    }
}
```

---

[wisdompeak/Youtube解說 O(NlogN)](https:www.youtube.comwatch?v=vcjfoFhqzcI&list=PLwdV8xC1EWHrtgsYCcDTXIMVaHSlsnLzL&index=3)

[Right and Left O(n)](https:leetcode.comproblemssubsequence-with-the-minimum-scoresolutions3174041right-and-left-o-n?orderBy=most_votes)


###### tags: `Leetcode` `Greedy` `Three-pass`
