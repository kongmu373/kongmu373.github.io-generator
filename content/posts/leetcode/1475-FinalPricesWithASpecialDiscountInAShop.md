---
title: "1475 FinalPricesWithASpecialDiscountInAShop"
date: 2020-07-18T14:47:14+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---

Given the array prices where prices[i] is the price of the ith item in a shop.
There is a special discount for items in the shop, if you buy the ith item, the
n you will receive a discount equivalent to prices[j] where j is the minimum ind
ex such that j > i and prices[j] <= prices[i], otherwise, you will not receive a
ny discount at all. 

Return an array where the ith element is the final price you will pay for the
ith item of the shop considering the special discount. 


Example 1: 


Input: prices = [8,4,6,2,3]
Output: [4,2,4,2,3]
Explanation: 
For item 0 with price[0]=8 you will receive a discount equivalent to prices[1]
=4, therefore, the final price you will pay is 8 - 4 = 4. 
For item 1 with price[1]=4 you will receive a discount equivalent to prices[3]
=2, therefore, the final price you will pay is 4 - 2 = 2. 
For item 2 with price[2]=6 you will receive a discount equivalent to prices[3]
=2, therefore, the final price you will pay is 6 - 2 = 4. 
For items 3 and 4 you will not receive any discount at all.


Example 2: 


Input: prices = [1,2,3,4,5]
Output: [1,2,3,4,5]
Explanation: In this case, for all items, you will not receive any discount at
all.


Example 3: 


Input: prices = [10,1,1,6]
Output: [9,0,1,6]



Constraints: 


1 <= prices.length <= 500 
1 <= prices[i] <= 10^3 

Related Topics Array
```java
package com.kongmu373.leetcode.editor.en;

import java.util.LinkedList;

public class FinalPricesWithASpecialDiscountInAShop {
    public static void main(String[] args) {
        Solution solution = new FinalPricesWithASpecialDiscountInAShop().new Solution();
    }


    /**
     * Solution: Monotonic Stack
     * One cheaper item can be the discount of multiple more expensive items
     * if it index is greater.
     * [8, 4, 6, *2*, 3]
     * Use a monotonic stack to store the item in increasing order.
     * Whenever encounter a cheaper item than the top, pop the stack.
     * <p>
     * Time complexity: O(n) one push / one pop Space complexity: O(n)
     */
    //leetcode submit region begin(Prohibit modification and deletion)
    class Solution {
        public int[] finalPrices(int[] prices) {
            // 1. 创建一个栈并保持元素单调性
            LinkedList<Integer> stack = new LinkedList<>();
            for (int i = 0; i < prices.length; i++) {

                // 2.1 栈非空且元素小于等于栈顶元素
                while (!stack.isEmpty() && prices[stack.peek()] >= prices[i]) {
                    prices[stack.peek()] -= prices[i];
                    stack.pop();
                }
                // 2.2 否则,将它的编号i加到栈中
                stack.push(i);
            }
            return prices;
        }
    }
//leetcode submit region end(Prohibit modification and deletion)

}
```