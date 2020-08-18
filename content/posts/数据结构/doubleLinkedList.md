---
title: "DoubleLinkedList"
date: 2020-07-26T08:50:55+08:00
draft: true
categories: ["数据结构"]
---

## 双向链表的实现

```java
/**
 * struct Node includes: next, prev, no,data
 * <p>
 * CRUD:
 * <p>
 * 1) 遍历与单链表一样，真是可以向前，向后
 * <p>
 * 2) 添加（默认添加到双向链表的最后)
 * 2.1) temp.next = node
 * 2.2) node.prev = temp
 * <p>
 * 3) 修改 思路和单向链表一样
 * <p>
 * 4) 删除
 * 4.1) temp.prev.next = temp.next
 * 4.2) temp.next.prev = temp.prev;
 */
public class DoubleLinkedList {

    private Node head = new Node(0, "");

    public Node getHead() {
        return head;
    }

    public void show() {
        if (isEmpty()) {
            return;
        }
        Node p = head.next;
        while (p != null) {
            System.out.println(p);
            p = p.next;
        }
    }

    public void add(Node node) {
        Node p = head;
        while (p.next != null) {
            p = p.next;
        }
        p.next = node;
        node.prev = p;
    }

    public boolean update(Node node) {
        if (isEmpty()) {
            return false;
        }
        Node p = head.next;
        while (p != null) {
            if (p.no == node.no) {
                p.data = node.data;
                return true;
            }
            p = p.next;
        }
        return false;
    }

    public void del(int no) {
        if (isEmpty()) {
            return;
        }
        Node p = head.next;
        while (p != null) {
            if (p.no == no) {
                p.prev.next = p.next;
                if (p.next != null) {
                    p.next.prev = p.prev;
                }
                return;
            }
            p = p.next;
        }
    }

    private boolean isEmpty() {
        if (head.next == null) {
            System.out.println("链表为空~");
            return true;
        }
        return false;
    }

    public static class Node {
        int no;
        String data;
        Node next;
        Node prev;

        public Node(int no, String data) {
            this.no = no;
            this.data = data;
        }

        @Override
        public String toString() {
            return "Node{" +
                       "no=" + no +
                       ",data=" + data +
                       '}';
        }
    }
}
```
