Edited by: Orange Liu,  Feb. 7 2024

过年前最后一次总结Leetcode中有关链表题目。
其实链表题目套路不是很多，更多的是考察链表的结构，以及如何根据结构来对链表进行一系列的操作。下面我将介绍两种链表中经常使用的思路。

```python
# Definition for singly-linked list.

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

# 虚拟头节点

## 概念

由上述对链表的定义我们可以知道，不管是增加还是删除还是在内存中修改，我们在操作相关节点的同时，还需对该节点的前置、后置节点进行修改（next），这样一来，我们可以发现在操作真实头节点时，会出现不存在前置节点的情况，导致算法不够严谨，需单独将头节点列出。

因此为了让所有节点的可操作性一致，我们引入虚拟头节点的概念，新建ListNode示例，并将该实例的next指向真正的头节点，最终返回真正头节点即可。

## 题目示例

[203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

```python
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        dummy_head = ListNode(next = head)
        current = dummy_head
        while current.next:
            if current.next.val == val:
                current.next = current.next.next
            else:
                current = current.next
        return dummy_head.next
```

其中dummy_head就是虚拟头节点，因为其数值没有意义，因此将从真正的头节点进行判断，这样一来在循环中就不用考虑头节点的问题，在python中也不会出现
```python
Error: NoneType object does not have attribute 'next'
```
的情况，方便链表操作。


# 与数组相关方法结合