---
title: "108 ConvertSortedArraytoBinarySearchTree"
date: 2020-07-18T14:51:38+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---

Given an array where elements are sorted in ascending order, convert it to a height balanced BST. 

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.



Example:


Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following he
ight balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5

 Related Topics Tree Depth-first Search

```java
package com.kongmu373.leetcode.editor.en;

public class ConvertSortedArrayToBinarySearchTree {
    public static void main(String[] args) {
        Solution solution = new ConvertSortedArrayToBinarySearchTree().new Solution();
    }
    //leetcode submit region begin(Prohibit modification and deletion)

    //  Definition for a binary tree node.
    public class TreeNode {
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
        /**
         * Approach: Recursion / Divide and conquer
         * BST 定义: Vals(root.left) <= root.val <= Vals(root.right)
         * Height balanced: Node(root.left) - Node(root.right) <= 1   (左右子树的高度差不能超过1)
         * sample:  l ... m-1 m m+1 ... r
         * 伪代码:
         * def build(arr, l, r):
         * if l > r : return None
         * m = l + (r - l) / 2
         * root = TreeNode(arr[m])
         * root.left = build(arr, l, m - 1)
         * root.right = build(arr, m + 1, r)
         * return root
         * T(n) = 2 * T(n/2) + O(1)
         * Time complexity: O(n) / Space complexity: O(logn)
         *
         * @param nums 一个 升序排序的数组
         * @return 一个平衡二叉搜索树
         */
        public TreeNode sortedArrayToBST(int[] nums) {
            return buildBST(nums, 0, nums.length - 1);
        }

        private TreeNode buildBST(int[] nums, int l, int r) {
            if (l > r) {
                return null;
            }
            int m = l + (r - l) / 2;
            TreeNode root = new TreeNode(nums[m]);
            root.left = buildBST(nums, l, m - 1);
            root.right = buildBST(nums, m + 1, r);
            return root;
        }
    }
//leetcode submit region end(Prohibit modification and deletion)
}
```