# Leetcode - 1079. Letter Tile Possibilities (M)

[Leetcode](https://leetcode.com/problems/letter-tile-possibilities/)

You have `n`  `tiles`, where each tile has one letter `tiles[i]` printed on it.

Return _the number of possible non-empty sequences of letters_ you can make using the letters printed on those `tiles`.

**Example 1:**

> **Input:** tiles = "AAB"
> **Output:** 8
> **Explanation:** The possible sequences are "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA".

**Example 2:**

> **Input:** tiles = "AAABBC"
> **Output:** 188

**Example 3:**

> **Input:** tiles = "V"
> **Output:** 1

**Constraints:**

-   `1 <= tiles.length <= 7`
-   `tiles` consists of uppercase English letters.

---
```java
// Java 2ms(Beats 87.75%), Time O(2^N), Space O(26)
class Solution {
    public int numTilePossibilities(String tiles) {
        int[] freq = new int[26];
        for (char c : tiles.toCharArray())
            freq[c - 'A']++;
        
        return dfs(freq);
    }

    int dfs(int[] freq)
    {
        int sum = 0;
        for (int i = 0; i < 26; i++)
        {
            if (freq[i] == 0)
                continue;
            freq[i]--;
            sum++;
            sum += dfs(freq);
            freq[i]++;
        }
        return sum;
    }
}
```
---

將tiles轉為每個字母的frequency
每次放入一個字母, 並且sum++
再加上放入這個字母後的結果


###### tags: `Leetcode` `DFS`