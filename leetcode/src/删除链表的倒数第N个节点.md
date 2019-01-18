## 删除链表的倒数第N个节点
> leetcode - 19

### Description
给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

### 示例
```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

### 说明
给定的 n 保证是有效的。

### 分析
1. 定义快慢两个指针，当快指针走到 n 时，慢指针开始走。
1. 当快指针走到结尾时，慢指针刚好位于倒数第 n 个位置。
1. 删除慢指针对应的元素即可。

### Code
```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    if head == nil {
        return head
    }
    p, fast, slow := head, head, head
    count := 0
    for fast != nil {
        // 找到被删除节点的前一个节点
        if count >= n + 1 {
            slow = slow.Next
        }
        fast = fast.Next
        count++
    }
    if count == n {
        return p.Next
    }
    slow.Next = slow.Next.Next
    return p
}
```