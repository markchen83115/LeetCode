# Leetcode - 315. Count of Smaller Numbers After Self (H-)

[Leetcode](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

Given an integer array `nums`, return_ an integer array _`counts`_ where _`counts[i]`_ is the number of smaller elements to the right of _`nums[i]`.

**Example 1:**

> **Input:** nums = [5,2,6,1]
> **Output:** [2,1,1,0]
> **Explanation:**
> To the right of 5 there are **2** smaller elements (2 and 1).
> To the right of 2 there is only **1** smaller element (1).
> To the right of 6 there is **1** smaller element (1).
> To the right of 1 there is **0** smaller element.

**Example 2:**

> **Input:** nums = [-1]
> **Output:** [0]

**Example 3:**

> **Input:** nums = [-1,-1]
> **Output:** [0,0]

**Constraints:**

-   `1 <= nums.length <= 105`
-   `-104 <= nums[i] <= 104`

---
```java
// Java 985ms(Beats 14.73%), Time O(NlogN), Space O(N)
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> ret = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < nums.length; i++)
            list.add(nums[i]);
        Collections.sort(list);

        for (int i = 0; i < nums.length; i++)
        {
            int index = binarySearch(list, nums[i]);
            ret.add(index);
            list.remove(index);
        }

        return ret;
    }

    int binarySearch(List<Integer> list, int k)
    {
        int l = 0, r = list.size() - 1;
        while (l < r)
        {
            int mid = l + (r - l) / 2;
            if (list.get(mid) < k)
                l = mid + 1;
            else
                r = mid;
        }
        return l;
    }
}
```
---

參考[【每日一题】315. Count of Smaller Numbers After Self, 11/11/2020 - YouTube](https://youtu.be/z-uLlQMvOVM)

將`nums`排序, 從`原始nums[i]`開始用`binary search`找到`index`
`index`代表比`原始nums[i]`小的數字有幾個, 記得要移除`原始nums[i]`
再進行`nums[i+1]`...以此類推



###### tags: `Leetcode` `Divide and Conquer`