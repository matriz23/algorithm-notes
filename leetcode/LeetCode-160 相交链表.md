---
title: LeetCode-160 相交链表
date: 2022-05-02 21:00:00
updated:
tags: [计算机, 算法, LeetCode, 链表, Java]
categories: LeetCode题解
cover: https://images.unsplash.com/photo-1471445281684-fdf28e036aa8?ixid=MnwxMTI1OHwwfDF8cmFuZG9tfHx8fHx8fHx8MTY1MTQ4ODcxMw&ixlib=rb-1.2.1&q=85&w=1920
description: LeetCode-160 「相交链表」的思考与解答
---
### [LeetCode-160 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

### Solution 1
利用双指针来一次解决问题.考虑在两条链表长度没有明确关联的情况下,如何让指针的操作有规律且合法?  
我们让指针p1和p2分别指向headA与headB,当指针p1走完A链表后再走B链表,对p2同理.这样一来,两条链表在逻辑上被我们连接到了一起.
- 当p1与p2不相等时便让它们一直往前走,若两条链表有共同节点,便会在p1==p2时停下;
- 若没有共同节点,则两者会同时走到null,注意到这一情况同样符合终止条件.

代码如下:
```Java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode p1 = headA;
        ListNode p2 = headB;
        while (p1 != p2) {
            if (p1 == null) {
                p1 = headB;
            } else {
                p1 = p1.next;
            }
            if (p2 == null) {
                p2 = headA;
            } else {
                p2 = p2.next;
            }
        }
        return p1;
    }
}
```
### Solution 2
同样是连接,这次我们考虑将链表B连接到链表A的末端.观察新链表的形式,我们会发现,判断是否具有共同节点的问题被转换成了判断新链表是否成环.显然这时返回共同节点等价于返回环节点,而这正是我们利用快慢指针解决过的问题.
代码如下:
```Java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode p1 = headA, p2 = headA;
        while (p1.next != null) {
            p1 = p1.next;
        }
        p1.next = headB;
        p1 = headA;
        while (p1 != null && p1.next != null) {
            p1 = p1.next.next;
            p2 = p2.next;
            if (p1 == p2) {
                break;
            }
        }
        if (p1 == null || p1.next == null) {
            return null;
        }
        p2 = headA;
        while (p1 != p2) {
            p1 = p1.next;
            p2 = p2.next;
        }
        return p1;
    }
}
```
不过这个解法在LeetCode判题系统上过不了,是因为题目中强调了"函数返回结果后,链表必须保持其原始结构",但这一思路还是值得参考的.