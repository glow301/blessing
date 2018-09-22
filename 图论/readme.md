## 图论
### 建图
> 使用数组形式的[邻接表](https://zh.wikipedia.org/wiki/%E9%82%BB%E6%8E%A5%E8%A1%A8)存储

#### 链式前向星
> 参考链接 https://blog.csdn.net/acdreamers/article/details/16902023

#### 存储方式
* 使用两个数组来表示
    * `edge` 数组用来存储**边**的信息。
    * `head` 数组用来存储**点**的信息（head 数组初始化全部为 -1）。

* 先看一下 `Edge` 结构体的定义
```cpp
const int MAX = 1e4;

struct Edge {
    int next;   // 下一条边存储的位置
    int to;     // 边的终点
    int w;      // 边的权值
} edge[MAX];
```
* 这里需要注意一点，对于无向图而言，`edge`数组的大小要开两倍。

我们使用 `from` 表示一条边的起点，`to` 表示一条边的终点， `w` 表示这条边的权值。
那么，数组中元素的含义如下
* edge[i].next  
    > 与第 i 条边同起点的下条边的位置（也就是在 `edge` 数组中的索引）
* edge[i].to  
    > 第 i 条边的起点
* edge[i].w  
    > 第 i 条边的权值
* head[i]  
    > 以 i 为起点的第一条边的存储位置

##### 添加一条边
* 对于无向图，需要调用两次 add 函数
    ```cpp
    add(u, v, w);
    add(v, u, w);
    ```
    
    ```cpp
    /**
    * 说明
    * u, v, w 分别表示边的 起点，重点，权值
    * tot 是数组的索引
    */
    void add(int u, int v, int w) {
        edge[tot].v = v;
        edge[tot].w = w;
        edge[tot].next = head[u];
        head[u] = tot++;
    }
   ```

##### 遍历
```cpp
for (int i = head[u]; i != -1; i = edge[i].next) {
    // ...
}
```

#### 树的直径
> 树的直径指的是，树上的最长简单路。

##### 求法
> 两遍 DFS 或 BFS
1. 选取任意一个顶点作为起点，对树进行搜索，找到距离该点最远的点 v。
1. 以 v 为起点，再进行搜索，找到距离 v 最远的点 u, u 到 v 之间的距离就是树的直径。
```cpp
/**
 * @params u 作为起点搜索的点
 * @params parent 标记该点是否被访问过
 * dis[v] 表示 以 v 为终点到 u 的距离
 */
void dfs (int u, int parent) {
    for (int i = head[u]; -1 != i; i = edge[i].next) {
        v = edge[i].to;
        if (v == parent) {
            continue;
        }
        dis[v] = dis[u] + edge[i].w;
        dfs(v, u);
    }
}
```

#### 树的重心
