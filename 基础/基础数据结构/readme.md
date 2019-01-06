## 单链表

### 概念
* 单链表的概念，其实大部分人都很清楚，但是实际操作中，可能在细节上就会出现各种各样的问题。

#### 效率分析
|操作|时间复杂度|
|---|---|
|头部插入|$O(1)$|
|指定位置插入|$O(n)$，需要先找到目标元素|
|查找|$O(n)$|
|删除|$O(n)$|

### 注意事项
* 在指定位置**插入**或删**删除**，都需要先找到前一个节点。 

### 基本操作

#### 初始化
```go
type Node struct {
    Val int
    Next *Node
} 

type LinkedList struct {
    haad *Node
}

func constructor() LinkedList {
    return LinkedList{nil}
}
```
#### 在头部添加元素
```go
func (this *LinkedList) addAtHead(val int) {
    this.head = &Node{val, this.head}
}
```
#### 在尾部添加元素
```go
func (this *LinkedList) addAtTail(val int) {
    p := this.head
    if p == nil {
        this.AddAtHead(val)
        return
    }
    for p.Next != nil {
        p = p.Next
    }
    p.Next = &Node{val, nil}
}
```

#### 在指定位置添加元素
```go
func (this *LinkedList) addAtIndex(index, val int) [
    p := this.head
    if 0 == index {
        this.AddAtHead(val)
        return
    }
    count := 0
    for p != nil {
        // 找到插入节点的前一个节点
        if count == index - 1 {
            node := &Node{val, p.Next}
            p.Next = node
            return
        }
        count++
        p = p.Next
    }
]
```

#### 删除指定位置元素
```go
func (this *LinkedList) deleteAtIndex(index int) {
    p := this.head
    if 0 == index {
        this.head = this.head.Next
        return
    }
    count := 0
    for p != nil {
        // 找到需要删除节点的前一个节点
        if count == index - 1 {
            next := p.Next
            if next != nil {
                p.Next = next.Next
            } else {
                p.Next = nil
            }
            return
        }
        count++
        p = p.Next
    }
}
```

#### 获取指定位置元素
```go
func (this *LinkedList) get(index int) int {
    p, count := this.head, 0
    for p != nil {
        if count == index {
            return p.Val
        }
        p = p.Next
        count++
    }
    return -1
}
```