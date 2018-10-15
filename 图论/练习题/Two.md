## Two
> POJ - 1849

### Description
The city consists of intersections and streets that connect them. 

Heavy snow covered the city so the mayor Milan gave to the winter-service a list of streets that have to be cleaned of snow. These streets are chosen such that the number of streets is as small as possible but still every two intersections to be connected i.e. between every two intersections there will be exactly one path. The winter service consists of two snow plovers and two drivers, Mirko and Slavko, and their starting position is on one of the intersections. 

The snow plover burns one liter of fuel per meter (even if it is driving through a street that has already been cleared of snow) and it has to clean all streets from the list in such order so the total fuel spent is minimal. When all the streets are cleared of snow, the snow plovers are parked on the last intersection they visited. Mirko and Slavko don’t have to finish their plowing on the same intersection. 

Write a program that calculates the total amount of fuel that the snow plovers will spend. 

### Input
The first line of the input contains two integers: N and S, 1 <= N <= 100000, 1 <= S <= N. N is the total number of intersections; S is ordinal number of the snow plovers starting intersection. Intersections are marked with numbers 1...N. 

Each of the next N-1 lines contains three integers: A, B and C, meaning that intersections A and B are directly connected by a street and that street's length is C meters, 1 <= C <= 1000. 

### Output
Write to the output the minimal amount of fuel needed to clean all streets.

### Sample Input
5 2  
1 2 1  
2 3 2  
3 4 2  
4 5 1  

### Sample Output
6 

### 问题分析
* 题目要求遍历所有的点，求出路径的最小值。
* 树上最上的路径是树的直径，既然要求最小值，我们要满足**直径上的点只走一次**，其他的点必然要走两次才能遍历完所有的点。假设路径的总和是 N，树的直径是 d。
其实，答案应该是 (N-d) * 2 + d = 2N - d

### Code
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

using namespace std;

const int MAX = 1e5+5;
int dis[MAX];
int head[MAX];
int tot = 0;
struct Edge {
    int to, w, next;
} edge[MAX<<1];

void init() {
    tot = 0;
    memset(dis, 0, sizeof(dis));
    memset(head, -1, sizeof(head));
}

void add(int u, int v, int w) {
    edge[tot].to = v;
    edge[tot].w  = w;
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
    int x, y;
    while (~scanf("%d %d", &x, &y)) {
        init();
        int u, v, w;
        int len = 0;
        for (int i = 1; i < x; i++) {
            scanf("%d %d %d", &u, &v, &w);
            add(u, v, w);
            add(v, u, w);
            len += w;
        }
        dfs(y, -1);
        int tmp = 0, node = 0;
        for (int i = 1; i<= x; i++) {
            if (dis[i] > tmp) {
                tmp = dis[i];
                node = i;
            }
        }
        memset(dis, 0, sizeof(dis));
        
        dfs(node, -1);
        int res = 0;
        for (int i = 1; i <= x; i++) {
            res = max(res, dis[i]);
        }
        printf("%d\n", 2*len-res);
    }
    return 0;
}
```