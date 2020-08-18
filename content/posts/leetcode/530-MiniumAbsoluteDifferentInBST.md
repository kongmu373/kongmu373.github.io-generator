---
title: "530 MiniumAbsoluteDifferentInBST"
date: 2020-07-20T12:33:27+08:00
draft: true
tags: ["BST"]
series: ["easy"]
categories: ["leetcode"]
---
Given a binary search tree with non-negative values, find the minimum absolute
difference between values of any two nodes. 

Example: 


Input:

  1
   \
    3
   /
  2

Output:
1

Explanation:
The minimum absolute difference is 1, which is the difference between 2 and 1 
(or between 2 and 3).




Note: 


There are at least two nodes in this BST. 
This question is the same as 783: https://leetcode.com/problems/minimum-dista
nce-between-bst-nodes/ 

Related Topics Tree
```java



package com.kongmu373.leetcode.editor.en;

public class MinimumAbsoluteDifferenceInBst {
    public static void main(String[] args) {
        Solution solution = new MinimumAbsoluteDifferenceInBst().new Solution();
    }
    //leetcode submit region begin(Prohibit modification and deletion)


    // Definition for a binary tree node.
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
     * BST property:
     * Vals are sorted if trevase the tree in-order
     * <p>
     * func in-order(root):
     * in-order(left)
     * print(root.val)
     * in-order(right)
     * <p>
     * Compare the adjacent elements to get the min_diff.
     */
    class Solution {
        int prev = -1;
        int minDiff = Integer.MAX_VALUE;

        public int getMinimumDifference(TreeNode root) {
            inorder(root);
            return minDiff;
        }

        void inorder(TreeNode root) {
            if (root == null) {
                return;
            }
            inorder(root.left);
            if (prev != -1) {
                minDiff = Math.min(minDiff, root.val - prev);
            }
            prev = root.val;
            inorder(root.right);
        }


    }
//leetcode submit region end(Prohibit modification and deletion)

}
```