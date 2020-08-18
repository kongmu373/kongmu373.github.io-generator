---
title: "707 DesignLinkedList"
date: 2020-07-19T22:24:59+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---
Design your implementation of the linked list. You can choose to use the singl
y linked list or the doubly linked list. A node in a singly linked list should h
ave two attributes: val and next. val is the value of the current node, and next
is a pointer/reference to the next node. If you want to use the doubly linked l
ist, you will need one more attribute prev to indicate the previous node in the 
linked list. Assume all nodes in the linked list are 0-indexed. 

Implement these functions in your linked list class: 


get(index) : Get the value of the index-th node in the linked list. If the in
dex is invalid, return -1. 
addAtHead(val) : Add a node of value val before the first element of the link
ed list. After the insertion, the new node will be the first node of the linked 
list. 
addAtTail(val) : Append a node of value val to the last element of the linked
list. 
addAtIndex(index, val) : Add a node of value val before the index-th node in 
the linked list. If index equals to the length of linked list, the node will be 
appended to the end of linked list. If index is greater than the length, the nod
e will not be inserted. 
deleteAtIndex(index) : Delete the index-th node in the linked list, if the in
dex is valid. 




Example: 


Input: 
["MyLinkedList","addAtHead","addAtTail","addAtIndex","get","deleteAtIndex","ge
t"]
[[],[1],[3],[1,2],[1],[1],[1]]
Output:  
[null,null,null,null,2,null,3]

Explanation:
MyLinkedList linkedList = new MyLinkedList(); // Initialize empty LinkedList
linkedList.addAtHead(1);
linkedList.addAtTail(3);
linkedList.addAtIndex(1, 2);  // linked list becomes 1->2->3
linkedList.get(1);            // returns 2
linkedList.deleteAtIndex(1);  // now the linked list is 1->3
linkedList.get(1);            // returns 3



Constraints: 


0 <= index,val <= 1000 
Please do not use the built-in LinkedList library. 
At most 2000 calls will be made to get, addAtHead, addAtTail, addAtIndex and 
deleteAtIndex. 

Related Topics Linked List Design
```java



package com.kongmu373.leetcode.editor.en;

public class DesignLinkedList {
    public static void main(String[] args) {
        Solution solution = new DesignLinkedList().new Solution();
    }

    /**
     * Singly Linked List.
     * addAtHead: O(1)
     * addAtTail: O(1)
     * addAtIndex: O(n)
     * <p>
     * deleteAtIndex: O(n)
     * <p>
     * get: O(n)
     * <p>
     * Keep tracing head, tail and size of the list.
     */
    //leetcode submit region begin(Prohibit modification and deletion)
    class MyLinkedList {
        /**
         * 声明在栈上的三个变量
         */
        Node head;
        Node tail;
        int size;

        /**
         * Initialize your data structure here.
         */
        public MyLinkedList() {
            // head,tail all set null, size set 0.
            this.head = null;
            this.tail = null;
            this.size = 0;
        }

        /**
         * Get the value of the index-th node in the linked list. If the index is invalid, return -1.
         */
        public int get(int index) {
            // 1. Determine whether index is legal.
            if (index < 0 || index >= this.size) {
                return -1;
            }

            Node temp = this.head;
            while (index != 0) {
                temp = temp.next;
                index--;
            }
            return temp.val;
        }

        /**
         * Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
         */
        public void addAtHead(int val) {
            head = new Node(val, head);
            if (size++ == 0) {
                tail = head;
            }
        }

        /**
         * Append a node of value val to the last element of the linked list.
         */
        public void addAtTail(int val) {
            Node node = new Node(val);
            if (size++ == 0) {
                tail = node;
                head = node;
            } else {
                tail.next = node;
                tail = tail.next;
            }
        }

        /**
         * Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted.
         */
        public void addAtIndex(int index, int val) {
            if (index < 0 || index > size) {
                return;
            }
            if (index == 0) {
                addAtHead(val);
                return;
            }
            if (index == size) {
                addAtTail(val);
                return;
            }

            Node dummy = new Node(0, head);
            Node prev = dummy;
            while(index != 0) {
                prev = prev.next;
                index--;
            }
            prev.next = new Node(val, prev.next);
            ++size;
        }

        /**
         * Delete the index-th node in the linked list, if the index is valid.
         */
        public void deleteAtIndex(int index) {
            if(index < 0 || index >= size) return;
            Node dummy = new Node(0, head);
            Node prev = dummy;
            for (int i = 0; i < index; i++) {
                prev = prev.next;
            }
            Node temp = prev.next.next;
            prev.next.next = null;
            prev.next = temp;
            if(index == 0) head = prev.next;
            if(index == size - 1) tail = prev;
            --size;
        }

        private class Node {
            int val;
            Node next;

            public Node(int val) {
                this.val = val;
            }

            public Node(int val, Node next) {
                this.val = val;
                this.next = next;
            }
        }
    }

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList obj = new MyLinkedList();
 * int param_1 = obj.get(index);
 * obj.addAtHead(val);
 * obj.addAtTail(val);
 * obj.addAtIndex(index,val);
 * obj.deleteAtIndex(index);
 */
//leetcode submit region end(Prohibit modification and deletion)

}
```
