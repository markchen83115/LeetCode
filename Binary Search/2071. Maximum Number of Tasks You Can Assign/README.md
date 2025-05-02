# Leetcode - 2071. Maximum Number of Tasks You Can Assign (H)

[Leetcode](https://leetcode.com/problems/maximum-number-of-tasks-you-can-assign/)

You have `n` tasks and `m` workers. Each task has a strength requirement stored in a **0-indexed** integer array `tasks`, with the `ith` task requiring `tasks[i]` strength to complete. The strength of each worker is stored in a **0-indexed** integer array `workers`, with the `jth` worker having `workers[j]` strength. Each worker can only be assigned to a **single** task and must have a strength **greater than or equal** to the task's strength requirement (i.e., `workers[j] >= tasks[i]`).

Additionally, you have `pills` magical pills that will **increase a worker's strength** by `strength`. You can decide which workers receive the magical pills, however, you may only give each worker **at most one** magical pill.

Given the **0-indexed **integer arrays `tasks` and `workers` and the integers `pills` and `strength`, return _the **maximum** number of tasks that can be completed._

**Example 1:**

> **Input:** tasks = [**3**,**2**,**1**], workers = [**0**,**3**,**3**], pills = 1, strength = 1
> **Output:** 3
> **Explanation:**
> We can assign the magical pill and tasks as follows:
> - Give the magical pill to worker 0.
> - Assign worker 0 to task 2 (0 + 1 >= 1)
> - Assign worker 1 to task 1 (3 >= 2)
> - Assign worker 2 to task 0 (3 >= 3)

**Example 2:**

> **Input:** tasks = [**5**,4], workers = [**0**,0,0], pills = 1, strength = 5
> **Output:** 1
> **Explanation:**
> We can assign the magical pill and tasks as follows:
> - Give the magical pill to worker 0.
> - Assign worker 0 to task 0 (0 + 5 >= 5)

**Example 3:**

> **Input:** tasks = [**10**,**15**,30], workers = [**0**,**10**,10,10,10], pills = 3, strength = 10
> **Output:** 2
> **Explanation:**
> We can assign the magical pills and tasks as follows:
> - Give the magical pill to worker 0 and worker 1.
> - Assign worker 0 to task 0 (0 + 10 >= 10)
> - Assign worker 1 to task 1 (10 + 10 >= 15)
> The last pill is not given because it will not make any worker strong enough for the last task.

**Constraints:**

-   `n == tasks.length`
-   `m == workers.length`
-   `1 <= n, m <= 5 * 104`
-   `0 <= pills <= m`
-   `0 <= tasks[i], workers[j], strength <= 109`

---
```java
// Java 773ms(Beats 60.82%), Time O(NlogN), Space O(N)
class Solution {
    public int maxTaskAssign(int[] tasks, int[] workers, int pills, int strength) {
        Arrays.sort(tasks);
        Arrays.sort(workers);
        int left = 0, right = Math.min(tasks.length, workers.length);
        while (left < right)
        {
            int mid = right - (right - left) / 2;
            if (isOK(tasks, workers, pills, strength, mid))
                left = mid;
            else
                right = mid - 1;
        }
        return left;
    }

    boolean isOK(int[] tasks, int[] workers, int pills, int strength, int k)
    {
        TreeMap<Integer, Integer> ws = new TreeMap<>();
        for (int i = workers.length - k; i < workers.length; i++)
            ws.merge(workers[i], 1, Integer::sum);
        
        for (int i = k - 1; i >= 0; i--)
        {
            Integer key = ws.lastKey();
            if (key >= tasks[i])
                ws.computeIfPresent(key, (a, b) -> b > 1 ? b - 1 : null);
            else
            {
                if (pills == 0)
                    return false;
                key = ws.ceilingKey(tasks[i] - strength);
                if (key == null)
                    return false;
                pills--;
                ws.computeIfPresent(key, (a, b) -> b > 1 ? b - 1 : null);
            }
        }
        return true;
    }
}
```
---

參考[【每日一题】LeetCode 2071. Maximum Number of Tasks You Can Assign - YouTube](https://www.youtube.com/watch?v=1f1vV0oYFFs)

這題如果正面硬剛的話比較棘手。對於任何一個task，我們首先不清楚它是否一定要被選中。即使要選中，它是應該分給一個能夠單幹的工人，還是分給一個吃了大力丸的工人？同樣，對於任何一個工人，我們也不清楚他是否一定要被選中，或者他是選擇單幹一個task，還是吃一個大力丸再找一個task。

因為本題的解其實是有明確的範圍的，也就是從0件到所有的task。所以我們可以考慮二分搜值，也就是猜測我們能夠完成k件，看看是否能夠有一個合法的方法。

我們發現如果給定了完成k件的指標，就多了很多資訊。首先這k件任務一定是難度最低的k件。我們先考慮其中難度最大的。這個難度最大的任務應該是必須完成的，所以它首先會找能力最強的worker，這是因為最強能力的工人如果不去做最難的任務，那麼做其他簡單任務就是浪費能力。於是就會有兩種情況：

1. 目前最強工人能夠單幹解決它，那麼就把這個任務和工人配對，記得將已經配對的工人剔除。
2. 如果目前最強工人不能單幹解決它，那麼意味著我們必須要找一個工人吃大力丸來解決。顯然，我們不一定要找最強工人去吃大力丸，我們只需要找一個剛剛好的工人，使得`worker+strength >= task`即可。於是這就提示我們需要將所有的worker排好序，用lower_bound來找到這個剛剛好的工人。完成這次配對之後，這位工人必須剔除。考慮到以後處理其他task時我們可能還需要剩餘的工人保持有序狀態，所以我們必須使用類似multiset的資料結構來保證刪除一個元素之後仍然自動有序。

透過以上方法，就可以貪心地確認每個task應該分給哪個工人。如果指定的k任務都能順利分配，那麼二分搜值的checkOK就可以回傳true，可以考慮在下一個回合提升k；反之就要減少k。


###### tags: `Leetcode` `Binary Search` `Binary Search by Value`