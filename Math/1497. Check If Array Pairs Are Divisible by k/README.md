# Leetcode - 1497. Check If Array Pairs Are Divisible by k (M-)

[Leetcode](https://leetcode.com/problems/check-if-array-pairs-are-divisible-by-k/)

Given an array of integers `arr` of even length `n` and an integer `k`.

We want to divide the array into exactly `n / 2` pairs such that the sum of each pair is divisible by `k`.

Return `true`_ If you can find a way to do that or _`false`_ otherwise_.

**Example 1:**
```
Input: arr = [1,2,3,4,5,10,6,7,8,9], k = 5
Output: true
Explanation: Pairs are (1,9),(2,8),(3,7),(4,6) and (5,10).
```
**Example 2:**
```
Input: arr = [1,2,3,4,5,6], k = 7
Output: true
Explanation: Pairs are (1,6),(2,5) and(3,4).
```
**Example 3:**
```
Input: arr = [1,2,3,4,5,6], k = 10
Output: false
Explanation: You can try all possible pairs to see that there is no way to divide arr into 3 pairs each with sum divisible by 10.
```
**Constraints:**

-   `arr.length == n`
-   `1 <= n <= 105`
-   `n` is even.
-   `-109 <= arr[i] <= 109`
-   `1 <= k <= 105`

---
```java
//Java 4ms(Beats 98.97%), Time O(N), Space O(N)

class Solution {
    public boolean canArrange(int[] arr, int k) {
        int[] freq = new int[k];
        int n = arr.length;
        for (int a : arr)
            freq[(a % k + k) % k]++;
        
        if (freq[0] % 2 != 0)
            return false;
        
        for (int i = 1; i <= k / 2; i++)
            if (freq[i] != freq[k-i])
                return false;

        return true;
    }
}
```
---

將所有數字取餘數, 並且考慮負數, 因此使用`(a % k + k) % k`取餘數
檢查:
1. `餘0`的個數為偶數
2. 其他`餘i`與`餘k-i`的個數需相同

###### tags: `Leetcode` `Math`