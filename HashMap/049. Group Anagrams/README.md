# Leetcode - 49. Group Anagrams (M+)

[Leetcode](https://leetcode.com/problems/group-anagrams/)

Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**
```
Input: strs = ["eat","tea","tan","ate","nat","bat"]  
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```
**Example 2:**
```
Input: strs = [""]  
Output: [[""]]
```
**Example 3:**
```
Input: strs = ["a"]  
Output: [["a"]]
```
**Constraints:**

-   `1 <= strs.length <= 104`
-   `0 <= strs[i].length <= 100`
-   `strs[i]` consists of lowercase English letters.

---

```java
// Java  
class Solution {  
    public List<List<String>> groupAnagrams(String[] strs) {  
        HashMap<String, List<String>> map = new HashMap<>();  
        for (String str : strs) {  
            char[] freq = new char[26];  
            for (char c : str.toCharArray()) {  
                freq[c - 'a']++;  
            }  
            String key = String.valueOf(freq);  
            if (!map.containsKey(key)) {  
                map.put(key, new LinkedList<String>());     
            }  
            map.get(key).add(str);  
        }  
  
        return new ArrayList<>(map.values());  
    }  
}
```
	
將String轉成char[]並重新排序做為key  
檢核哪些String為一組

> 技巧

1.  String轉char[]  
    `string.toCharArray()`
2.  Array排序  
    `Arrays.sort( array[] )`  
    big(O) = nlog(n)
3.  char[]轉成String  
    `String.valueOf(char[])`
4.  HashMap檢查是否存在Key  
    `HashMap.containsKey()`  
    檢查是否存在Value  
    `HashMap.containsValue()`
5.  不使用Arrays.sort(), 更快的解法

```java
char[] frequencyArr = new char[26];  
for(int i = 0;i<s.length();i++){  
     frequencyArr[s.charAt(i) - 'a']++;  
}
```

_(1) 6 ms use char(0~127) array and new String(frequencyArr) method.  
(2) 17ms when use byte (-128 to 127) array and Arrays.toString(frequencyArr) method  
(3) 29ms when use int(-2,147,483,648 to 2,147,483,647) and Arrays.toString(frequencyArr) method_

###### tags: `Leetcode` `HashMap`0
