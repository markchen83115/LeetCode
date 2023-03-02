# Leetcode - 124. Binary Tree Maximum Path Sum (M)

[Leetcode](https://leetcode.com/problems/binary-tree-maximum-path-sum/description/)

A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return _the maximum **path sum** of any **non-empty** path_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)
```
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)
```
Input: root = [-10,9,20,null,null,15,7]
Output: 42
Explanation: The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
```
**Constraints:**

-   The number of nodes in the tree is in the range `[1, 3 * 104]`.
-   `-1000 <= Node.val <= 1000`

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
// Java Time O(N) 1ms
class Solution {
    int ret = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        maxDownPathSum(root);
        return ret;
    }

    // only can move downward from root
    public int maxDownPathSum(TreeNode root) {
        if (root == null) 
            return 0;
        int left = maxDownPathSum(root.left);
        int right = maxDownPathSum(root.right);

        int maxTurnSum = Math.max(left, 0) + Math.max(right, 0) + root.val;
        ret = Math.max(ret, maxTurnSum);
        
        if (left < 0 && right < 0) {
            return root.val;
        } else {
            return Math.max(left, right) + root.val;
        }
    }
}
```

---

[wisdompeak/YouTube](https://www.youtube.com/watch?v=cuneDTWejTw)

核心思想為如何遍歷整棵樹的路徑
利用找一條只能往下的路徑和的DFS, 求算從root往下的最大路徑和
中間可得其副產品`root + leftSum + rightSum`, 為以root為拐點的最大路徑和
意即DFS時可得以所有點作為拐點時的最大路徑和 = 樹的所有路徑和


###### tags: `Leetcode` `Tree` `Path in a tree`
