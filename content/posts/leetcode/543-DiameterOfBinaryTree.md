---
title: "543 DiameterOfBinaryTree"
date: 2020-07-22T15:33:24+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---

## leetcode-543
```java
package com.kongmu373.leetcode.editor.en;

public class DiameterOfBinaryTree {
    public static void main(String[] args) {
        Solution solution = new DiameterOfBinaryTree().new Solution();
    }
    //leetcode submit region begin(Prohibit modification and deletion)

    /**
     * Definition for a binary tree node.
     * <p>
     * }
     */

    class TreeNode {
        int val;
        TreeNode left;
        TreeNode right;

        TreeNode() {
        }

        TreeNode(int val) {
            this.val = val;
        }

        TreeNode(int val, TreeNode left, TreeNode right) {
            this.val = val;
            this.left = left;
            this.right = right;
        }

    }

    /**
     * LP(root) max path length that passes root and at most one of its child.
     * <p>
     * if root is None:
     * LP(root) = -1
     * else:
     * LP(root) = 1 + max(LP(root.left), LP(root.right));
     * ans = max { 2 + LP(root.left) + LP(root.right) }
     */
    class Solution {
        private int res = 0;

        public int diameterOfBinaryTree(TreeNode root) {
            lp(root);
            return res;
        }

        private int lp(TreeNode root) {
            if (root == null) {
                return -1;
            }
            int l = lp(root.left) + 1;
            int r = lp(root.right) + 1;
            res = Math.max(res, l + r);
            return Math.max(l, r);
        }
    }
//leetcode submit region end(Prohibit modification and deletion)

}
```