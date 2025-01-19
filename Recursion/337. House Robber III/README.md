# Leetcode - 337. House Robber III (M+)

[Leetcode](https://leetcode.com/problems/house-robber-iii/)

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called `root`.

Besides the `root`, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if **two directly-linked houses were broken into on the same night**.

Given the `root` of the binary tree, return _the maximum amount of money the thief can rob **without alerting the police**_.

**Example 1:**

> ![](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)
> 
> **Input:** root = [3,2,3,null,3,null,1]
> **Output:** 7
> **Explanation:** Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.

**Example 2:**

> ![](https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg)
> 
> **Input:** root = [3,4,5,1,3,null,1]
> **Output:** 9
> **Explanation:** Maximum amount of money the thief can rob = 4 + 5 = 9.

**Constraints:**

-   The number of nodes in the tree is in the range `[1, 104]`.
-   `0 <= Node.val <= 104`

---
```java
// Java 0ms(Beats 100.00%), Time O(N), Space O(1)
class Solution {
    public int rob(TreeNode root) {
        int[] ret = dfs(root);
        return Math.max(ret[0], ret[1]);
    }

    int[] dfs(TreeNode root) // 0=不拿 1=拿
    {
        if (root == null)
            return new int[] {0, 0};
        int[] left = dfs(root.left);
        int[] right = dfs(root.right);

        int takeRoot = root.val + left[0] + right[0];

        int option1 = left[0] + right[0];
        int option2 = left[1] + right[0];
        int option3 = left[0] + right[1];
        int option4 = left[1] + right[1];
        int notTakeRoot = Math.max(option1, Math.max(option2, Math.max(option3, option4)));

        return new int[] {notTakeRoot, takeRoot};
    }
}
```
---

從下往上開始計算rob與not rob的最大值
注意:not rob root的話, 也可以not rob leaf
因此有四種狀態要考慮


###### tags: `Leetcode` `Recursion`