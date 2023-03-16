# Leetcode - 2561. Rearranging Fruits (H-)

[Leetcode](https://leetcode.com/problems/rearranging-fruits/description/)

You have two fruit baskets containing `n` fruits each. You are given two **0-indexed** integer arrays `basket1` and `basket2` representing the cost of fruit in each basket. You want to make both baskets **equal**. To do so, you can use the following operation as many times as you want:

-   Chose two indices `i` and `j`, and swap the `ith` fruit of `basket1` with the `jth` fruit of `basket2`.
-   The cost of the swap is `min(basket1[i],basket2[j])`.

Two baskets are considered equal if sorting them according to the fruit cost makes them exactly the same baskets.

Return _the minimum cost to make both the baskets equal or _`-1`_ if impossible._

**Example 1:**
```
Input: basket1 = [4,2,2,2], basket2 = [1,4,1,2]
Output: 1
Explanation: Swap index 1 of basket1 with index 0 of basket2, which has cost 1. Now basket1 = [4,1,2,2] and basket2 = [2,4,1,2]. Rearranging both the arrays makes them equal.
```
**Example 2:**
```
Input: basket1 = [2,3,4,1], basket2 = [3,2,5,1]
Output: -1
Explanation: It can be shown that it is impossible to make both the baskets equal.
```
**Constraints:**

-   `basket1.length == bakste2.length`
-   `1 <= basket1.length <= 105`
-   `1 <= basket1[i],basket2[i] <= 109`

---
```java
// Java
class Solution {
    public long minCost(int[] basket1, int[] basket2) {
        TreeMap<Integer, Integer> map = new TreeMap<>();
        long swapTimes = 0;
        long ret = 0;
        
        for (int i = 0; i < basket1.length; i++) {
            map.put(basket1[i], map.getOrDefault(basket1[i], 0) + 1);
            map.put(basket2[i], map.getOrDefault(basket2[i], 0) - 1);
        }

        int min = map.firstKey();
        ArrayList<Integer> arr = new ArrayList<>();

        for (int i : map.keySet()) {
            int fruitSum = map.get(i);
            if ((fruitSum) % 2 == 1) return -1;
            int times = Math.abs(fruitSum / 2);
            for (int k = 0; k < times; k++) {
                arr.add(i);
            }
        }
        
        for (int i = 0; i < arr.size() / 2; i++) {
            ret += Math.min(arr.get(i), min * 2);
        } 
            
        
        return ret;
    }
}
```

---

`basket1 = [1,1,2,2,1000,1000], basket2 = [3,3,1001,1001,1001,1001]`

假設目前要交換2跟1001
交換有兩種方式
1. 直接交換
   2跟1001直接交換, cost = 2
   
2. 分別跟最小的數交換兩次
   basket1: 用1跟1001交換, cost = 1
   basket2: 用1跟2交換, cost = 1
   total cost = 1 * 2;

   basket1 = [1,1,**2**,2,1000,1000], basket2 = [3,3,**1001**,1001,1001,1001]
   第一次交換:
   basket1 = [**1001**,1,2,2,1000,1000], basket2 = [3,3,**1**,1001,1001,1001]
   第二次交換:
   basket1 = [1001,1,**1**,2,1000,1000], basket2 = [3,3,**2**,1001,1001,1001]






###### tags: `Leetcode` `HashMap`
