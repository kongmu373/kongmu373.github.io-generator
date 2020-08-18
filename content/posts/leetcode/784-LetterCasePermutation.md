---
title: "784 LetterCasePermutation"
date: 2020-07-19T23:37:15+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---
Given a string S, we can transform every letter individually to be lowercase o
r uppercase to create another string. Return a list of all possible strings we c
ould create. 


Examples:
Input: S = "a1b2"
Output: ["a1b2", "a1B2", "A1b2", "A1B2"]

Input: S = "3z4"
Output: ["3z4", "3Z4"]

Input: S = "12345"
Output: ["12345"]


Note: 


S will be a string with length between 1 and 12. 
S will consist only of letters or digits. 

Related Topics Backtracking Bit Manipulation
```java



package com.kongmu373.leetcode.editor.en;

import java.util.ArrayList;
import java.util.List;

public class LetterCasePermutation {
    public static void main(String[] args) {
        Solution solution = new LetterCasePermutation().new Solution();
    }

    /**
     * Solution 1: DFS
     * Time complexity: O(n^2^l)l: # of letters
     * Space complexity: O(n) + O(n^2^l) stack + sols
     * def dfs(S, i):
     * if i == len(S): ans.append(S)
     * dfs(S, i+1)
     * if S[i] is letter:
     * toggle(S[i]) # S[i] ^= (1 << 5)
     * dfs(S, i + 1)
     * <p>
     * ASCII:
     * Upper case A-Z: 65-90
     * Lower case a-z: 97-122
     * 'a' - 'A' = 32
     * 大小写转换:
     * S[i] ^= (1 << 5) 把S[i]中二进制的第5位从0变1,从1变0
     */
    //leetcode submit region begin(Prohibit modification and deletion)
    class Solution {
        public List<String> letterCasePermutation(String S) {
            List<String> ans = new ArrayList<String>();
            dfs(S.toCharArray(), 0, ans);
            return ans;
        }

        private void dfs(char[] S, int i, List<String> ans) {
            if (i == S.length) {
                ans.add(new String(S));
                return;
            }
            dfs(S, i + 1, ans);
            if (!Character.isLetter(S[i])) {
                return;
            }
            S[i] ^= 1 << 5;
            dfs(S, i + 1, ans);
        }
    }
//leetcode submit region end(Prohibit modification and deletion)

}
```
