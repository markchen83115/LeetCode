# Leetcode - 572. Subtree of Another Tree (M/H)

[Leetcode](https://leetcode.com/problems/subtree-of-another-tree/description/)

Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/28/subtree1-tree.jpg)
```
Input: root = [3,4,5,1,2], subRoot = [4,1,2]
Output: true
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/28/subtree2-tree.jpg)
```
Input: root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
Output: false
```
**Constraints:**

-   The number of nodes in the `root` tree is in the range `[1, 2000]`.
-   The number of nodes in the `subRoot` tree is in the range `[1, 1000]`.
-   `-104 <= root.val <= 104`
-   `-104 <= subRoot.val <= 104`

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
// Java DFS 3ms(Beat 91.13%), Time O(N^2)
class Solution {
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        if (root == null || subRoot == null)
            return root == subRoot;
        if (isSame(root, subRoot))
            return true;
        return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
    }

    public boolean isSame(TreeNode root, TreeNode subRoot) {
        if (root == null || subRoot == null)
            return root == subRoot;
        if (root.val != subRoot.val)
            return false;
        return isSame(root.left, subRoot.left) && isSame(root.right, subRoot.right);
    }
}
```

```java
// Java KMP, Time O(M+N)
class Solution {
    public boolean isSubtree(TreeNode root, TreeNode subRoot) {
        StringBuilder rootSB = new StringBuilder();
        StringBuilder subRootSB = new StringBuilder();
        treeToString(root, rootSB);
        treeToString(subRoot, subRootSB);
        String rootStr = rootSB.toString();
        String subRootStr = subRootSB.toString();
		// Use KMP
        if (rootStr.indexOf(subRootStr) >= 0)
            return true;
        return false;
    }

    public void treeToString(TreeNode root, StringBuilder sb) {
        if (root == null) {
            sb.append(",").append("#");
        } else {
            sb.append(",").append(root.val);
            treeToString(root.left, sb);
            treeToString(root.right, sb);
        }
    }
}
```
---
[此題講解wisdompeak](https://www.youtube.com/watch?v=BHzTSN6gAaM)
Tree有個性質, 可用preorder(LVR)代表一棵Unique Tree
因此用LVR搭配KMP可將Time降到O(M+N)

>KMP寫法

[參考wisdompeak/YouTube - 1392. Longest Happy Prefix
](https://www.youtube.com/watch?v=n4an59As73Y)
[wisdompeak/Github圖例](https://github.com/wisdompeak/LeetCode/tree/master/String/1392.Longest-Happy-Prefix)


###### tags: `Leetcode` `Tree` `KMP`
