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
// Java 39ms(Beats 60.77%), Time O(NlogN), Space O(N)
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

```csharp
// C# 190ms(Beats 53.57%), Time O(NlogN), Space O(N)
public class Solution {
    public int[] SmallestRange(IList<IList<int>> nums) {
        PriorityQueue<int[], int> pq = new PriorityQueue<int[], int>();
        int n = nums.Count;
        int max = Int32.MinValue;
        int start = -100000, end = 100000;
        for (int i = 0; i < n; i++) {
            int[] element = new int[] {nums[i][0], 0, i}; // num, index, group
            max = Math.Max(max, nums[i][0]);
            pq.Enqueue(element, element[0]);
        }

        while (pq.Count == n) {
            int[] element = pq.Dequeue();
            int min = element[0], index = element[1], group = element[2];
            // update result
            if (end - start > max - min) {
                start = min;
                end = max;
            }

            // add new element
            if (++index < nums[group].Count) {
                int[] newElement = new int[] {nums[group][index], index, group};
                pq.Enqueue(newElement, newElement[0]);
                max = Math.Max(max, nums[group][index]);
            }
        }

        return new int[] {start, end};
    }
}
```

---

[wisdompeak/YouTube](https://www.youtube.com/watch?v=ejVD92bJe34)

```
[4,10,15,24,26]
[0,9,12,20]
[5,18,22,30]

[0, 4, 5] -> 剔除最小的0
從剔除的那組找下一個 -> [0,9,12,20] 找到9放入PQ
```

###### tags: `Leetcode` `Heap`