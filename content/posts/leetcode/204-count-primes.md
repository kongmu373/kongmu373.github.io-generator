---
title: "204 Count Primes"
date: 2020-07-23T10:51:34+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---

## leecode - 204 -  count primes
```java
//Count the number of prime numbers less than a non-negative number, n.
//
// Example: 
//
// 
//Input: 10
//Output: 4
//Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
// 
// Related Topics Hash Table Math 
// 👍 2023 👎 614


package com.kongmu373.leetcode.editor.en;

import java.util.Arrays;

public class CountPrimes {
    public static void main(String[] args) {
        Solution solution = new CountPrimes().new Solution();
    }

    //leetcode submit region begin(Prohibit modification and deletion)
    class Solution {
        public int countPrimes(int n) {
            // 原理: 2是质数， 其他能不被2整除的也是质数
            if (n < 3) {
                return 0;
            }
            int[] f = new int[n];
            Arrays.fill(f, 1);
            f[0] = 0;
            f[1] = 0;
            for (int i = 2; i <= Math.sqrt(n); i++) {
                if (f[i] == 0) {
                    continue;
                }
                for (int j = i * i; j < n; j += i) {
                    f[j] = 0;
                }
            }
            return Arrays.stream(f).sum();
        }
    }
//leetcode submit region end(Prohibit modification and deletion)

}
```