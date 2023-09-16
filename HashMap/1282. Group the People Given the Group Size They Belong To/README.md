# Leetcode - 1282. Group the People Given the Group Size They Belong To (E+)

[Leetcode](https://leetcode.com/problems/group-the-people-given-the-group-size-they-belong-to/)

There are `n` people that are split into some unknown number of groups. Each person is labeled with a **unique ID** from `0` to `n - 1`.

You are given an integer array `groupSizes`, where `groupSizes[i]` is the size of the group that person `i` is in. For example, if `groupSizes[1] = 3`, then person `1` must be in a group of size `3`.

Return _a list of groups such that each person `i` is in a group of size `groupSizes[i]`_.

Each person should appear in **exactly one group**, and every person must be in a group. If there are multiple answers, **return any of them**. It is **guaranteed** that there will be **at least one** valid solution for the given input.

**Example 1:**
```
Input: groupSizes = [3,3,3,3,3,1,3]
Output: [[5],[0,1,2],[3,4,6]]
Explanation: 
The first group is [5]. The size is 1, and groupSizes[5] = 1.
The second group is [0,1,2]. The size is 3, and groupSizes[0] = groupSizes[1] = groupSizes[2] = 3.
The third group is [3,4,6]. The size is 3, and groupSizes[3] = groupSizes[4] = groupSizes[6] = 3.
Other possible solutions are [[2,1,6],[5],[0,4,3]] and [[5],[0,6,2],[4,3,1]].
```
**Example 2:**
```
Input: groupSizes = [2,1,3,3,3,2]
Output: [[1],[0,5],[2,3,4]]
```
**Constraints:**

-   `groupSizes.length == n`
-   `1 <= n <= 500`
-   `1 <= groupSizes[i] <= n`

---
```java
// Java 7ms (Beats 55.72%), Time O(N), Space O(N)
class Solution {
    public List<List<Integer>> groupThePeople(int[] groupSizes) {
        List<List<Integer>> ret = new LinkedList<List<Integer>>();
        Map<Integer, List<Integer>> map = new HashMap<Integer, List<Integer>>();
        for (int i = 0; i < groupSizes.length; i++)
        {
            if (!map.containsKey(groupSizes[i]))
                map.put(groupSizes[i], new LinkedList<Integer>());
            List<Integer> group = map.get(groupSizes[i]);
            group.add(i);

            if (group.size() == groupSizes[i])
            {
                ret.add(new LinkedList<Integer>(group));
                map.remove(groupSizes[i]);
            }
        }

        return ret;
            
    }
}
```
```csharp
// C# 140ms (Beats 45.51%), Time O(N), Space O(N)
public class Solution {
    public IList<IList<int>> GroupThePeople(int[] groupSizes) {
        IList<IList<int>> ret = new List<IList<int>>();
        Dictionary<int, IList<int>> dict = new Dictionary<int, IList<int>>();
        for (int i = 0; i < groupSizes.Length; i++)
        {
            if (!dict.ContainsKey(groupSizes[i]))
                dict.Add(groupSizes[i], new List<int>());
            IList<int> group = dict[groupSizes[i]];
            group.Add(i);

            if (group.Count == groupSizes[i])
            {
                ret.Add(new List<int>(group));
                dict.Remove(groupSizes[i]);
            }
        }

        return ret;
    }
}
```
---

###### tags: `Leetcode` `HashMap`