# Leetcode - 981. Time Based Key-Value Store (M)

[Leetcode](https://leetcode.com/problems/time-based-key-value-store/)

Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key’s value at a certain timestamp.

Implement the `TimeMap` class:

-   `TimeMap()` Initializes the object of the data structure.
-   `void set(String key, String value, int timestamp)` Stores the key `key` with the value `value `at the given time `timestamp`.
-   `String get(String key, int timestamp)` Returns a value such that `set` was called previously, with `timestamp_prev <= timestamp`. If there are multiple such values, it returns the value associated with the largest `timestamp_prev`. If there are no values, it returns `""`.

**Example 1:**
```
Input  
["TimeMap", "set", "get", "get", "set", "get", "get"]  
[[], ["foo", "bar", 1], ["foo", 1], ["foo", 3], ["foo", "bar2", 4], ["foo", 4], ["foo", 5]]  
Output  
[null, null, "bar", "bar", null, "bar2", "bar2"]
```
```
Explanation  
TimeMap timeMap = new TimeMap();  
timeMap.set("foo", "bar", 1);  // store the key "foo" and value "bar" along with timestamp = 1.  
timeMap.get("foo", 1);         // return "bar"  
timeMap.get("foo", 3);         // return "bar", since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is "bar".  
timeMap.set("foo", "bar2", 4); // store the key "foo" and value "bar2" along with timestamp = 4.  
timeMap.get("foo", 4);         // return "bar2"  
timeMap.get("foo", 5);         // return "bar2"
```
**Constraints:**

-   `1 <= key.length, value.length <= 100`
-   `key` and `value` consist of lowercase English letters and digits.
-   `1 <= timestamp <= 107`
-   All the timestamps `timestamp` of `set` are strictly increasing.
-   At most `2 * 105` calls will be made to `set` and `get`.

---

```java
// Java (HashMap + Binary Search)  
class TimeMap {  
    private HashMap<String, ArrayList<Pair<Integer, String>>> map;  
  
    public TimeMap() {  
        map = new HashMap();  
    }  
      
    public void set(String key, String value, int timestamp) {  
        if (!map.containsKey(key)) {  
            map.put(key, new ArrayList());  
        }  
        map.get(key).add(new Pair(timestamp, value));  
    }  
      
    public String get(String key, int timestamp) {  
        if (!map.containsKey(key))  
            return "";  
        if (timestamp < map.get(key).get(0).getKey()) {  
            return "";  
        }  
          
        int left = 0;  
        int right = map.get(key).size();  
        while (left < right) {  
            int mid = left + (right - left) / 2;  
            // 10 20  
            if (map.get(key).get(mid).getKey() <= timestamp) {  
                left = mid + 1;  
            } else {  
                right = mid;  
            }  
        }  
  
        if (left == 0) {  
            return "";  
        }  
        return map.get(key).get(left - 1).getValue();  
    }  
}  
  
/**  
 * Your TimeMap object will be instantiated and called as such:  
 * TimeMap obj = new TimeMap();  
 * obj.set(key,value,timestamp);  
 * String param_2 = obj.get(key,timestamp);  
 */
```
 
 
```java
// Java (HashMap + TreeMap)  
class TimeMap {  
    HashMap<String, TreeMap<Integer, String>> map;  
  
    public TimeMap() {  
        map = new HashMap<String, TreeMap<Integer, String>>();  
    }  
      
    public void set(String key, String value, int timestamp) {  
        if (!map.containsKey(key)) {  
             map.put(key, new TreeMap<Integer, String>());  
        }  
        map.get(key).put(timestamp, value);  
    }  
      
    public String get(String key, int timestamp) {  
        TreeMap<Integer, String> treeMap = map.get(key);  
        if (treeMap == null) {  
            return "";  
        }  
        //Integer的初始值是null, int的初始值是0。  
        Integer floor = map.get(key).floorKey(timestamp);  
        if (floor == null) {  
            return "";  
        }  
        return treeMap.get(floor);  
    }  
}  
  
/**  
 * Your TimeMap object will be instantiated and called as such:  
 * TimeMap obj = new TimeMap();  
 * obj.set(key,value,timestamp);  
 * String param_2 = obj.get(key,timestamp);  
 */
```

---

1.  使用Binary Search尋找最左/最右的True false,  
    使用的right必須為`length()`, 不可為`length() - 1`  
    **模板要背好 **[278. First Bad Version (E)](https://hackmd.io/UPKecvtYRJm4gbkn7al5IQ?view)
2.  TreeMap / TreeSet 為有排序(小 →大)的Map / Set  
    `TreeMap.floorKey(k) = 小於等於k中,最大的Key值`  
    `TreeMap.ceilingKey(k) = 大於等於k中,最小的Key值`
3.  int的初始值為0, Integer的初始值為null```

---

```csharp
// C#  
public class TimeMap {  
    Dictionary<string, List<KeyValuePair<int, string>>> dict;  
    public TimeMap() {  
        dict = new Dictionary<string, List<KeyValuePair<int, string>>>();  
    }  
      
    public void Set(string key, string value, int timestamp) {  
        if (!dict.ContainsKey(key)) {  
            dict.Add(key, new List<KeyValuePair<int, string>>());  
        }  
        dict[key].Add(new KeyValuePair<int, string>(timestamp, value));  
    }  
      
    public string Get(string key, int timestamp) {  
        if (!dict.ContainsKey(key)) {  
            return "";   
        }  
        List<KeyValuePair<int, string>> list = dict[key];  
        if (timestamp < list[0].Key) {  
            return "";  
        }  
        int left = 0, right = list.Count();  
        while (left < right) {  
            int mid = left + (right - left) / 2;  
            if (list[mid].Key <= timestamp) {  
                left = mid + 1;  
            } else {  
                right = mid;  
            }  
        }  
        if (right == 0) {  
            return "";  
        }  
        return list[right - 1].Value;  
    }  
}  
  
/**  
 * Your TimeMap object will be instantiated and called as such:  
 * TimeMap obj = new TimeMap();  
 * obj.Set(key,value,timestamp);  
 * string param_2 = obj.Get(key,timestamp);  
 */
```

---

如果題目沒有限制  
All the timestamps `timestamp` of `set` are strictly increasing.  
則  
需使用SortedList  


###### tags: `Leetcode` `Binary Search`
