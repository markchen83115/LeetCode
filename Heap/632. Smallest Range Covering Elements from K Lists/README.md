# Leetcode - 632. Smallest Range Covering Elements from K Lists (H-)

[Leetcode](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/description/)

You have `k` lists of sorted integers in **non-decreasing order**. Find the **smallest** range that includes at least one number from each of the `k` lists.

We define the range `[a, b]` is smaller than range `[c, d]` if `b - a < d - c` **or** `a < c` if `b - a == d - c`.

**Example 1:**
```
Input: nums = [[4,10,15,24,26],[0,9,12,20],[5,18,22,30]]
Output: [20,24]
Explanation: 
List 1: [4, 10, 15, 24,26], 24 is in range [20,24].
List 2: [0, 9, 12, 20], 20 is in range [20,24].
List 3: [5, 18, 22, 30], 22 is in range [20,24].
```
**Example 2:**
```
Input: nums = [[1,2,3],[1,2,3],[1,2,3]]
Output: [1,1]
```
**Constraints:**

-   `nums.length == k`
-   `1 <= k <= 3500`
-   `1 <= nums[i].length <= 50`
-   `-105 <= nums[i][j] <= 105`
-   `nums[i]` is sorted in **non-decreasing** order.

---
```java
// Java PQ 39ms(Beats 60.77%), Time O(NlogN), Space O(N)
class Solution {
    public int[] smallestRange(List<List<Integer>> nums) {
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> (a[0] - b[0]));
        int n = nums.size();
        int max = Integer.MIN_VALUE;
        int start = -100000, end = 100000;
        for (int i = 0; i < n; i++) {
            int[] element = new int[] {nums.get(i).get(0), 0, i}; // num, index, group
            max = Math.max(max, nums.get(i).get(0));
            pq.offer(element);
        }

        while (pq.size() == n) {
            int[] element = pq.poll();
            int min = element[0], index = element[1], group = element[2];
            // update result
            if (end - start > max - min) {
                start = min;
                end = max;
            }

            // add new element
            if (++index < nums.get(group).size()) {
                int[] newElement = new int[] {nums.get(group).get(index), index, group};
                pq.offer(newElement);
                max = Math.max(max, nums.get(group).get(index));
            }
        }

        return new int[] {start, end};
    }
}
```
```java
// Java Sliding Window 66ms(Beats 95.84%), Time O(NlogN), Space O(N)
class Solution {
    public int[] smallestRange(List<List<Integer>> nums) {
        int need = nums.size();
        int[] freq = new int[need];
        List<int[]> list = new ArrayList<>();
        for (int group = 0; group < nums.size(); group++)
            for (int num : nums.get(group))
                list.add(new int[] {num, group});

        Collections.sort(list, (a, b) -> a[0] - b[0]);

        int n = list.size();
        int start = -100000, end = 100000;
        int j = 0;
        for (int i = 0; i < n; i++)
        {
            while (j < n && need > 0)
            {
                int group = list.get(j)[1];
                if (++freq[group] == 1)
                    need--;
                j++;
            }

            if (need == 0 && list.get(j - 1)[0] - list.get(i)[0] < end - start)
            {
                start = list.get(i)[0];
                end = list.get(j - 1)[0];
            }

            int group = list.get(i)[1];
            if (--freq[group] == 0)
                need++;
        }

        return new int[] {start, end};
    }
}
```

---

#### Sorted Container
參考[【每日一题】632. Smallest Range Covering Elements from K Lists, 12/3/2020 - YouTube](https://youtu.be/ejVD92bJe34)
```
[4,10,15,24,26]
[0,9,12,20]
[5,18,22,30]

[0, 4, 5] -> 剔除最小的0
從剔除的那組找下一個 -> [0,9,12,20] 找到9放入PQ
```

#### Sliding Window
![image](https://hackmd.io/_uploads/Hk0VDUOcJl.png)
將每個數字及分組放入List中, 並依數字從小到大排序
若分組不夠時, 則右指針往右, 擴大範圍
若分組已夠時, 則左指針往左, 縮小範圍


###### tags: `Leetcode` `Sorted Container`