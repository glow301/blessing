# 并查集
> 并查集是一种树型的数据结构，用于处理一些不交集（Disjoint Sets）的合并及查询问题。有一个联合-查找算法（union-find algorithm）定义了两个用于此数据结构的操作
* 可以参考一下[这篇文章](http://www.csie.ntnu.edu.tw/~u91029/Set.html#8)
* 并查集使用数组存储。

## 并查集的操作
1. 初始化。
1. 查找，确定元素属于哪一个子集，可以用来确认两个元素是否属于同一子集。
1. 合并，将两个子集合并成同一个集合。

### 初始化
```java
public static int[] init(int num) {
    int[] p = new int[num];
    for (int i = 0; i < num; i++) {
        p[i] = i;
    }
    return p;
}
```

### 查找
```java
public static int find(int x) {
    while (x != p[x]) {
        x = p[x];
    }
    return x;
}
```

### 合并
```java
public static int[] setUnion(int x, int y, int[] p) {
    x = find(x, p);
    y = find(y, p);
    p[x] = y;
    return p;
}
```

### 几个优化点
#### 路径压缩
> 对于大量的数据，如果每次都是需要从底层逐步找到根节点，效率就太低了。所以需要采用路径压缩，让每一个子节点直接知道它们最终的根节点是哪个
```java
public static int find(int x) {
    if (x != p[x]) {
        // 把将经过的每一个节点，都直接连接在根节点上
        p[x] = find(p[x]);
    }
    return p[x];
}
```
#### 按秩合并
> 直接合并的话，有可能会导致树的层数很多，这时候需要使用按秩合并，把查询的次数维持在 log 的水平
* 这里 rank 表示每一个集合的元素个数，初始化为 **1**
```java
public static void union(int x, int y) {
    x = find(x);
    y = find(y);
    if (x != y) {
        if (rank[x] >= rank[y]) {
            p[y] = x;
            rank[x] += rank[y];
        } else {
            p[x] = y;
            rank[y] += rank[x];
        }
    }
}
```