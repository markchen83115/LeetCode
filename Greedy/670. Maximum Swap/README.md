# Leetcode - 670. Maximum Swap (M+)

[Leetcode](https://leetcode.com/problems/maximum-swap/)

You are given an integer `num`. You can swap two digits at most once to get the maximum valued number.

Return _the maximum valued number you can get_.

**Example 1:**
```
Input: num = 2736
Output: 7236
Explanation: Swap the number 2 and the number 7.
```
**Example 2:**
```
Input: num = 9973
Output: 9973
Explanation: No swap.
```
**Constraints:**

-   `0 <= num <= 108`

---
```java
// Java 1ms(Beats 30.39%), Time O(N), Space O(N)
class Solution {
    public int maximumSwap(int num) {
        char[] arr = ("" + num).toCharArray();
        int n = arr.length;
        int[] last = new int[10];
        for (int i = 0; i < n; i++)
            last[arr[i] - '0'] = i;

        for (int i = 0; i < n; i++) 
            for (int d = 9; d > arr[i] - '0'; d--)
            {
                if (i < last[d])
                {
                    //swap i max
                    char tmp = arr[i];
                    arr[i] = arr[last[d]];
                    arr[last[d]] = tmp;
                    return Integer.valueOf(new String(arr));
                }
            }
            
        return num;
    }
}
```

```java
// Java 2ms(Beats 8.36%), Time O(NlogN), Space O(N)
class Solution {
    public int maximumSwap(int num) {
        char[] arr = ("" + num).toCharArray();
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> (a[0] == b[0] ? b[1] - a[1] : b[0] - a[0]));
        int n = arr.length;
        for (int i = 0; i < n; i++)
            pq.offer(new int[] {arr[i] - '0', i});
        
        for (int i = 0; i < n; i++) 
        {
            if (pq.isEmpty())
                return num;
            
            int[] p = pq.peek();
            if (arr[i] - '0' >= p[0])
                continue;

            if (p[1] <= i)
            {
                pq.poll();
                i--;
                continue;
            }

            //swap i max
            char tmp = arr[i];
            arr[i] = (char) ('0' + p[0]);
            arr[p[1]] = tmp;
            return Integer.valueOf(new String(arr));
        }

        return num;
    }
}
```
---

###### tags: `Leetcode` `Greedy`