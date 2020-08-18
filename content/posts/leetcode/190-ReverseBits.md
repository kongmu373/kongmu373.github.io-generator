---
title: "190 ReverseBits"
date: 2020-07-18T15:57:12+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---
Reverse bits of a given 32 bits unsigned integer. 



Example 1: 


Input: 00000010100101000001111010011100
Output: 00111001011110000010100101000000
Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.


Example 2: 


Input: 11111111111111111111111111111101
Output: 10111111111111111111111111111111
Explanation: The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is 10111111111111111111111111111111. 



Note: 


Note that in some languages such as Java, there is no unsigned integer type. 
In this case, both input and output will be given as signed integer type and should not affect your implementation, as the internal binary representation of the
integer is the same whether it is signed or unsigned. 
In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 2 above the input represents the signed integer -3 
and the output represents the signed integer -1073741825. 




Follow up: 

If this function is called many times, how would you optimize it? 
Related Topics Bit Manipulation

```java


package com.kongmu373.leetcode.editor.en;

public class ReverseBits {
    public static void main(String[] args) {
        Solution solution = new ReverseBits().new Solution();
    }

    //leetcode submit region begin(Prohibit modification and deletion)
    public class Solution {
        /**
         * Bit operation
         * Base-10 time case:
         *  ans = ans * 10 + n % 10
         *  n /= 10
         *
         * Base-2 is exact the same if n is positive
         * ans = ans * 2 + n % 2
         * n /= 2
         *
         * or use bit operators:
         *  ans = (ans << 1) | (n & 1)
         *  n >>= 1
         *
         * 但是要处理前置的零
         * 如: 00110 => 01100 而不是 011
         *
         * 当n时负数的时候，就不能用算术操作了，只能用位运算
         * Time complexity: O(logn)
         * Space complexity: O(1)
         * @param n
         * @return
         */
        // you need treat n as an unsigned value
        public int reverseBits(int n) {
            int res = 0;
            for (int i = 0; i < 32; i++) {
                res = (res << 1) | (n & 1);
                n >>= 1;
            }
            return res;
        }
    }
//leetcode submit region end(Prohibit modification and deletion)

}
```