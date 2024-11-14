# Leetcode - 3307. Find the K-th Character in String Game II (M+)

[Leetcode](https://leetcode.com/problems/find-the-k-th-character-in-string-game-ii/)

Alice and Bob are playing a game. Initially, Alice has a string `word = "a"`.

You are given a **positive** integer `k`. You are also given an integer array `operations`, where `operations[i]` represents the **type** of the `ith` operation.

Now Bob will ask Alice to perform **all** operations in sequence:

-   If `operations[i] == 0`, **append** a copy of `word` to itself.
-   If `operations[i] == 1`, generate a new string by **changing** each character in `word` to its **next** character in the English alphabet, and **append** it to the _original_ `word`. For example, performing the operation on `"c"` generates `"cd"` and performing the operation on `"zb"` generates `"zbac"`.

Return the value of the `kth` character in `word` after performing all the operations.

**Note** that the character `'z'` can be changed to `'a'` in the second type of operation.

**Example 1:**

> **Input:** k = 5, operations = [0,0,0]
> 
> **Output:** "a"
> 
> **Explanation:**
> 
> Initially, `word == "a"`. Alice performs the three operations as follows:
> 
> -   Appends `"a"` to `"a"`, `word` becomes `"aa"`.
> -   Appends `"aa"` to `"aa"`, `word` becomes `"aaaa"`.
> -   Appends `"aaaa"` to `"aaaa"`, `word` becomes `"aaaaaaaa"`.

**Example 2:**

> **Input:** k = 10, operations = [0,1,0,1]
> 
> **Output:** "b"
> 
> **Explanation:**
> 
> Initially, `word == "a"`. Alice performs the four operations as follows:
> 
> -   Appends `"a"` to `"a"`, `word` becomes `"aa"`.
> -   Appends `"bb"` to `"aa"`, `word` becomes `"aabb"`.
> -   Appends `"aabb"` to `"aabb"`, `word` becomes `"aabbaabb"`.
> -   Appends `"bbccbbcc"` to `"aabbaabb"`, `word` becomes `"aabbaabbbbccbbcc"`.

**Constraints:**

-   `1 <= k <= 1014`
-   `1 <= operations.length <= 100`
-   `operations[i]` is either 0 or 1.
-   The input is generated such that `word` has **at least** `k` characters after all operations.

---
```java
// Java 1ms(Beats 100.00%), Time O(logN), Space O(1)
class Solution {
    public char kthCharacter(long k, int[] operations) {
        int change = 0;
        while (k > 1)
        {
            long cnt = 1;
            int oprt = 0;
            while (cnt * 2 < k)
            {
                cnt *= 2;
                oprt++;
            }

            k -= cnt;
            change += operations[oprt];
        }

        
        char ret = (char) ('a' + (change % 26));

        return ret;
    }
}
// a [a] b b a a b b | b [b] c c b b c c
// a [a] b b a a b b

// a [a] b b | a a b b
// a [a] b b
```
---
```
cnt = 8
a [a] b b a a b b | b [b] c c b b c c
  i-cnt                i
a [a] b b a a b b

cnt = 1
   a   [a]
i-cnt   i
  a | [a]
```
考察長度`< k`的該次operation是否為1, 因為該次`operation * 2`後將會產生k
一系列逆推之後則可知道`operation = 1`總共有幾次
因此`k = 'a' + (operation = 1的次數)`


###### tags: `Leetcode` `Recursion` `Digit counting & finding`