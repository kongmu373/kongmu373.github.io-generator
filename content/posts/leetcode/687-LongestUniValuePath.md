---
title: "687 LongestUniValuePath"
date: 2020-07-22T16:07:42+08:00
draft: true
---

## Leetcode - 第687题

```java
package com.kongmu373.leetcode.editor.en;

public class LongestUnivaluePath {
    public static void main(String[] args) {
        Solution solution = new LongestUnivaluePath().new Solution();
    }
    //leetcode submit region begin(Prohibit modification and deletion)

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
     * int univaluePath(TreeNode root, int ans)
     * 1. root must be used
     * 2. update ans, can use both children
     * 3. return longest path with only one child
     * 注意：算的edge，不是node.
     */
    class Solution {
        int res = 0;

        public int longestUnivaluePath(TreeNode root) {
            uniValuePath(root);
            return res;
        }

        private int uniValuePath(TreeNode root) {
            if (root == null) {
                return 0;
            }
            int l = 0;
            int r = 0;
            int pl = uniValuePath(root.left);
            int pr = uniValuePath(root.right);
            if (root.left != null && root.left.val == root.val) {
                l = pl + 1;
            }
            if (root.right != null && root.right.val == root.val) {
                r = pr + 1;
            }
            res = Math.max(res, l + r);
            return Math.max(l, r);
        }
    }
//leetcode submit region end(Prohibit modification and deletion)


```
