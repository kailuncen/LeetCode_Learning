## 题目要求
> You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.You may assume the two numbers do not contain any leading zero, except the number 0 itself.

> 给出两个非空的链表用来表示两个非负的整数。其中，它们各自的位数是按照逆序的方式存储的，并且它们的每个节点只能存储一位数字。如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
链接：https://leetcode-cn.com/problems/add-two-numbers

## 示例
> Input: (2 -> 4 -> 3) + (5 -> 6 -> 4) Output: 7 -> 0 -> 8 Explanation: 342 + 465 = 807.

> 输入：(2 -> 4 -> 3) + (5 -> 6 -> 4) 输出：7 -> 0 -> 8 原因：342 + 465 = 807

## 题解思路
题目就是将一个整数相加的问题，使用链表来呈现了。我想到的方法就是去遍历两个链表，每两个数字相加后，会产生一个进位 carry，进位会带到下一轮继续使用，
同时计算后的结果会成为新链表的节点，例如 4 和 6 相加，carry 1带入下一轮继续使用, 0 成为新链表的其中一个节点。整体难度不是很大，注意考虑两个链表不是
等长的，链表循环到末尾也可能产生进位，出现新节点，以及使用额外节点保存头结点即可解决这个问题。

##### 示例代码：
**Java版本**
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode current = new ListNode(0);
        ListNode p = l1, q = l2;
        int carry = 0;
        ListNode dumpHead = current;  // 额外的哑节点用来保存头部节点,因为最后要进行返回
        while (p != null || q != null) {
            int number1 = (p != null) ? p.val : 0;
            int number2 = (q != null) ? q.val : 0;
            int result = number1 + number2 + carry;
            carry = result / 10;
            ListNode next = new ListNode(result % 10);
            current.next = next;
            current = current.next; // 继续往下移动
            
            // 原有两个列表的节点继续往下移动
            if (p != null) {
                p = p.next;
            }
            if (q != null) {
                q = q.next;
            }
        }

        // 如果最后链表的数字加完还有进位的话，那么还需要额外再创建出一个节点
        if (carry > 0) {
            current.next = new ListNode(carry);
        }
        return dumpHead.next;
    }
}
```
**Python版本**
```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        current = ListNode(0)
        dumpHead = current
        p = l1
        q = l2
        carry = 0
        while p  or q :
            number1 = (p.val if p else 0)
            number2 = (q.val if q else 0)
            result = number1 + number2 + carry
            carry = result // 10
            current.next = ListNode(result % 10)
            current = current.next

            if p is not None:
                p = p.next
            if q is not None:
                q = q.next

        if carry:
            current.next = ListNode(carry)

        return dumpHead.next

```
##### 复杂度分析：
时间复杂度：O(max(m,n))，m和n分别为两个链表的长度，最多循环max(m,n)次。
空间复杂度：O(max(m,n))，因为引入额外的链表存储，最多存储空间为max(m,n)+1,因为最后一位可能额外产生新节点。

## Tag

* 哑节点
* 链表

![file](https://graph.baidu.com/resource/222e868080a15c56201de01577006737.png)
