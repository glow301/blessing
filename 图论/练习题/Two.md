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

### Code
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

using namespace std;

const int MAX = 1e5+5;
int head[MAX];
struct Edgege {
    int to, w, next;
} edge[MAX<<1];
int dis[MAX];
int tot = 0;

void add(int u, int v, int w) {
    edge[tot].to = v;
    edge[tot].w  = w;
    edge[tot].next = head[u];
    head[u] = tot++;
}

void dfs(int u, int parent) {
    for (int i = head[u]; -1 != i; i = edge[i].next) {
        int v = edge[i].to;
        if (v == parent) {
            continue;
        }
        dis[v] = dis[u] + edge[i].w;
        dfs(v, u);
    }
}

int main() {
    int n, s;
    while (~scanf("%d %d", &n, &s)) {
        int a, b, c;
        int total = 0;
        memset(head, -1, sizeof(head));
        for (int i = 1; i < n; i++) {
            scanf("%d %d %d", &a, &b, &c);
            add(a, b, c);
            add(b, a, c);
            total += c;
        }

        dfs(s, -1);
        int tmp = 0, v = 0;
        for (int i = 1; i <= n; i++) {
            if (dis[i] > tmp) {
                tmp = dis[i];
                v = i;
            }
        }

        memset(dis, 0, sizeof(dis));
        dfs(v, -1);
        int res = 0;
        for (int i = 1; i <= n; i++) {
            res = max(res, dis[i]);
        }
        printf("%d\n", total*2-res);
    }
    return 0;
}
```