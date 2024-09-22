# Leetcode - 386. Lexicographical Numbers (H)

[Leetcode](https://leetcode.com/problems/lexicographical-numbers/)

Given an integer `n`, return all the numbers in the range `[1, n]` sorted in lexicographical order.

You must write an algorithm that runs in `O(n)` time and uses `O(1)` extra space. 

**Example 1:**
```
Input: n = 13
Output: [1,10,11,12,13,2,3,4,5,6,7,8,9]
```
**Example 2:**
```
Input: n = 2
Output: [1,2]
```
**Constraints:**

-   `1 <= n <= 5 * 104`

---
```java
// Java DFS 1ms (Beats 100.00%), Time O(N), Space O(1)

class Solution {
    List<Integer> ret = new ArrayList<>();
    public List<Integer> lexicalOrder(int n) {
        for (int i = 1; i < 10; i++)
            dfs(i, n);
        return ret;
    }

    void dfs(int i, int n)
    {
        if (i > n)
            return;
        ret.add(i);
        for (int j = 0; j < 10; j++)
        {
            int k = i * 10 + j;
            if (k > n)
                return;
            dfs(k, n);
        }
    }
}  
```
```java
// Java 6ms(Beats 25.95%), Time O(N), Space O(1)
class Solution {
    public List<Integer> lexicalOrder(int n) {
        List<Integer> ret = new ArrayList<>();
        ret.add(1);
        int i = 1;
        while (ret.size() < n)
        {
            if (i * 10 <= n)
            {
                i *= 10;
                ret.add(i);
            }
            else
            {
                while (i+1 > n || (i % 10) == 9)
                    i /= 10;
                i++;
                ret.add(i);
            }
        }

        return ret;
    }
}
```

---
解1: DFS
```
       1        2        3    ...
      /\        /\       /\
   10 ...19  20...29  30...39   ....
```

解2:
對於字典序列的next，核心就是
1.  嘗試往後加0, 否则
2.  找最低的、加1不需要進位的位置，在該位置加1後，捨棄之後的位置即可。

###### tags: `Leetcode` `Greedy`