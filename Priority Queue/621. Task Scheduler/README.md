# Leetcode - 621. Task Scheduler (H-)

[Leetcode](https://leetcode.com/problems/task-scheduler/description/)

Given a characters array `tasks`, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle.

However, there is a non-negative integer `n` that represents the cooldown period between two **same tasks** (the same letter in the array), that is that there must be at least `n` units of time between any two same tasks.

Return _the least number of units of times that the CPU will take to finish all the given tasks_.

**Example 1:**
```
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: 
A -> B -> idle -> A -> B -> idle -> A -> B
There is at least 2 units of time between any two same tasks.
```
**Example 2:**
```
Input: tasks = ["A","A","A","B","B","B"], n = 0
Output: 6
Explanation: On this case any permutation of size 6 would work since n = 0.
["A","A","A","B","B","B"]
["A","B","A","B","A","B"]
["B","B","B","A","A","A"]
...
And so on.
```
**Example 3:**
```
Input: tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2
Output: 16
Explanation: 
One possible solution is
A -> B -> C -> A -> D -> E -> A -> F -> G -> A -> idle -> idle -> A -> idle -> idle -> A
```
**Constraints:**

-   `1 <= task.length <= 104`
-   `tasks[i]` is upper-case English letter.
-   The integer `n` is in the range `[0, 100]`.

---
```java
// Java 1ms (Beats 100%), Time O(N), Space O(N)
class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] freq = new int[26];
        int maxFreq = 0;
        int maxFreqCount = 0;

        for (char t : tasks)
            freq[t - 'A']++;

        // find max frequency
        for (int i = 0; i < 26; i++)
            maxFreq = Math.max(maxFreq, freq[i]);

        // find how many "max frequency" task
        for (int i = 0; i < 26; i++)
            if (freq[i] == maxFreq)
                maxFreqCount++;

        return Math.max(tasks.length, (maxFreq - 1) * (n + 1) + maxFreqCount);
    }
}
```
---

假設目前有`3A, 3B, 3C, 2D, n = 3`
代表最短至少為`A ? ? ? A ? ? ? A`

還需要找出有多少個同為最長的task
故為`A B C`個數皆為3
因此最短會是`A ? ? ? A ? ? ? A B C`

但若n較小, `ex: n = 1`
因此會是`A ? A ? A B C`, 最短的長度會是`tasks.length`


###### tags: `Leetcode` `Priority Queue` `Arrangement with Stride`