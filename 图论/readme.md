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
    int next;   // 下一条边存储的位置
    int to;     // 边的终点
    int w;      // 边的权值
} edge[MAX]
```
我们使用 `from` 表示一条边的起点，`to` 表示一条边的终点， `w` 表示这条边的权值。
那么，数组中元素的含义如下
* edge[i].next
    与第 i 条边同起点的下条边的位置（也就是在 `edge` 数组中的索引）
* edge[i].to
    第 i 条边的起点
* edge[i].w
    第 i 条边的权值
* head[i]
    以 i 为起点的第一条边的存储位置

##### 添加一条边
```cpp
/**
 * 说明
 * u, v, w 分别表示边的 起点，重点，权值
 * tot 是数组的索引
 */
void add(int u, int v, int w) {
    edge[tot].v = v;
    edge[tot].w = w;
    edge[tot].next = head[u];
    head[u] = tot++;
}
```

##### 遍历
```cpp
for (int i = head[u]; i != -1; i = edge[i].next) {
    // ...
}
```
