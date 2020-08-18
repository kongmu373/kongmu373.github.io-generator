---
title: "69 Sqrt"
date: 2020-07-20T08:29:35+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---
Implement int sqrt(int x).

Compute and return the square root of x, where x is guaranteed to be a non-ne
gative integer. 

Since the return type is an integer, the decimal digits are truncated and onl
y the integer part of the result is returned. 

Example 1: 


Input: 4
Output: 2


Example 2: 


Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since 
             the decimal part is truncated, 2 is returned.

Related Topics Math Binary Search
```java
package com.kongmu373.leetcode.editor.en;

public class Sqrtx {
    public static void main(String[] args) {
        Solution solution = new Sqrtx().new Solution();
    }

    /**
     * 1. 暴力破解
     * def mySqrt(x):
     * if x <= 1 : return x;
     * for(long i = 1; i <= x; i++)
     * if i * i > x: return i - 1
     * return -1
     * <p>
     * 2. Binary Search
     * def mySqrt(x):
     * int l = 1;
     * int r = x;
     * while(l<=r) {
     * int m = l + (r-l) /2;
     * if(m>x/m) {
     * r = m - 1;
     * } else {
     * l = m + 1;
     * }
     * }
     * return r;
     */
    //leetcode submit region begin(Prohibit modification and deletion)
    class Solution {
        public int mySqrt(int x) {
            int l = 1;
            int r = x;
            while (l <= r) {
                int m = l + (r - l) / 2;
                if (m > x / m) {
                    r = m - 1;
                } else {
                    l = m + 1;
                }
            }
            return r;
        }
    }
//leetcode submit region end(Prohibit modification and deletion)

}
```