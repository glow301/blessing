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
* 区间更新
* 区间合并
* 扫描线

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

### 区间合并
