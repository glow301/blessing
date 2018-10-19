## Godfather 
> POJ - 3107

### Description
Last years Chicago was full of gangster fights and strange murders. The chief of the police got really tired of all these crimes, and decided to arrest the mafia leaders.

Unfortunately, the structure of Chicago mafia is rather complicated. There are n persons known to be related to mafia. The police have traced their activity for some time, and know that some of them are communicating with each other. Based on the data collected, the chief of the police suggests that the mafia hierarchy can be represented as a tree. The head of the mafia, Godfather, is the root of the tree, and if some person is represented by a node in the tree, its direct subordinates are represented by the children of that node. For the purpose of conspiracy the gangsters only communicate with their direct subordinates and their direct master.

Unfortunately, though the police know gangsters’ communications, they do not know who is a master in any pair of communicating persons. Thus they only have an undirected tree of communications, and do not know who Godfather is.

Based on the idea that Godfather wants to have the most possible control over mafia, the chief of the police has made a suggestion that Godfather is such a person that after deleting it from the communications tree the size of the largest remaining connected component is as small as possible. Help the police to find all potential Godfathers and they will arrest them.

### Input
The first line of the input file contains n — the number of persons suspected to belong to mafia (2 ≤ n ≤ 50 000). Let them be numbered from 1 to n.

The following n − 1 lines contain two integer numbers each. The pair ai, bi means that the gangster ai has communicated with the gangster bi. It is guaranteed that the gangsters’ communications form a tree.

### Output
Print the numbers of all persons that are suspected to be Godfather. The numbers must be printed in the increasing order, separated by spaces.

### Sample Input
6
1 2
2 3
2 5
3 4
3 6

### Sample Output
2 3

### 问题分析
* 基本思路是求树的重心，并按顺序输出重心节点的编号（一棵树只可能有 1 个或 两个重心）。
* 通过一遍 dfs 求出树的重心，在 balance 数组中求出 balance[i] 最小的即可。

### Code
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

using namespace std;

const int MAX = 5e4+5;

int head[MAX];
struct Edge {
    int to, w, next;
} edge[MAX<<1];
int size[MAX];
int balance[MAX];
int tot = 0;
int n = 0;

void init() {
    tot = 0;
    memset(head, -1, sizeof(head));
}

void add(int u, int v, int w) {
    edge[tot].to = v;
    edge[tot].w  = w;
    edge[tot].next = head[u];
    head[u] = tot++;
}

void dfs(int u, int parent) {
    size[u] = 1;
    for (int i = head[u]; -1 != i; i = edge[i].next) {
        int v = edge[i].to;
        if (v == parent) {
            continue;
        }
        dfs(v, u);
        size[u] += size[v];
        balance[u] = max(balance[u], size[v]);
    }
    balance[u] = max(balance[u], n - size[u]);
}

int main() {
    while(~scanf("%d", &n)) {
        init();
        int a, b;
        for (int i = 1; i < n; i++) {
            scanf("%d %d", &a, &b);
            add(a, b, 1);
            add(b, a, 1);
        }

        dfs(1, -1);
        int tmp = n, node = 0;
        for (int i = 1; i <= n; i++) {
            if (balance[i] < tmp) {
                node = i;
                tmp = balance[i];
            }
        }
        int flag = 0;
        for (int i = 1; i <= n; i++) {
            if (balance[i] == tmp) {
                printf("%s%d", 0==flag ? "" : " ", i);
                flag = 1;
            }
        }
        printf("\n");
    }
    return 0;
}
```

##### Go 语言版本
```go
package main

import "fmt"

const MAX = 5e4+5

type Edge struct {
    to, w, next int
}

var (
    head [MAX]int
    tot  int
    n int
    size [MAX]int
    balance [MAX]int
    edge [MAX<<1]Edge
)

func memset(arr []int, v int) {
    for key := range arr {
        arr[key] = v
    }
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func add(u, v, w int) {
    edge[tot].to = v
    edge[tot].w  = w
    edge[tot].next = head[u]
    head[u] = tot
    tot++
}

func dfs(u, p int) {
    size[u] = 1
    for i := head[u]; -1 != i; i = edge[i].next {
        v := edge[i].to
        if v == p {
            continue
        }
        dfs(v, u)
        size[u] += size[v]
        balance[u] = max(balance[u], size[v])
    }
    balance[u] = max(balance[u], n - size[u])
}

func main() {
    for {
        if _, err := fmt.Scanf("%d\n", &n); nil != err {
            break
        }
        memset(head[:], -1)
        memset(balance[:], 0)
        var u, v int
        for i := 1; i < n; i++ {
            fmt.Scanf("%d %d\n", &u, &v)
            add(u, v, 1)
            add(v, u, 1)
        }

        dfs(1, -1)
        tmp := n
        for i := 1; i <= n; i++ {
            if balance[i] < tmp {
                tmp = balance[i]
            }
        }

        for i := 1; i <= n; i++ {
            if balance[i] == tmp {
                fmt.Printf("%d ", i)
            }
        }
        fmt.Println()
    }
}
```