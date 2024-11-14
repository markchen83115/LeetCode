# Leetcode - 1448. Count Good Nodes in Binary Tree (M-)

[Leetcode](https://leetcode.com/problems/count-good-nodes-in-binary-tree/)

Given a binary tree `root`, a node _X_ in the tree is named **good** if in the path from root to _X_ there are no nodes with a value _greater than_ X.

Return the number of **good** nodes in the binary tree.

**Example 1:**

> **![](https://assets.leetcode.com/uploads/2020/04/02/test_sample_1.png)**
> 
> **Input:** root = [3,1,4,3,null,1,5]
> **Output:** 4
> **Explanation:** Nodes in blue are **good**.
> Root Node (3) is always a good node.
> Node 4 -> (3,4) is the maximum value in the path starting from the root.
> Node 5 -> (3,4,5) is the maximum value in the path
> Node 3 -> (3,1,3) is the maximum value in the path.

**Example 2:**

> **![](https://assets.leetcode.com/uploads/2020/04/02/test_sample_2.png)**
> 
> **Input:** root = [3,3,null,4,2]
> **Output:** 3
> **Explanation:** Node 2 -> (3, 3, 2) is not good, because "3" is higher than it.

**Example 3:**

> **Input:** root = [1]
> **Output:** 1
> **Explanation:** Root is considered as **good**.

**Constraints:**

-   The number of nodes in the binary tree is in the range `[1, 10^5]`.
-   Each node's value is between `[-10^4, 10^4]`.

---
```java
// Java 2ms(Beats 85.68%), Time O(N), Space O(1)
class Solution {
    int ret = 0;
    public int goodNodes(TreeNode root) {
        dfs(root, root.val);
        return ret;
    }

    void dfs(TreeNode root, int maxVal)
    {
        if (root.val >= maxVal)
            ret++;
        if (root.left != null)
            dfs(root.left, Math.max(maxVal, root.left.val));
        if (root.right != null)
            dfs(root.right, Math.max(maxVal, root.right.val));
    }
}
```
---

透過DFS, 取得`最大值 = Math.max(當前最大值, root.val)`後往下傳
即可比較`當前node.val是否>=最大值`


###### tags: `Leetcode` `Tree`