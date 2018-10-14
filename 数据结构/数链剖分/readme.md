## 树链剖分
> 把一棵树剖分成若干条不相交的链，然后用数据结构（树状数组，线段树）去维护每一条链，时间复杂度为 O(log n)。

### 基本概念
| 名称 | 描述 |
|--|--|
| 重节点 | 子树中节点个数最多的节点 |
| 轻节点 | 父节点中，除了重节点之外的节点 |
| 重边 | 父节点和重节点练成的边 |
| 轻边 | 父节点和轻节点练成的边 |
| 重链 | 由多条重边组成的路径 |
| 轻连 | 由多条轻边组成的路径 |

### 一些变量约定
| 变量名 | 说明 |
|--|--|
| size[u] | 以 u 为根的子树，节点的个数 |
| top[u] | 节点 u 所在链的顶端节点 |
| son[u] | 节点 u 的重儿子 |
| dep[u] | 节点 u 的深度 |
| fa[u] | 节点 u 的父节点 |
| tid[u] | 节点 u 被剖分后的新编号（dfs 的执行顺序） |
| rank[u] | 节点 u 在线段树中的位置 |

### 性质
* 在轻边 (u, v) 中，有 size(v) <= size(u) / 2;
* 从根节点到某一节点的路径上，轻边和重链的数量都不超过 log n。

### 求法
> 两次 dfs
* 第一次 dfs，记录所有重边（第一次 dfs 可以找到 重儿子，深度，父节点以及子树节点个数）
* 第二次 dfs，重边组成重链

```cpp
/**
 * u 边的起点 默认 1
 * parent 父节点，默认 -1
 * depth 深度，默认 1
 */
void dfs1(int u, int parent, int depth) {
    dep[u]  = depth;
    size[u] = 1;
    fa[u]   = parent;
    son[u]  = -1;
    for (int i = head[u]; -1 != i; i = edge[i].next) {
        int v = edge[i].to;
        if (v == parent) {
            continue;
        }
        dfs1(v, u, depth+1);
        size[u] += size[v];
        if (-1 == son[u] || size[v] > size[son[u]]) {
            son[u] = v;
        }
    }
}
```

```cpp
/**
 * u 起点，默认 1
 * tp 所在链的顶点，默认 1
 * time 默认 0
 */
void dfs2(int u, int tp) {
    top[u] = tp;
    if (son[u] == -1) {
        return;
    }
    // 这次 dfs 保证优先访问重儿子
    dfs2(son[u], tp);
    for (int i = head[u]; -1 != i; i = edge[i].next) {
        int v = edge[i].to;
        if (v != son[u] && v != fa[u]) {
            dfs2(v, v);
        }
    }
}
```

### 应用
#### LCA
> 最近公共祖先
```cpp
int lca(int x, int y) {
    while(top[x] != top[y]) {
        if (dep[top[x]] > dep[top[y]]) {
            x = fa[top[x]];
        } else {
            y = fa[top[y]];
        }
    }
    return dep[x] > dep[y] ? y : x;
}
```