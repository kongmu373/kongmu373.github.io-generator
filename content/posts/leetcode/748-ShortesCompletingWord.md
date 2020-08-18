---
title: "748 ShortesCompletingWord"
date: 2020-07-20T10:40:42+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---


Find the minimum length word from a given dictionary words, which has all the 
letters from the string licensePlate. Such a word is said to complete the given 
string licensePlate

Here, for letters we ignore case. For example, "P" on the licensePlate still m
atches "p" on the word.

It is guaranteed an answer exists. If there are multiple answers, return the o
ne that occurs first in the array.

The license plate might have the same letter occurring multiple times. For exa
mple, given a licensePlate of "PP", the word "pair" does not complete the licens
ePlate, but the word "supper" does.


Example 1: 

Input: licensePlate = "1s3 PSt", words = ["step", "steps", "stripe", "stepple"
]
Output: "steps"
Explanation: The smallest length word that contains the letters "S", "P", "S",
and "T".
Note that the answer is not "step", because the letter "s" must occur in the w
ord twice.
Also note that we ignored case for the purposes of comparing whether a letter 
exists in the word.



Example 2: 

Input: licensePlate = "1s3 456", words = ["looks", "pest", "stew", "show"]
Output: "pest"
Explanation: There are 3 smallest length words that contains the letters "s".
We return the one that occurred first.



Note: 

licensePlate will be a string with length in range [1, 7]. 
licensePlate will contain digits, spaces, or letters (uppercase or lowercase)
. 
words will have a length in the range [10, 1000]. 
Every words[i] will consist of lowercase letters, and have length in range [1
, 15]. 

Related Topics Hash Table
```java



package com.kongmu373.leetcode.editor.en;

public class ShortestCompletingWord {
    public static void main(String[] args) {
        Solution solution = new ShortestCompletingWord().new Solution();
        System.out.println(solution.shortestCompletingWord("1s3 PSt", new String[] {"step", "steps", "stripe", "stepple"}));
    }

    //leetcode submit region begin(Prohibit modification and deletion)
    class Solution {
        public String shortestCompletingWord(String licensePlate, String[] words) {
            int[] l = new int[26];
            for (char c : licensePlate.toCharArray()) {
                if (Character.isLetter(c)) {
                    ++l[Character.toLowerCase(c) - 'a'];
                }
            }
            String ans = null;
            int min_l = Integer.MAX_VALUE;
            for (String word : words) {
                if (word.length() >= min_l) {
                    continue;
                }
                if (!matchers(word, l)) {
                    continue;
                }
                min_l = word.length();
                ans = word;
            }
            return ans;
        }

        private boolean matchers(String word, int[] l) {
            int[] c = new int[26];
            for (char c1 : word.toCharArray()) {
                if (Character.isLetter(c1)) {
                    ++c[Character.toLowerCase(c1) - 'a'];
                }
            }
            for (int i = 0; i < 26; i++) {
                if (c[i] < l[i]) {
                    return false;
                }
            }
            return true;
        }
    }
//leetcode submit region end(Prohibit modification and deletion)

}
```