# Leetcode - 3399. Smallest Substring With Identical Characters II (H-)

[Leetcode](https://leetcode.com/problems/smallest-substring-with-identical-characters-ii/)

You are given a binary string `s` of length `n` and an integer `numOps`.

You are allowed to perform the following operation on `s` **at most** `numOps` times:

-   Select any index `i` (where `0 <= i < n`) and **flip** `s[i]`. If `s[i] == '1'`, change `s[i]` to `'0'` and vice versa.

You need to **minimize** the length of the **longest**  substring

 of `s` such that all the characters in the substring are **identical**.

Return the **minimum** length after the operations.

**Example 1:**

> **Input:** s = "000001", numOps = 1
> 
> **Output:** 2
> 
> **Explanation:** 
> 
> By changing `s[2]` to `'1'`, `s` becomes `"001001"`. The longest substrings with identical characters are `s[0..1]` and `s[3..4]`.

**Example 2:**

> **Input:** s = "0000", numOps = 2
> 
> **Output:** 1
> 
> **Explanation:** 
> 
> By changing `s[0]` and `s[2]` to `'1'`, `s` becomes `"1010"`.

**Example 3:**

> **Input:** s = "0101", numOps = 0
> 
> **Output:** 1

**Constraints:**

-   `1 <= n == s.length <= 105`
-   `s` consists only of `'0'` and `'1'`.
-   `0 <= numOps <= n`

---
```java
// Java 59ms(Beats 74.07%), Time O(NlogN), Space O(N)
class Solution {
    public int minLength(String s, int numOps) {
        int n = s.length();
        List<Integer> list = new ArrayList<>();
        int i = 0;
        while (i < n)
        {
            int j = i;
            while (j < n && s.charAt(i) == s.charAt(j))
                j++;
            list.add(j - i);
            i = j;
        }

        int l = 1, r = n;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (isOK(s, list, numOps, mid))
                r = mid;
            else
                l = mid + 1;
        }
        return l;
    }

    boolean isOK(String s, List<Integer> list, int numOps, int minLen)
    {
        int n = s.length();
        if (minLen == 1)
        {
            char ch = '0';
            int ops = 0;
            for (int i = 0; i < n; i++) {
                if (s.charAt(i) != ch)
                    ops++;
                ch = ch == '0' ? '1' : '0';
            }

            return Math.min(ops, n - ops) <= numOps; 
        }

        // minLen >= 2
        int count = 0;
        for (int i = 0; i < list.size(); i++) {
            count += Math.ceil((list.get(i) - minLen) * 1.0 / (minLen + 1));
            if (count > numOps)
                return false;
        }

        return true;
    }
}
```
---

參考[【每日一题】LeetCode 3399. Smallest Substring With Identical Characters II - YouTube](https://youtu.be/AdT2F0x-uKo)

只要操作次數越多，就越容易將最長的identical-chracter sbustring長度降下來，所以很適合`binary search by value`的框架

我們將原始字串裡進行預處理，分割為一系列由相同字元組成的子字串
對於任一段長度為`x`的子字串，我們至少要做多少次`flip`呢？
很明顯，貪心思想就可以得到最優解，也就是每隔`len`個字符，我們就做一次`flip`
假設最少做`t`次`flip`，我們需要滿足`x <= (len+1) * t + len `
```java
len = 3
0 0 0 [0] 0 0 0 [0] 0 0 0 [0] 0 0 0
       1         1         1
```

若情況為
```java
len = 3
{0 0 0 [0] 0 0 0 [0] 0 0 0 [0] 0 0 0 [0]} {1 1 1 1 1}
        1         1         1         1
```
會發現最後一個`0`翻成`1`的話, 會影響到下一組的長度
但我們可以透過平移`0`翻成`1`的位置, 而不影響到下一組
`len = 2`也不會影響到下一組
```java
len = 2
{0 0 [0] 0 0 [0] 0 0 [0] 0 0 [0]} {1 1 1 1 1}
      1         1     1       1
平移:
{0 [0] 0 0 [0] 0 0 [0] 0 0 [0] 0} {1 1 1 1 1}
    1         1     1       1
```

因此只有`len = 1`的時候會影響到下一組, 因此`len = 1`需要特別計算


###### tags: `Leetcode` `Binary Search` `Binary Search by Value`