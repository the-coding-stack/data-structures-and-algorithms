# Group Anagrams

[Leetcode problem](https://leetcode.com/problems/group-anagrams/description/)

## Problem

Given an array of strings strs, group the anagrams together. You can return the answer in any order.

### Examples

**Example 1**

```text
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

Explanation:

There is no string in strs that can be rearranged to form "bat".
The strings "nat" and "tan" are anagrams as they can be rearranged to form each other.
The strings "ate", "eat", and "tea" are anagrams as they can be rearranged to form each other.

**Example 2**

```text
Input: strs = [""]
Output: [[""]]
```

**Example 3**

```text
Input: strs = ["a"]
Output: [["a"]]
```

### Constraints

- `1 <= strs.length <= 10^4`
- `0 <= strs[i].length <= 100`
- `strs[i] consists of lowercase English letters.`

## Approach 1: Sort Each String

Anagrams contain the same letters, so sorting the characters of each string produces the same result for every anagram. Use the sorted string as a key in a hash map and add each original string to the group for that key. For example, both `eat` and `tea` sort to `aet`.

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> groupsBySortedString = new HashMap<>();

        for (String str : strs) {
            char[] characters = str.toCharArray();
            Arrays.sort(characters);
            String key = new String(characters);

            groupsBySortedString
                    .computeIfAbsent(key, ignored -> new ArrayList<>())
                    .add(str);
        }

        return new ArrayList<>(groupsBySortedString.values());
    }
}
```

**Complexity**

- **Time:** `O(n * k log k)`, where `n` is the number of strings and `k` is the maximum string length. Each string is sorted to build its key.
- **Auxiliary space:** `O(n * k)` for the groups and their keys, excluding the returned groups' storage.

## Approach 2: Character Frequency (Optimized)

Anagrams contain the same number of each letter, even when their letters appear in a different order. For every string, count its 26 lowercase letters. Use that frequency pattern as a key in a hash map and add the string to the matching group.

For example, `eat` and `tea` both have one `a`, one `e`, and one `t`, so they receive the same key. The empty string has 26 zero counts and is grouped correctly as well. Once every string is processed, return the map's groups.

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> groupsByFrequency = new HashMap<>();

        for (String str : strs) {
            int[] frequency = new int[26];
            for (int i = 0; i < str.length(); i++) {
                frequency[str.charAt(i) - 'a']++;
            }

            // Option 1: encode all 26 counts. The '#' separator keeps counts unambiguous.
            StringBuilder key = new StringBuilder();
            for (int count : frequency) {
                key.append('#').append(count);
            }

            // Option 2: build the equivalent sorted-letter key by repeating each letter.
            // This alternative can replace the count-encoding loop above:
            //
            // StringBuilder key = new StringBuilder();
            // for (int i = 0; i < 26; i++) {
            //     if (frequency[i] > 0) {
            //         key.append(String.valueOf((char) ('a' + i))
            //                 .repeat(frequency[i]));
            //     }
            // }

            groupsByFrequency
                    .computeIfAbsent(key.toString(), ignored -> new ArrayList<>())
                    .add(str);
        }

        return new ArrayList<>(groupsByFrequency.values());
    }
}
```

**Complexity**

- **Time:** `O(n * k)`, where `n` is the number of strings and `k` is the maximum string length. Each character is counted once, and the fixed 26-count key takes constant time to build.
- **Auxiliary space:** `O(n * k)` for the groups and their keys, excluding the returned groups' storage.
