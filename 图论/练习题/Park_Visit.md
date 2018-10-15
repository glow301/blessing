## Park Visit
> HDU - 4607

### Description
Claire and her little friend, ykwd, are travelling in Shevchenko's Park! The park is beautiful - but large, indeed. N feature spots in the park are connected by exactly (N-1) undirected paths, and Claire is too tired to visit all of them. After consideration, she decides to visit only K spots among them. She takes out a map of the park, and luckily, finds that there're entrances at each feature spot! Claire wants to choose an entrance, and find a way of visit to minimize the distance she has to walk. For convenience, we can assume the length of all paths are 1. 
Claire is too tired. Can you help her? 

### Input
An integer T(T≤20) will exist in the first line of input, indicating the number of test cases. 
Each test case begins with two integers N and M(1≤N,M≤10 5), which respectively denotes the number of nodes and queries. 
The following (N-1) lines, each with a pair of integers (u,v), describe the tree edges. 
The following M lines, each with an integer K(1≤K≤N), describe the queries. 
The nodes are labeled from 1 to N. 

### Output
For each query, output the minimum walking distance, one per line.

### Sample Input
1  
4 2  
3 2  
1 2  
4 2  
2  
4  

### Sample Output
1  
4  

### 问题分析
题目思路和[铲雪](./Two.md)那道题差不多，总体思路都是保证直径上的路径只访问一次。
唯一不同就是本题指定了访问**节点的个数**，最后输出的时候判断一下就好，**小于等于**直径就是`query-1`，大于直径就是`2(query-1)-d`

### 错误记录
1. 多 case 的问题，没有对 head 做初始化。
1. edge 数组没有开两倍空间（无向图，双向加边）。

### Code
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

using namespace std;

const int MAX = 1e5+5;
int head[MAX];
int dis[MAX];
int tot = 0;
struct Edge {
    int to, w, next;
} edge[MAX<<1];

void init(){
    tot = 0;
    memset(head, -1, sizeof(head));
}

void add(int u, int v) {
    edge[tot].to = v;
    edge[tot].w  = 1;
    edge[tot].next = head[u];
    head[u] = tot++;
}

void dfs(int u, int p) {
    for (int i = head[u]; ~i; i = edge[i].next) {
        int v = edge[i].to;
        if (v == p) {
            continue;
        }
        dis[v] = dis[u] + edge[i].w;
        dfs(v, u);
    }
}

int main() {
    int T;
    scanf("%d", &T);
    while (T--) {
        int x, y;
        scanf("%d %d", &x, &y);
        int u, v;
        init();
        for (int i = 1; i < x; i++) {
            scanf("%d %d", &u, &v);
            add(u, v);
            add(v, u);
        }
        dfs(1, -1);
        int tmp = 0, node = 0;
        for (int i = 1; i <= x; i++) {
            if (dis[i] > tmp) {
                tmp = dis[i];
                node = i;
            }
        }
        memset(dis, 0, sizeof(dis));
        dfs(node, -1);
        int d = 0;
        for (int i = 1; i <= x; i++) {
            d = max(d, dis[i]);
        }

        int q;
        while(y--) {
            scanf("%d", &q);
            if (q <= d) {
                printf("%d\n", q-1);
            } else {
                printf("%d\n", 2*(q-1)-d);
            }
        }
    }
    return 0;
}
```
