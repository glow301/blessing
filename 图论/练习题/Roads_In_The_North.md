## Roads in the North
> POJ - 2631

### Description
Building and maintaining roads among communities in the far North is an expensive business. With this in mind, the roads are build such that there is only one route from a village to a village that does not pass through some other village twice. 
Given is an area in the far North comprising a number of villages and roads among them such that any village can be reached by road from any other village. Your job is to find the road distance between the two most remote villages in the area. 

The area has up to 10,000 villages connected by road segments. The villages are numbered from 1. 

### Input
Input to the problem is a sequence of lines, each containing three positive integers: the number of a village, the number of a different village, and the length of the road segment connecting the villages in kilometers. All road segments are two-way.

### Output
You are to output a single integer: the road distance between the two most remote villages in the area.

### Sample Input
5 1 6  
1 4 5  
6 3 9  
2 6 8  
6 1 7  

### Sample Output
22

### 问题分析
* 题目要求找到这些村庄之间，最远的两个村庄之间的距离。把每一个村庄看成一个点，把每个村庄连起来，就形成了一棵树，其实就是求这棵树上的直径。
* 调用两次 dfs 求出直径即可。

### Tips
* 这个题需要注意一点，如果输入为空的情况下，需要输出 0。

### Code
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

using namespace std;

const int MAX = 10005;

int head[MAX];
struct Edge {
    int to, next, w;
} edge[MAX<<1];

int dis[MAX];
int tot = 0;

void add(int u, int v, int w) {
    edge[tot].to = v;
    edge[tot].w  = w;
    edge[tot].next = head[u];
    head[u] = tot++;
}

void dfs(int u, int p) {
    for (int i = head[u]; -1 != i; i = edge[i].next) {
        int v = edge[i].to;
        if (v == p) {
            continue;
        }
        dis[v] = dis[u] + edge[i].w;
        dfs(v, u);
    }
}

int main() {
    int x = 0, y = 0, z = 0, len = 0;
    memset(head, -1, sizeof(head));
    while (~scanf("%d %d %d", &x, &y, &z)) {
        add(x, y, z);
        add(y, x, z);
        len = max(x, y);
    }
    if (0 == x) {
        printf("0\n");
        return 0;
    }

    dfs(1, -1);
    int tmp = 0, node = 0;
    for (int i = 1; i <= len; i++) {
        if (dis[i] > tmp) {
            tmp = dis[i];
            node = i;
        }
    }
    memset(dis, 0, sizeof(dis));
    dfs(node, -1);
    int res = 0;
    for (int i = 1; i <= len; i++) {
        if (dis[i] > res) {
            res = dis[i];
        }
    }
    printf("%d\n", res);
    return 0;
}
```