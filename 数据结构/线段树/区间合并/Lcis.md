## Lcis

### Description
Given n integers. 
You have two operations: 
U A B: replace the Ath number by B. (index counting from 0) 
Q A B: output the length of the longest consecutive increasing subsequence (LCIS) in [a, b]. 

### Input
T in the first line, indicating the case number. 
Each case starts with two integers n , m(0<n,m<=10 5). 
The next line has n integers(0<=val<=10 5). 
The next m lines each has an operation: 
U A B(0<=A,n , 0<=B=10 5) 
OR 
Q A B(0<=A<=B< n). 

### Output
For each Q, output the answer.

Sample Input
1  
10 10  
7 7 3 3 5 9 9 8 1 8   
Q 6 6  
U 3 4  
Q 0 1  
Q 0 5  
Q 4 7  
Q 3 5  
Q 0 2  
Q 4 6  
U 6 10  
Q 0 9  

### Sample Output
1  
1  
4  
2  
3  
1  
2  
5  

### 问题分析
* 要求最大连续递增序列，先思考一下每个节点需要维护哪些数据。
    1. 和 [Destory Array](./Destory_Array.md) 中求最大连续字串长度一样，首先要维护区间**左起最长递增序列**，**右起最长递增序列**，以及**当前区间最长子序列**（当前区间最长序列，就是在**左起最长连续和**，**右起最长连续和**，**跨越中点最长连续和**，三者之间，取最大值）。
    1. 由于是**递增序列**，所以再计算跨越中点的序列时，我们要判断一下，**左子树的右端点**是否小于**右子树的左端点**，如果小于，需要把两者加起来。所以，还需要维护每个区间左右端点这两个数据。  
    > 综上所述，每个节点需要维护，**左起最长连续和**，**右起最长连续和**，**当前区间最长连续和**，**区间左端点**，**区间右端点**，这 5 个属性。
* pushUp 函数
    1. 首先初始化当前节点数据，由左右子树直接得到。
    1. 如果**左子树右端点**小于**右子树左端点**，此时需要合并左右两个区间。
        1. 如果**左区间长度**等于**左子树的最大连续和**，说明左区间是连续递增的，这时需要延伸右边。
        1. 同理，如果**右区间长度**等于**右子树右起最大连续和**，说明右区间时连续递增的，需要延伸到左边。
        1. 最后，整个区间的最长递增连续和，等于，**左子树最长连续和**，**右子树最长连续和**，**左子树最右连续和 + 右子树最左连续和**，三者之间最大值。
* query 函数
    * 当左子树右端点小于右子树左端点的时候，需要做一下判断
    * **左/右起最长连续和**，要在区间**长度和左/右起连续和**之间取**最小值**，原因是，连续的长度可能会超过区间的查找长度。

### Tips
* pushUp 操作时，更新的一定是当前节点(root)的数据，而不是**左右子树**的数据。
* update 和 query 时，比较的对象，永远都是 mid.
* 动手之前，想一想，线段树维护的节点信息是什么。

### 错误记录
1. 题目查询索引可能从 0 开始，第一次构建的线段树是从 1 开始的，没有注意（要读懂题意）
1. query 函数中，`if (tree[lson].right < tree[rson].left)` 这里条件写错了。

### code
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

#define lson root*2+1
#define rson root*2+2

using namespace std;

const int MAX = 1e5+5;
struct {
    int left, right, lcis, llcis, rlcis;
} tree[MAX<<2];
int input[MAX];

void pushUp(int left, int right, int root) {
    tree[root].left  = tree[lson].left;
    tree[root].right = tree[rson].right;
    tree[root].llcis = tree[lson].llcis;
    tree[root].rlcis = tree[rson].rlcis;
    tree[root].lcis  = max(tree[lson].lcis, tree[rson].lcis);

    if (tree[lson].right < tree[rson].left) {
        int mid = (left + right) >> 1;
        if (tree[root].llcis == mid - left + 1) {
            tree[root].llcis += tree[rson].llcis;
        }
        if (tree[root].rlcis == right - mid) {
            tree[root].rlcis += tree[lson].rlcis;
        }
        tree[root].lcis = max(tree[root].lcis, tree[lson].rlcis + tree[rson].llcis);
    }
}

void build (int left, int right, int root) {
    if (left == right) {
        tree[root].lcis = tree[root].llcis = tree[root].rlcis = 1;
        tree[root].left = tree[root].right = input[left];
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, lson);
    build(mid+1, right, rson);
    pushUp(left, right, root);
}

void update(int l, int c, int left, int right, int root) {
    if (left == right) {
        tree[root].left = tree[root].right = c;
        return;
    }
    int mid = (left + right) >> 1;
    if (l <= mid) {
        update(l, c, left, mid, lson);
    } else {
        update(l, c, mid+1, right, rson);
    }
    pushUp(left, right, root);
}

int query(int ql, int qr, int left, int right, int root) {
    if (ql <= left && qr >= right) {
        return tree[root].lcis;
    }
    int mid = (left + right) >> 1;
    int l = 0, r = 0;
    if (ql <= mid) {
        l = query(ql, qr, left, mid, lson);
    }
    if (qr > mid) {
        r = query(ql, qr, mid+1, right, rson);
    }
    int ans = max(l, r);
    if (tree[lson].right < tree[rson].left) {
        int lrlcis = min(tree[lson].rlcis, mid - ql +1);
        int rllcis = min(tree[rson].llcis, qr - mid);
        ans = max(ans, lrlcis + rllcis);
    }
    return ans;
}

int main() {
    int line;
    scanf("%d", &line);
    while (line--) {
        int n, m;
        scanf("%d %d", &n, &m);
        for (int i = 0; i < n; i++) {
            scanf("%d", &input[i]);
        }
        build(0, n-1, 0);

        while (m--) {
            char op[2];
            int a, b;
            scanf("%s %d %d", op, &a, &b);
            if ('Q' == op[0]) {
                printf("%d\n", query(a, b, 0, n-1, 0));
            } else {
                update(a, b, 0, n-1, 0);
            }
        }
    }
    return 0;
}
```