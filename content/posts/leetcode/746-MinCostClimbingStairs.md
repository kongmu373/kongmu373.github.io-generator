---
title: "746 MinCostClimbingStairs"
date: 2020-07-20T11:09:30+08:00
draft: true
tags: ["DP"]
series: ["easy"]
categories: ["leetcode"]
---

On a staircase, the i-th step has some non-negative cost cost[i] assigned (0 i
ndexed).

Once you pay the cost, you can either climb one or two steps. You need to find
minimum cost to reach the top of the floor, and you can either start from the s
tep with index 0, or the step with index 1.


Example 1: 

Input: cost = [10, 15, 20]
Output: 15
Explanation: Cheapest is start on cost[1], pay that cost and go to the top.



Example 2: 

Input: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
Output: 6
Explanation: Cheapest is start on cost[0], and only step on 1s, skipping cost[
3].



Note: 

cost will have a length in the range [2, 1000]. 
Every cost[i] will be an integer in the range [0, 999]. 

Related Topics Array Dynamic Programming


```java

package com.kongmu373.leetcode.editor.en;

public class MinCostClimbingStairs {
    public static void main(String[] args) {
        Solution solution = new MinCostClimbingStairs().new Solution();
    }

    /**
     * Solution:DP:
     * Time complexity: O(n)
     * Space complexity: O(n) -> O(1)
     * <p>
     * dp(n) := min cost to climb to n-th step (n-th step no need to pay cost.)
     * dp(n) = min(dp(n-1) + cost[n-1],
     * dp(n-2） + cost[n-2])
     * ans = dp(n)
     */
    //leetcode submit region begin(Prohibit modification and deletion)
    class Solution {
        public int minCostClimbingStairs(int[] cost) {
            int dp1 = 0;
            int dp2 = 0;
            int dp =0;
            for (int i = 2; i <= cost.length; i++) {
                dp = Math.min(dp1 + cost[i - 1], dp2 + cost[i - 2]);
                dp2 = dp1;
                dp1 = dp;
            }
            return dp;
        }
    }
//leetcode submit region end(Prohibit modification and deletion)

}
```
