---
title: "13 RomantoInteger"
date: 2020-07-18T15:18:38+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---
  Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M. 


Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000 

For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven
is written as XXVII, which is XX + V + II. 

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as 
IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used: 


I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900. 


Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999. 

Example 1: 


Input: "III"
Output: 3 

Example 2: 


Input: "IV"
Output: 4 

Example 3: 


Input: "IX"
Output: 9 

Example 4: 


Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.


Example 5: 


Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4. 
Related Topics Math String
```java
package com.kongmu373.leetcode.editor.en;

import java.util.HashMap;
import java.util.Map;

public class RomanToInteger {
    public static void main(String[] args) {
        Solution solution = new RomanToInteger().new Solution();
    }

    //leetcode submit region begin(Prohibit modification and deletion)
    class Solution {
        /**
         * "ab"模式: 如果b是大于a的,那么该值由 a+b, 转变成 a-b;
         * Solution:
         * 将所有符号加起来, 当遇到"ab"模式时，还是相加，然后减去 2 * a.
         * sample1:
         * "IV"
         * I: 1
         * V: 1+5-2*a=6-2=4
         * sample2:
         * "VX" = 5+10-2*5=15-10=5
         * <p>
         * Time complexity: O(n)
         * Space complexity: O(1)
         *
         * @param s 输入的罗马字符串
         * @return 返回int值
         */
        public int romanToInt(String s) {
            Map<Character, Integer> map = new HashMap<>();
            map.put('I', 1);
            map.put('V', 5);
            map.put('X', 10);
            map.put('L', 50);
            map.put('C', 100);
            map.put('D', 500);
            map.put('M', 1000);
            Character prev = null;
            int ans = 0;
            for (char c : s.toCharArray()) {
                ans += map.get(c);
                if(prev != null && map.get(prev) < map.get(c)) {
                    ans -= 2 * map.get(prev);
                }
                prev = c;
            }
            return ans;
        }
    }
//leetcode submit region end(Prohibit modification and deletion)

}
```