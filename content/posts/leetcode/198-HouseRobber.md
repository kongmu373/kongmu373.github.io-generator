---
title: "198 HouseRobber"
date: 2020-07-20T13:38:28+08:00
draft: true
tags: ["DP"]
series: ["easy"]
categories: ["leetcode"]
---

对应 leetCode 198题.
```java

package com.kongmu373.leetcode.editor.en;

public class HouseRobber {
    public static void main(String[] args) {
        Solution solution = new HouseRobber().new Solution();
    }

    /**
     * Recursion + Memorization
     * For a given house i, we have two options:
     * 1. Take the moeny if we didn't robber house i - 1
     * 2. Skip it
     * rob(n) = max(rob(n-2) + money[i], rob(n-1))
     * Time complexity:O(n), Space complexity:O(n)
     * <p>
     * DP
     * <p>
     * dp[i]: Max money after
     * "visiting" house[i]
     * <p>
     * dp[i]: max(dp[i-2] + money[i], dp[i-1])
     * <p>
     * Time complexity: O(n)
     * Space complexity: O(n) -> O(1)
     */
    //leetcode submit region begin(Prohibit modification and deletion)
    class Solution {
        public int rob(int[] nums) {
                return dp(nums);
        }

        private int dp(int[] nums) {
            int length = nums.length;
            int dp1 = 0;
            int dp2 = 0;
            int dp = 0;
            for (int num : nums) {
                dp = Math.max(dp2 + num, dp1);
                dp2 = dp1;
                dp1 = dp;
            }
            return dp;
        }
    }
//leetcode submit region end(Prohibit modification and deletion)

}
```