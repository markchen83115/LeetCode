# Leetcode - 1123. Lowest Common Ancestor of Deepest Leaves (M+)

[Leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/)

Given the `root` of a binary tree, return _the lowest common ancestor of its deepest leaves_.

Recall that:

-   The node of a binary tree is a leaf if and only if it has no children
-   The depth of the root of the tree is `0`. if the depth of a node is `d`, the depth of each of its children is `d + 1`.
-   The lowest common ancestor of a set `S` of nodes, is the node `A` with the largest depth such that every node in `S` is in the subtree with root `A`.

**Example 1:**

> ![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/01/sketch1.png)
> 
> **Input:** root = [3,5,1,6,2,0,8,null,null,7,4]
> **Output:** [2,7,4]
> **Explanation:** We return the node with value 2, colored in yellow in the diagram.
> The nodes coloured in blue are the deepest leaf-nodes of the tree.
> Note that nodes 6, 0, and 8 are also leaf nodes, but the depth of them is 2, but the depth of nodes 7 and 4 is 3.

**Example 2:**

> **Input:** root = [1]
> **Output:** [1]
> **Explanation:** The root is the deepest node in the tree, and it's the lca of itself.

**Example 3:**

> **Input:** root = [0,1,3,null,2]
> **Output:** [2]
> **Explanation:** The deepest leaf node in the tree is 2, the lca of one node is itself.

**Constraints:**

-   The number of nodes in the tree will be in the range `[1, 1000]`.
-   `0 <= Node.val <= 1000`
-   The values of the nodes in the tree are **unique**.

**Note:** This question is the same as 865: [https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/](https://leetcode.com/problems/smallest-subtree-with-all-the-deepest-nodes/)

---
```java
// Java 0ms(Beats 100.00%), Time O(N), Space O(1)
class Solution {
    Integer maxDepth = -1;
    TreeNode ret;
    public TreeNode lcaDeepestLeaves(TreeNode root) {
        dfs(root, 0);

        return ret;
    }

    int dfs(TreeNode root, int depth)
    {
        maxDepth = Math.max(maxDepth, depth);
        
        if (root == null)
            return depth;
        
        int left = dfs(root.left, depth + 1);
        int right = dfs(root.right, depth + 1);
        if (left == right && left == maxDepth)
            ret = root;
        return Math.max(left, right);
    }
}
```
```java
// Java 1ms(Beats 32.65%), Time O(N), Space O(1)
class Solution {
    Integer maxDepth = -1;
    int[] depthArr = new int[1001];
    TreeNode ret = null;
    public TreeNode lcaDeepestLeaves(TreeNode root) {
        dfs1(root, 0);
        dfs2(root, 0);

        return ret;
    }

    void dfs1(TreeNode root, int depth)
    {
        depthArr[depth]++;
        maxDepth = Math.max(maxDepth, depth);

        if (root.left != null)
            dfs1(root.left, depth + 1);
        if (root.right != null)
            dfs1(root.right, depth + 1);
    }

    int dfs2(TreeNode root, int depth)
    {
        if (root == null)
            return 0;
        int count = dfs2(root.left, depth + 1) + dfs2(root.right, depth + 1);
        if (depth == maxDepth)
            count++;
        if (ret == null && depthArr[maxDepth] == count)
            ret = root;

        return count;
    }
}
```
---


解法1:
比較左右兩葉子節點的最大深度, 若左邊深度 = 右邊深度 = 最大深度, 則該點為LCA

解法2:
參考[【每日一题】LeetCode 1123. Lowest Common Ancestor of Deepest Leaves - YouTube](https://www.youtube.com/watch?v=DUXvcoEZJqw)

提前遍歷整棵樹做預處理，記錄整棵樹最深的深度maxDepth，以及總共有多少個深度是maxDepth的節點deepCount

然二次遍歷整棵樹，從下往上查看每個節點包含多少個maxDepth的葉子節點。當找到第一個數目是deepCount的節點時，請說明它的子樹下麵包含了所有的deepest leaves，這就是答案


###### tags: `Leetcode` `Tree` `LCA`