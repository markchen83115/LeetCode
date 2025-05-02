# Leetcode - 838. Push Dominoes (M+)

[Leetcode](https://leetcode.com/problems/push-dominoes/)

There are `n` dominoes in a line, and we place each domino vertically upright. In the beginning, we simultaneously push some of the dominoes either to the left or to the right.

After each second, each domino that is falling to the left pushes the adjacent domino on the left. Similarly, the dominoes falling to the right push their adjacent dominoes standing on the right.

When a vertical domino has dominoes falling on it from both sides, it stays still due to the balance of the forces.

For the purposes of this question, we will consider that a falling domino expends no additional force to a falling or already fallen domino.

You are given a string `dominoes` representing the initial state where:

-   `dominoes[i] = 'L'`, if the `ith` domino has been pushed to the left,
-   `dominoes[i] = 'R'`, if the `ith` domino has been pushed to the right, and
-   `dominoes[i] = '.'`, if the `ith` domino has not been pushed.

Return _a string representing the final state_.

**Example 1:**

> **Input:** dominoes = "RR.L"
> **Output:** "RR.L"
> **Explanation:** The first domino expends no additional force on the second domino.

**Example 2:**

> ![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/05/18/domino.png)
> 
> **Input:** dominoes = ".L.R...LR..L.."
> **Output:** "LL.RR.LLRRLL.."

**Constraints:**

-   `n == dominoes.length`
-   `1 <= n <= 105`
-   `dominoes[i]` is either `'L'`, `'R'`, or `'.'`.

---
```java
// Java Greedy 27ms(Beats 36.60%), Time O(N), Space O(N)
class Solution {
    public String pushDominoes(String dominoes) {
        int n = dominoes.length();
        int[] force = new int[n];
        // R
        int f = 0;
        for (int i = 0; i < n; i++)
        {
            if (dominoes.charAt(i) == 'R')
                f = n;
            else if (dominoes.charAt(i) == 'L')
                f = 0;
            else
                f = Math.max(f - 1, 0);
            force[i] += f;
        }

        // L
        f = 0;
        for (int i = n - 1; i >= 0; i--)
        {
            if (dominoes.charAt(i) == 'L')
                f = n;
            else if (dominoes.charAt(i) == 'R')
                f = 0;
            else
                f = Math.max(f - 1, 0);
            force[i] -= f;
        }

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++)
        {
            if (force[i] > 0)
                sb.append('R');
            else if (force[i] < 0)
                sb.append('L');
            else
                sb.append('.');
        }

        return sb.toString();
    }
}
```
```java
// Java BFS 57ms(Beats 11.32%), Time O(N), Space O(N)
class Solution {
    public String pushDominoes(String dominoes) {
        Queue<int[]> queue = new LinkedList<>();
        int n = dominoes.length();
        int[] ret = new int[n];
        Arrays.fill(ret, -2);

        for (int i = 0; i < n; i++)
        {
            if (dominoes.charAt(i) == 'L')
            {
                queue.offer(new int[] {i, -1});
                ret[i] = -1;
            }
            else if (dominoes.charAt(i) == 'R')
            {
                queue.offer(new int[] {i, 1});
                ret[i] = 1;
            }
        }

        while (!queue.isEmpty())
        {
            HashMap<Integer, Integer> map = new HashMap<>();
            int size = queue.size();
            for (int k = 0; k < size; k++)
            {
                int[] cur = queue.poll();
                int i = cur[0], dir = cur[1];

                if (dir == -1 && i - 1 >= 0 && ret[i - 1] == -2)
                    map.merge(i - 1, -1, Integer::sum);
                else if (dir == 1 && i + 1 < n && ret[i + 1] == -2)
                    map.merge(i + 1, 1, Integer::sum);
            }

            for (int key : map.keySet())
            {
                ret[key] = map.get(key);
                queue.offer(new int[] {key, ret[key]});
            }
        }

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++)
        {
            if (ret[i] == 1)
                sb.append('R');
            else if (ret[i] == -1)
                sb.append('L');
            else
                sb.append('.');
        }
        return sb.toString();
    }
}
```
---

### 解法1

Here is a worked example on the string `S = 'R.R...L'`: 
We find the force going from left to right is `[7, 6, 7, 6, 5, 4, 0]`. 
The force going from right to left is `[0, 0, 0, -4, -5, -6, -7]`. 

Combining them (taking their vector addition), the combined force is `[7, 6, 7, 2, 0, -2, -7]`, for a final answer of `RRRR.LL`.


### 解法2

根據題意，我們依照回合的順序進行BFS模擬。隊列的初始，放入所有被推導的骨牌。

在一個回合裡，我們彈出所有在上一回合被推倒的骨牌。如果是個剛被往左推倒的骨牌，會在此回合影響它左邊的骨牌（如果未被推倒的話），使其有左傾的趨勢；同理，對於剛被往右推倒的骨牌，會在此回合影響它右邊的多米諾骨牌（如果未被推倒的話），使其有右傾的趨勢。注意，有些多米諾骨牌可能會在此回合既被左傾也被右傾，那麼它就最終的狀態是站立。這回合結束時，我們要將所有在本回合被推倒的骨牌加入隊列。

最後所有多米諾骨牌會在BFS的過程中被加入隊列確認狀態。

###### tags: `Leetcode` `Greedy` `BFS`