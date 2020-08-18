---
title: "169 MajorityElement"
date: 2020-07-22T14:54:30+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---

leetcode - 169
```java
package com.kongmu373.leetcode.editor.en;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.Random;

public class MajorityElement {
    public static void main(String[] args) {
        Solution solution = new MajorityElement().new Solution();
        System.out.println(solution.majorityElement(new int[] {3, 3, 4}));
    }

    /**
     *
     */
    //leetcode submit region begin(Prohibit modification and deletion)
    // 1. hashMap
    class Solution1 {
        public int majorityElement(int[] nums) {
            HashMap<Integer, Integer> map = new HashMap<>();
            for (int num : nums) {
                map.put(num, map.getOrDefault(num, 0) + 1);
                if (map.get(num) > nums.length / 2) {
                    return num;
                }
            }
            return -1;
        }
    }

    // 2. random
    class Solution2 {
        public int majorityElement(int[] nums) {
            Random random = new Random();
            int n = nums.length;
            while (true) {
                int index = random.nextInt(n);
                int majority = nums[index];
                int count = 0;
                for (int num : nums) {
                    if (num == majority && ++count > n / 2) {
                        return num;
                    }
                }
            }
        }
    }

    // 3. voting bit.
    class Solution3 {
        public int majorityElement(int[] nums) {
            int n = nums.length;
            int majority = 0;
            for (int i = 0; i < 32; i++) {
                int mask = 1 << i;
                int count = 0;
                for (int num : nums) {
                    if ((num & mask) != 0) {
                        ++count;
                    }
                }
                if (count > n / 2) {
                    majority |= mask;
                }
            }
            return majority;
        }
    }

    // 4. sort 取中间
    class Solution {
        public int majorityElement(int[] nums) {
            int[] array = Arrays.stream(nums).sorted().toArray();
            return array[nums.length / 2];
        }
    }


//leetcode submit region end(Prohibit modification and deletion)

}
```