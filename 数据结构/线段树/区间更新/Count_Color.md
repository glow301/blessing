## Count Color
> POJ - 2777

### Description
Chosen Problem Solving and Program design as an optional course, you are required to solve all kinds of problems. Here, we get a new problem. 

There is a very long board with length L centimeter, L is a positive integer, so we can evenly divide the board into L segments, and they are labeled by 1, 2, ... L from left to right, each is 1 centimeter long. Now we have to color the board - one segment with only one color. We can do following two operations on the board: 

1. "C A B C" Color the board from segment A to segment B with color C. 
2. "P A B" Output the number of different colors painted between segment A and segment B (including). 

In our daily life, we have very few words to describe a color (red, green, blue, yellow…), so you may assume that the total number of different colors T is very small. To make it simple, we express the names of colors as color 1, color 2, ... color T. At the beginning, the board was painted in color 1. Now the rest of problem is left to your. 

### Input
First line of input contains L (1 <= L <= 100000), T (1 <= T <= 30) and O (1 <= O <= 100000). Here O denotes the number of operations. Following O lines, each contains "C A B C" or "P A B" (here A, B, C are integers, and A may be larger than B) as an operation defined previously.

### Output
Ouput results of the output operation in order, each line contains a number.

### Sample Input
2 2 4  
C 1 1 2  
P 1 2  
C 2 2 2  
P 1 2  

### Sample Output
2  
1  

### 问题分析

### Code
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

using namespace std;

#define lson root<<1
#define rson root<<1|1

const int MAX = 1e5+5;
int lazy[MAX<<2];
struct {
    int sum;
} tree[MAX<<2];

void pushUp(int root) {
    tree[root].sum = tree[lson].sum | tree[rson].sum;
}

void pushDown(int left, int right, int root) {
    if (lazy[root]) {
        lazy[lson] = lazy[rson] = lazy[root];
        tree[lson].sum = tree[rson].sum = (1<<(lazy[root]-1));
        lazy[root] = 0;
    }
}

void build(int left, int right, int root) {
    lazy[root] = 0;
    if (left == right) {
        tree[root].sum = 1;
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, lson);
    build(mid+1, right, rson);
    pushUp(root);
}

void update(int ql, int qr, int c, int left, int right, int root) {
    if (ql <= left && qr >= right) {
        lazy[root] = c;
        tree[root].sum = 1<<(c-1);
        return;
    }
    pushDown(left, right, root);
    int mid = (left + right) >> 1;
    if (ql <= mid) {
        update(ql, qr, c, left, mid, lson);
    }
    if (qr > mid) {
        update(ql, qr, c, mid+1, right, rson);
    }
    pushUp(root);
}

int query(int ql, int qr, int left, int right, int root) {
    if (ql <= left && qr >= right) {
        return tree[root].sum;
    }
    pushDown(left, right, root);
    int mid = (left + right) >> 1;
    int l = 0, r = 0;
    if (ql <= mid) {
        l = query(ql, qr, left, mid, lson);
    }
    if (qr > mid) {
        r = query(ql, qr, mid+1, right, rson);
    }
    return l | r;
}

int main() {
    int l, t, o;
    while (~scanf("%d %d %d", &l, &t, &o)) {
        build(1, l, 1);
        while (o--) {
            char op[2];
            int x, y, z;
            scanf("%s", op);
            if ('C' == op[0]) {
                scanf("%d %d %d", &x, &y, &z);
                if (x > y) {
                    swap(x, y);
                }
                update(x, y, z, 1, l, 1);
            } else if ('P' == op[0]) {
                scanf("%d %d", &x, &y);
                if (x > y) {
                    swap(x, y);
                }
                int ans = query(x, y, 1, l, 1);
                int res = 0;
                for (int i = 0; i < t; i++) {
                    if (ans & (1 << i)) {
                        res++;
                    }
                }
                printf("%d\n", res);
            }
        }
    }
}
```