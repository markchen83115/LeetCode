# Leetcode - 1942. The Number of the Smallest Unoccupied Chair (M+)

[Leetcode](https://leetcode.com/problems/the-number-of-the-smallest-unoccupied-chair/)

There is a party where `n` friends numbered from `0` to `n - 1` are attending. There is an **infinite** number of chairs in this party that are numbered from `0` to `infinity`. When a friend arrives at the party, they sit on the unoccupied chair with the **smallest number**.

-   For example, if chairs `0`, `1`, and `5` are occupied when a friend comes, they will sit on chair number `2`.

When a friend leaves the party, their chair becomes unoccupied at the moment they leave. If another friend arrives at that same moment, they can sit in that chair.

You are given a **0-indexed** 2D integer array `times` where `times[i] = [arrivali, leavingi]`, indicating the arrival and leaving times of the `ith` friend respectively, and an integer `targetFriend`. All arrival times are **distinct**.

Return_ the **chair number** that the friend numbered _`targetFriend`_ will sit on_.

**Example 1:**
```
Input: times = [[1,4],[2,3],[4,6]], targetFriend = 1
Output: 1
Explanation: 
- Friend 0 arrives at time 1 and sits on chair 0.
- Friend 1 arrives at time 2 and sits on chair 1.
- Friend 1 leaves at time 3 and chair 1 becomes empty.
- Friend 0 leaves at time 4 and chair 0 becomes empty.
- Friend 2 arrives at time 4 and sits on chair 0.
Since friend 1 sat on chair 1, we return 1.
```
**Example 2:**
```
Input: times = [[3,10],[1,5],[2,6]], targetFriend = 0
Output: 2
Explanation: 
- Friend 1 arrives at time 1 and sits on chair 0.
- Friend 2 arrives at time 2 and sits on chair 1.
- Friend 0 arrives at time 3 and sits on chair 2.
- Friend 1 leaves at time 5 and chair 0 becomes empty.
- Friend 2 leaves at time 6 and chair 1 becomes empty.
- Friend 0 leaves at time 10 and chair 2 becomes empty.
Since friend 0 sat on chair 2, we return 2.
```
**Constraints:**

-   `n == times.length`
-   `2 <= n <= 104`
-   `times[i].length == 2`
-   `1 <= arrivali < leavingi <= 105`
-   `0 <= targetFriend <= n - 1`
-   Each `arrivali` time is **distinct**.

---
```java
// Java 48ms(Beats 63.16%), Time O(NlogN), Space O(N)
class Solution {
    public int smallestChair(int[][] times, int targetFriend) {
        int n = times.length;
        Integer[] order = new Integer[n];
        for (int i = 0; i < n; i++)
            order[i] = i;
        Arrays.sort(order, (a, b) -> Integer.compare(times[a][0], times[b][0]));

        PriorityQueue<Integer> emptyChair = new PriorityQueue<>();
        PriorityQueue<int[]> usedChair = new PriorityQueue<>((a, b) -> Integer.compare(a[0], b[0]));
        for (int i = 0; i < n; i++)
            emptyChair.offer(i);

        for (int i : order)
        {
            int arrive = times[i][0], leave = times[i][1];
            while (!usedChair.isEmpty() && usedChair.peek()[0] <= arrive)
                emptyChair.offer(usedChair.poll()[1]);
            
            int sit = emptyChair.poll();
            if (i == targetFriend)
                return sit;
            
            usedChair.offer(new int[] {leave, sit});
        }

        return -1;
    }
}
```
---

維護兩個Priority Queue
一個回傳index最小的`未使用座位`, 一個by時間回傳`使用中座位`

當有人到達時, 先將此到達時間之前的所有`使用中座位`轉為`未使用座位`
再取得index最小的`未使用座位`, 並將其會離開的時間放回`使用中座位`


###### tags: `Leetcode` `Priority Queue` `Dual PQ`