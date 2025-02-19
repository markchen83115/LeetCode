# Leetcode - 1415. The k-th Lexicographical String of All Happy Strings of Length n (H-)

[Leetcode](https://leetcode.com/problems/the-k-th-lexicographical-string-of-all-happy-strings-of-length-n/)

A **happy string** is a string that:

-   consists only of letters of the set `['a', 'b', 'c']`.
-   `s[i] != s[i + 1]` for all values of `i` from `1` to `s.length - 1` (string is 1-indexed).

For example, strings **"abc", "ac", "b"** and **"abcbabcbcb"** are all happy strings and strings **"aa", "baa"** and **"ababbc"** are not happy strings.

Given two integers `n` and `k`, consider a list of all happy strings of length `n` sorted in lexicographical order.

Return _the kth string_ of this list or return an **empty string** if there are less than `k` happy strings of length `n`.

**Example 1:**

> **Input:** n = 1, k = 3
> **Output:** "c"
> **Explanation:** The list ["a", "b", "c"] contains all happy strings of length 1. The third string is "c".

**Example 2:**

> **Input:** n = 1, k = 4
> **Output:** ""
> **Explanation:** There are only 3 happy strings of length 1.

**Example 3:**

> **Input:** n = 3, k = 9
> **Output:** "cab"
> **Explanation:** There are 12 different happy string of length 3 ["aba", "abc", "aca", "acb", "bab", "bac", "bca", "bcb", "cab", "cac", "cba", "cbc"]. You will find the 9th string = "cab"

**Constraints:**

-   `1 <= n <= 10`
-   `1 <= k <= 100`

---
```java
// Java 1ms(Beats 99.09%), Time O(k * 2^N), Space O(N)
class Solution {
    StringBuilder sb = new StringBuilder();
    String ret = "";
    int k;
    public String getHappyString(int n, int k) {
        this.k = k;
        dfs(n, 0);
        return ret;
    }

    void dfs(int n, int pos)
    {
        if (pos == n)
        {
            if (--k == 0)
                ret = sb.toString();
            return;
        }
            
        for (char i = 'a'; i <= 'c'; i++)
        {
            if (pos > 0 && i == sb.charAt(pos - 1))
                continue;
            sb.append(i);
            dfs(n, pos + 1);
            if (!ret.isEmpty())
                return;
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```
---

#### 解法1

[ ](https://github.com/wisdompeak/LeetCode/tree/master/Recursion/1415.The-k-th-Lexicographical-String-of-All-Happy-Strings-of-Length-n#%E8%A7%A3%E6%B3%951)

考慮到k只有不到100，可以暴力列舉從小到大所有合法的字串，取第k個。

#### 解法2

[ ](https://github.com/wisdompeak/LeetCode/tree/master/Recursion/1415.The-k-th-Lexicographical-String-of-All-Happy-Strings-of-Length-n#%E8%A7%A3%E6%B3%952)

更聰明點的遞歸。

當我們嘗試填入長度為n的字串的首字母時，無論首字母是什麼，之後的n-1位元都有pow(2,n-1)種填寫方法。所以我們用`t = k/pow(2,n-1)`就可以確定此時的首字母ch應該是字母表的第幾個。注意這裡的k和t都用0-index比較方便。例如t=0，那麼ch應該就是'a'，如果t=1，那麼ch應該就是'b'.

但是我們還需要考慮到之前一位的製約。如果發現計算得到的ch比上一位字母大，那麼就代表實際填寫的ch還需要再加1。例如，上一個位置是'a'，本輪計算得到`t=1`，意味著我們需要跳過`2^(n-1)`種排列。但注意這`2^(n-1)`種排列並不是對應的`axx..xx`，因為它與上一個位置'a'衝突。所以我們只能認為這`2^(n-1)`種排列對應的是`bxx..xx`。故跳過他們之後，我們認為本位置必須是填寫`c`.


###### tags: `Leetcode` `Recursion` `Digit counting & finding`