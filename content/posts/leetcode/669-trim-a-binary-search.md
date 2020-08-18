---
title: "669 Trim a Binary Search"
date: 2020-07-23T11:02:35+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---

## leetcode - 669 - trim a Binary Search
```java
package com.kongmu373.leetcode.editor.en;

public class TrimABinarySearchTree {
    public static void main(String[] args) {
        Solution solution = new TrimABinarySearchTree().new Solution();
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

    class Solution {
        public TreeNode trimBST(TreeNode root, int L, int R) {
            if (root == null) {
                return null;
            }
            if (root.val < L) {
                return trimBST(root.right, L, R);
            }
            if (root.val > R) {
                return trimBST(root.left, L, R);
            }
            root.left = trimBST(root.left, L, R);
            root.right = trimBST(root.right, L, R);
            return root;
        }
    }
//leetcode submit region end(Prohibit modification and deletion)

}
```

