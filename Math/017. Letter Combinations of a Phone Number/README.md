# Leetcode - 17. Letter Combinations of a Phone Number (M)

[Leetcode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png)

**Example 1:**
```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```
**Example 2:**
```
Input: digits = ""
Output: []
```
**Example 3:**
```
Input: digits = "2"
Output: ["a","b","c"]
```
**Constraints:**

-   `0 <= digits.length <= 4`
-   `digits[i]` is a digit in the range `['2', '9']`.

---
```java
// Java
class Solution {
    List<String> ret = new ArrayList<>();
    public List<String> letterCombinations(String digits) {
        String[] numArr = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        if (digits.length() == 0) return ret;
        backtracking(digits.toCharArray(), 0, numArr, new StringBuilder());
        return ret;
    }
    public void backtracking(char[] digitsArr, int index, String[] numArr, StringBuilder sb) {
        if (sb.length() == digitsArr.length) {
            ret.add(sb.toString());
            return;
        }
        String letter = numArr[digitsArr[index] - '0'];
        for (char str : letter.toCharArray()) {
            sb.append(str);
            backtracking(digitsArr, index+1, numArr, sb);
            sb.delete(sb.length() - 1, sb.length());
        }
    }
}
```
---

###### tags: `Leetcode`
