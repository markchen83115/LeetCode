# Leetcode - 347. Top K Frequent Elements (M+)

[Leetcode](https://leetcode.com/problems/top-k-frequent-elements/)

Given an integer array `nums` and an integer `k`, return _the_ `k` _most frequent elements_. You may return the answer in **any order**.

**Example 1:**

> **Input:** nums = [1,1,1,2,2,3], k = 2
> **Output:** [1,2]

**Example 2:**

> **Input:** nums = [1], k = 1
> **Output:** [1]

**Constraints:**

-   `1 <= nums.length <= 105`
-   `-104 <= nums[i] <= 104`
-   `k` is in the range `[1, the number of unique elements in the array]`.
-   It is **guaranteed** that the answer is **unique**.

**Follow up:** Your algorithm's time complexity must be better than `O(n log n)`, where n is the array's size.

---
```java
// Java Quick Select, 10ms(Beats 95.78%), Time O(N), Space O(N)
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        int[] ret = new int[k];
        int n = nums.length;
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int num : nums)
            map.merge(num, 1, Integer::sum);
        
        int[][] arr = new int[map.size()][2];
        int i = 0;
        for (int key : map.keySet())
            arr[i++] = new int[] {map.get(key), key};

        int f = quickSelect(arr, 0, arr.length - 1, k);
        i = 0;
        for (int key : map.keySet())
            if (map.get(key) >= f)
                ret[i++] = key;

        return ret;
        
    }

    int quickSelect(int[][] arr, int a, int b, int k)
    {
        int pivot = arr[(a + b) / 2][0];
        int i = a, j = b, t = a;
        while (t <= j)
        {
            if (arr[t][0] < pivot)
            {
                swap(arr, i, t);
                i++;
                t++;
            } 
            else if (arr[t][0] == pivot)
                t++;
            else
            {
                swap(arr, t, j);
                j--;
            }
        }
        if (b - j >= k)
            return quickSelect(arr, t, b, k);
        else if (b - i + 1 >= k)
            return pivot;
        else 
            return quickSelect(arr, a, i - 1, k - (b - i + 1));
        // S S S P P L L L L
        // a     i j t     b
    }

    void swap(int[][] arr, int i, int j)
    {
        int[] tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}
```
```java
// Java Bucket Sort, 9ms(Beats 97.37%), Time O(N), Space O(N)
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        int[] ret = new int[k];
        int n = nums.length;
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int num : nums)
            map.merge(num, 1, Integer::sum);
        
        List<Integer>[] bucket = new List[n + 1];
        for (int key : map.keySet())
        {
            if (bucket[map.get(key)] == null)
                bucket[map.get(key)] = new ArrayList<>();
            bucket[map.get(key)].add(key);
        }

        int i = 0;
        for (int p = n; p >= 0 && i < k; p--)
            if (bucket[p] != null)
                for (int j = 0; j < bucket[p].size(); j++)
                    ret[i++] = bucket[p].get(j);

        return ret;
    }
}
```
---

`Quick Select`: 用O(N)的時間來時間搜尋第k大元素
每次找一個`pivot`
根據`pivot`的值用三指針算法把整個arr重新調為为三部份(`小於pivot`、`等於pivot`、`大於pivot`)
根據各部分數量與`k`的關係，選擇下一步遞迴需要處理哪個部分

找到這個頻率值`f`之後，再掃一遍全部所有的頻次，`頻率 >= f`的元素就是答案。


###### tags: `Leetcode` `Others` `Quick Select`