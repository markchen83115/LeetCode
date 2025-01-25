# Leetcode - 2948. Make Lexicographically Smallest Array by Swapping Elements (M+)

[Leetcode](https://leetcode.com/problems/make-lexicographically-smallest-array-by-swapping-elements/)

You are given a **0-indexed** array of **positive** integers `nums` and a **positive** integer `limit`.

In one operation, you can choose any two indices `i` and `j` and swap `nums[i]` and `nums[j]` **if** `|nums[i] - nums[j]| <= limit`.

Return _the **lexicographically smallest array** that can be obtained by performing the operation any number of times_.

An array `a` is lexicographically smaller than an array `b` if in the first position where `a` and `b` differ, array `a` has an element that is less than the corresponding element in `b`. For example, the array `[2,10,3]` is lexicographically smaller than the array `[10,2,3]` because they differ at index `0` and `2 < 10`.

**Example 1:**

> **Input:** nums = [1,5,3,9,8], limit = 2
> **Output:** [1,3,5,8,9]
> **Explanation:** Apply the operation 2 times:
> - Swap nums[1] with nums[2]. The array becomes [1,3,5,9,8]
> - Swap nums[3] with nums[4]. The array becomes [1,3,5,8,9]
> We cannot obtain a lexicographically smaller array by applying any more operations.
> Note that it may be possible to get the same result by doing different operations.

**Example 2:**

> **Input:** nums = [1,7,6,18,2,1], limit = 3
> **Output:** [1,6,7,18,1,2]
> **Explanation:** Apply the operation 3 times:
> - Swap nums[1] with nums[2]. The array becomes [1,6,7,18,2,1]
> - Swap nums[0] with nums[4]. The array becomes [2,6,7,18,1,1]
> - Swap nums[0] with nums[5]. The array becomes [1,6,7,18,1,2]
> We cannot obtain a lexicographically smaller array by applying any more operations.

**Example 3:**

> **Input:** nums = [1,7,28,19,10], limit = 3
> **Output:** [1,7,28,19,10]
> **Explanation:** [1,7,28,19,10] is the lexicographically smallest array we can obtain because we cannot apply the operation on any two indices.

**Constraints:**

-   `1 <= nums.length <= 105`
-   `1 <= nums[i] <= 109`
-   `1 <= limit <= 109`

---
```java

class Solution {
// Java 73ms(Beats 88.89%), Time O(NlogN), Space O(N)
public int[] lexicographicallySmallestArray(int[] nums, int limit) {
        int n = nums.length;
        int[] arr = Arrays.copyOf(nums, n);
        Arrays.sort(arr);
        int group = 0;
        HashMap<Integer, Integer> numToGroup = new HashMap<>();
        HashMap<Integer, Queue<Integer>> groupToQueue = new HashMap<>();
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(arr[0]);
        numToGroup.put(arr[0], group);
        groupToQueue.put(group, queue);
        for (int i = 1; i < n; i++)
        {
            if (arr[i] - arr[i - 1] > limit)
            {
                group++;
                queue = new LinkedList<>();
                groupToQueue.put(group, queue);
            }
            numToGroup.putIfAbsent(arr[i], group);
            queue.offer(arr[i]);
        }

        for (int i = 0; i < n; i++)
        {
            group = numToGroup.get(nums[i]);
            queue = groupToQueue.get(group);
            nums[i] = queue.poll();
        }

        return nums;
    }
}
```
---

![image](https://hackmd.io/_uploads/SJBg1Wf_1e.png)
將`nums`複製一個新的`array`出來後, 將新`array`排序並依照`limit`來分組

![image](https://hackmd.io/_uploads/ryaNJWf_1g.png)
最後將`nums[i]`依照該`nums[i]`所屬的群組取出最小值, 並代替`nums[i]`



###### tags: `Leetcode` `Sorted Container`