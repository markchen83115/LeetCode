# Leetcode - 437. Path Sum III (H-)

[Leetcode](https://leetcode.com/problems/path-sum-iii/description/)

Given the `root` of a binary tree and an integer `targetSum`, return _the number of paths where the sum of the values along the path equals_ `targetSum`.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)
```
Input: root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
Output: 3
Explanation: The paths that sum to 8 are shown.
```
**Example 2:**
```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: 3
```
**Constraints:**

-   The number of nodes in the tree is in the range `[0, 1000]`.
-   `-109 <= Node.val <= 109`
-   `-1000 <= targetSum <= 1000`

---

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
// Java
class Solution {
    int ret = 0;
    HashMap<Long, Integer> map = new HashMap<>();
    public int pathSum(TreeNode root, int targetSum) {
        map.put(0L, 1);
        dfs(root, targetSum, 0);
        return ret;
    }

    public void dfs(TreeNode root, int targetSum, long currSum) {
        if (root == null) return;
        currSum += root.val;
        ret += map.getOrDefault(currSum - targetSum, 0);
        
        map.put(currSum, map.getOrDefault(currSum, 0) + 1);
        dfs(root.left, targetSum, currSum);
        dfs(root.right, targetSum, currSum);
        map.put(currSum, map.get(currSum) - 1);
    }
}
```
---

利用prefix sum + DFS求解

注意:
```
測試案例:
root = [1000000000,1000000000,null,294967296,null,1000000000,
        null,1000000000,null,1000000000]

targetSum = 0
```
會造成prefix sum 超過int上限, 因此改用long


###### tags: `Leetcode` `Tree`
