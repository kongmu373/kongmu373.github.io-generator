---
title: "438 FindAllAngramsInAString"
date: 2020-07-19T23:07:30+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---
Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.

Strings consists of lowercase English letters only and the length of both str
ings s and p will not be larger than 20,100. 

The order of output does not matter. 

Example 1:

Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".



Example 2:

Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".

Related Topics Hash Table
```java



package com.kongmu373.leetcode.editor.en;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class FindAllAnagramsInAString {
    public static void main(String[] args) {
        Solution solution = new FindAllAnagramsInAString().new Solution();
        System.out.println(solution.findAnagrams("cbaebabacd", "abc"));
    }


    /**
     * Solution 1: Brute Force
     * Time complexity: O((|S|-|P|)*|P|) = O(n^2)
     * <p>
     * Solution 2: Sliding Window
     * Use a hashtable to represent p and the sliding window
     * Time complexity: O(|S| * 26/128) = O(n)
     */
    //leetcode submit region begin(Prohibit modification and deletion)
    class Solution {

        public List<Integer> findAnagrams(String s, String p) {
            int s_len = s.length();
            int p_len = p.length();
            Map<Character, Integer> p_map = new HashMap<>();
            Map<Character, Integer> s_map = new HashMap<>();
            ArrayList<Integer> ans = new ArrayList<>();
            for (char c : p.toCharArray()) {
                p_map.put(c, p_map.getOrDefault(c, 0) + 1);
            }
            for (int i = 0; i < s_len; i++) {
                if (i >= p_len) {
                    char firstChar = s.charAt(i - p_len);
                    Integer index = s_map.getOrDefault(firstChar, 0);
                    if (index > 1) {
                        s_map.put(firstChar, --index);
                    } else {
                        s_map.remove(firstChar);
                    }
                }
                char key = s.charAt(i);
                s_map.put(key, s_map.getOrDefault(key, 0) + 1);
                if (p_map.equals(s_map)) {
                    ans.add(i + 1 - p_len);
                }
            }
            return ans;
        }
    }
//leetcode submit region end(Prohibit modification and deletion)

}
```

