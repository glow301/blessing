## 线段树
> 线段树是一种二叉搜索树，与区间树相似，它将一个区间划分成一些单元区间，每个单元区间对应线段树中的一个叶结点。

### 参考文章
可以参考一下这篇文章 [链接](https://blog.csdn.net/zearot/article/details/52280189)
* java 中有时候会报 time limt 的错误，这种情况，如果确认是代码没有问题的话，需要把 `scan.next()` 这种改成 `BufferedReader`

### 操作
* 建树（build）
* 更新（update）
* 查询（query）

### 问题分类
* [单点更新](#单点更新)
* [区间更新](#区间更新)
* [区间合并](#区间合并)
* [扫描线](#扫描线)

#### 单点更新
> 单点更新是线段树中最基本的操作，每次更新只会更新一个点的数据。

##### 建树
> 线段树涉及两个数组，一个是保存节点信息的数组，另一个是原始输入数组。
* 这里以求和为例。
```java
private static final int MAX = 10;
private static int[] sum     = new int[MAX<<2];     // 4倍空间
private static int[] arr     = new int[MAX];

public static void build (int left, int right, int root) {
    if (left == right) {
        // sum 表示线段树数组， arr 表示待插入的数组
        sum[root] = arr[left];
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, root*2+1);
    build(mid+1, right, root*2+2);
    pushUp(root);
}

private static void pushUp(int root) {
    sum[root] = sum[root*2+1] + sum[root*2+2];
}
```

##### 更新
```java
private static void update(int l, int c, int left, int right, int root) {
    if (left == right) {
        sum[root] += c;
        return;
    }
    int mid = (left + right) >> 1;
    if (l <= mid) {
        update(l, c, left, mid, root*2+1);
    } else {
        update(l, c, mid+1, right, root*2+2);
    }
    pushUp(root);
}
```

##### 查询
```java
public static int query(int start, int end, int left, int right, int root) {
    if (start <= left && end >= right) {
        return sum[root];
    }
    int mid   = (left + right) >> 1;
    int count = 0;
    if (start <= mid) {
        count += query(start, end, left, mid, root*2+1);
    }
    if (end > mid) {
        count += query(start, end, mid+1, right, root*2+2);
    }
    return count;
}
```

### 区间更新
#### 区间更新与单点更新的区别
##### lazy 标记
维护的数据相比单点更新，需要多维护一个 lazy 标记，lazy 数组的大小与线段树相同，需要开 4 倍空间。

##### 多了一个 pushDown 函数
> pushDown 的主要思想
    * 如果 lazy 标记存在，将 lazy 传递给左右子树，并更新左右子树的数据。
    * 清空本层 lazy 标记。

```cpp
void pushDown(int left, int right, int root) {
    if (lazy[root]) {
        int mid = (left + right) >> 1;
        // lazy 标记传递给左右子树
        lazy[lson] = lazy[rson] = lazy[root];
        // 更新左右子树节点数据
        tree[lson].sum = lazy[root] * (mid - left + 1);
        tree[rson].sum = lazy[root] * (right - mid);
        // 清除本层 lazy 标记
        lazy[root] = 0;
    }
}
```
##### update 时，参数的变化
相比单点更新，区间更新的`update`会传入更新区间的左右端点。所以递归的时候，不能简单的用 if - else 判断，需要判断左右端点与 `mid` 之间的关系。
```cpp
void update(int ql, int qr, int c, int left, int right, int root) {
    if (ql <= left && qr >= right) {
        tree[root].sum = c * (right - left + 1);
        lazy[root] = c;
        return;
    }
    pushDown(left, right, root);
    int mid = (left + right) >> 1;
    // 这里需要两次判断，不能简单的用 if-else
    if (ql <= mid) {
        update(ql, qr, c, left, mid, lson);
    } 
    if (qr > mid) {
        update(ql, qr, c, mid+1, right, rson);
    }
    pushUp(root);
}
```

* query 函数样例
```cpp
long long query(int ql, int qr, int left, int right, int root) {
    if (ql <= left && qr >= right) {
        return tree[root].sum;
    }
    pushDown(left, right, root);
    int mid = (left + right) >> 1;
    long long l = 0, r = 0;
    if (ql <= mid) {
        l += query(ql, qr, left, mid, lson);
    }
    if (qr > mid) {
        r += query(ql, qr, mid+1, right, rson);
    }
    return l + r;
}
```

### 区间合并
#### 区间合并与单点更新的主要区别
1. 单点更新大部分情况下，一个节点只需要维护一个数据即可，一般 tree 是一个整型数组。对于区间合并问题，一般 tree 是一个结构体数组，一个节点通常需要维护多个数据。
1. 单点更新中，pushUp 函数大部分只做一下简单的运算（求和，最大值等），不涉及逻辑判断。在区间合并中，通常需要做一些逻辑判断，根据条件来更新数据。

### 扫描线