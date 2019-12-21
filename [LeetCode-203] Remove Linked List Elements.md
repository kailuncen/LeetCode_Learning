## 题目要求
> Remove all elements from a linked list of integers that have value **_val_** .

> 删除链表中等于给定值 **_val _** 的所有节点。

## 示例
> **Input:**  1->2->6->3->4->5->6, _**val**_ = 6
**Output:** 1->2->3->4->5

> **输入:** 1->2->6->3->4->5->6, _**val**_ = 6
**输出:** 1->2->3->4->5

## 题解思路
想到常规解法是比较简单的，因为是单向链表，不能够滑到指定节点再进行删除，因为没法取到前一个节点。因此是通过判断当前的节点的下一个节点的值是否是要删除的值，如果是的话，那么当前的节点的下一个节点替换成下下个节点，如果不是的话，继续往下滑动。

但这里需要考虑一个特殊情况，可能存在头节点就是要删除的节点的情况，因此通过增加一个哑结点，哑结点的下一个节点记头部节点，就可以使用上面的逻辑了。


## 解题代码
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
    public ListNode removeElements(ListNode head, int val) {
        ListNode temp = new ListNode(0);
        temp.next = head;
        ListNode prev = temp;

        while(prev.next != null) {
            if (prev.next.val == val) {
                prev.next = prev.next.next;
            } else {
                prev = prev.next;
            }
        }

        return temp.next;
    }
}
```

**Python3版本**
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        temp = ListNode(0)
        temp.next = head
        prev = temp

        while(prev.next != None):
            if (prev.next.val == val):
                prev.next = prev.next.next
            else:
                prev = prev.next
        
        return temp.next
```

