# Leetcode - 2096. Step-By-Step Directions From a Binary Tree Node to Another (M+)

[Leetcode](https://leetcode.com/problems/step-by-step-directions-from-a-binary-tree-node-to-another/)

You are given the `root` of a **binary tree** with `n` nodes. Each node is uniquely assigned a value from `1` to `n`. You are also given an integer `startValue` representing the value of the start node `s`, and a different integer `destValue` representing the value of the destination node `t`.

Find the **shortest path** starting from node `s` and ending at node `t`. Generate step-by-step directions of such path as a string consisting of only the **uppercase** letters `'L'`, `'R'`, and `'U'`. Each letter indicates a specific direction:

-   `'L'` means to go from a node to its **left child** node.
-   `'R'` means to go from a node to its **right child** node.
-   `'U'` means to go from a node to its **parent** node.

Return _the step-by-step directions of the **shortest path** from node _`s`_ to node_ `t`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/15/eg1.png)
```
Input: root = [5,1,2,3,null,6,4], startValue = 3, destValue = 6
Output: "UURL"
Explanation: The shortest path is: 3 → 1 → 5 → 2 → 6.
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/15/eg2.png)
```
Input: root = [2,1], startValue = 2, destValue = 1
Output: "L"
Explanation: The shortest path is: 2 → 1.
```
**Constraints:**

-   The number of nodes in the tree is `n`.
-   `2 <= n <= 105`
-   `1 <= Node.val <= n`
-   All the values in the tree are **unique**.
-   `1 <= startValue, destValue <= n`
-   `startValue != destValue`

---
```java
// Java [3 Steps] 19ms (Beats 86.64%), Time O(N), Space O(N)

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
class Solution {
    public String getDirections(TreeNode root, int startValue, int destValue) {
        StringBuilder s = new StringBuilder(), d = new StringBuilder();
        find(s, root, startValue);
        find(d, root, destValue);
        int minLen = Math.min(s.length(), d.length());
        int i = 0;
        while (i < minLen && s.charAt(s.length() - 1 - i) == d.charAt(d.length() - 1 - i))
            i++;
        
        return "U".repeat(s.length() - i) + d.reverse().toString().substring(i);
    }

    boolean find(StringBuilder sb, TreeNode root, int val)
    {
        if (root == null)
            return false;
        if (root.val == val)
            return true;
        if (root.left != null && find(sb, root.left, val))
            sb.append('L');
        else if (root.right != null && find(sb, root.right, val))
            sb.append('R');

        return sb.length() > 0;
    }
}
```

```java
// Java [wisdompeak] 26ms (Beats 59.76%), Time O(N), Space O(N)
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
class Solution {
    public String getDirections(TreeNode root, int startValue, int destValue) {
        StringBuilder s = new StringBuilder(), d = new StringBuilder();
        find(s, root, startValue);
        find(d, root, destValue);
        int i = 0;
        while (i < s.length() && i < d.length() && s.charAt(i) == d.charAt(i))
            i++;
        
        return "U".repeat(s.length() - i) + d.toString().substring(i);
    }

	// 正向紀錄走過的點
    boolean find(StringBuilder sb, TreeNode root, int val)
    {
        if (root == null)
            return false;
        if (root.val == val)
            return true;

        if (root.left != null)
        {
            sb.append('L');
            if (find(sb, root.left, val))
                return true;
            sb.delete(sb.length()-1, sb.length());
        }
            
        if (root.right != null)
        {
            sb.append('R');
            if (find(sb, root.right, val))
                return true;
            sb.delete(sb.length()-1, sb.length());
        }

        return false;
    }
}
```
---
refer to: [3 Steps](https://leetcode.com/problems/step-by-step-directions-from-a-binary-tree-node-to-another/solutions/1612105/3-steps)
refer to: [wisdompeak/YouTube](https://youtu.be/VvdlzPAQE0s)


###### tags: `Leetcode` `Tree` `LCA`