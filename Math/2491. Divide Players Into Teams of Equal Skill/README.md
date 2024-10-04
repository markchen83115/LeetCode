# Leetcode - 2491. Divide Players Into Teams of Equal Skill (E+)

[Leetcode](https://leetcode.com/problems/divide-players-into-teams-of-equal-skill/)

You are given a positive integer array `skill` of **even** length `n` where `skill[i]` denotes the skill of the `ith` player. Divide the players into `n / 2` teams of size `2` such that the total skill of each team is **equal**.

The **chemistry** of a team is equal to the **product** of the skills of the players on that team.

Return _the sum of the **chemistry** of all the teams, or return _`-1`_ if there is no way to divide the players into teams such that the total skill of each team is equal._

**Example 1:**
```
Input: skill = [3,2,5,1,3,4]
Output: 22
Explanation: 
Divide the players into the following teams: (1, 5), (2, 4), (3, 3), where each team has a total skill of 6.
The sum of the chemistry of all the teams is: 1 * 5 + 2 * 4 + 3 * 3 = 5 + 8 + 9 = 22.
```
**Example 2:**
```
Input: skill = [3,4]
Output: 12
Explanation: 
The two players form a team with a total skill of 7.
The chemistry of the team is 3 * 4 = 12.
```
**Example 3:**
```
Input: skill = [1,1,2,3]
Output: -1
Explanation: 
There is no way to divide the players into teams such that the total skill of each team is equal.
```
**Constraints:**

-   `2 <= skill.length <= 105`
-   `skill.length` is even.
-   `1 <= skill[i] <= 1000`

---
```java
// Java HashMap 25ms(Beats 8.31%), Time O(N), Space O(N)
class Solution {
    public long dividePlayers(int[] skill) {
        long ret = 0;
        int avg = 0;
        int n = skill.length;
        HashMap<Integer, Integer> countMap = new HashMap<>(); // skill, count
        for (int sk : skill)
        {
            avg += sk;
            countMap.merge(sk, 1, Integer::sum);
        }
        avg /= n / 2;

        for (int sk : skill)
        {
            int need = avg - sk;
            countMap.merge(need, -1, Integer::sum);
            if (countMap.get(need) < 0)
                return -1L;
            ret += need * sk;
        }

        return ret / 2;
    }
}
```
```java
// Java Array 3ms(Beats 96.47%), Time O(N), Space O(N)
class Solution {
    public long dividePlayers(int[] skill) {
        long ret = 0;
        int avg = 0;
        int n = skill.length;
        int[] count = new int[1001];
        for (int sk : skill)
        {
            avg += sk;
            count[sk]++;
        }
        if (avg % (n / 2) != 0)
            return -1;
        avg /= n / 2;

        for (int sk : skill)
        {
            int need = avg - sk;
            count[need]--;
            if (count[need] < 0)
                return -1L;
            ret += need * sk;
        }

        return ret / 2;
    }
}
```
---

將所有數字加總並檢查是否可被`n/2`整除, 若無法整除則回傳`-1`
將所有數字依序配對, 若有無法配對的數字則回傳`-1`


###### tags: `Leetcode` `Math`