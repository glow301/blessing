## A Corrupt Mayor's Performance Art
> HDU - 5023

### Description
Because a lot of people praised mayor X's painting(of course, X was a mayor), mayor X believed more and more that he was a very talented painter. Soon mayor X was not satisfied with only making money. He wanted to be a famous painter. So he joined the local painting associates. Other painters had to elect him as the chairman of the associates. Then his painting sold at better price.

The local middle school from which mayor X graduated, wanted to beat mayor X's horse fart(In Chinese English, beating one's horse fart means flattering one hard). They built a wall, and invited mayor X to paint on it. Mayor X was very happy. But he really had no idea about what to paint because he could only paint very abstract paintings which nobody really understand. Mayor X's secretary suggested that he could make this thing not only a painting, but also a performance art work.

This was the secretary's idea:

The wall was divided into N segments and the width of each segment was one cun(cun is a Chinese length unit). All segments were numbered from 1 to N, from left to right. There were 30 kinds of colors mayor X could use to paint the wall. They named those colors as color 1, color 2 .... color 30. The wall's original color was color 2. Every time mayor X would paint some consecutive segments with a certain kind of color, and he did this for many times. Trying to make his performance art fancy, mayor X declared that at any moment, if someone asked how many kind of colors were there on any consecutive segments, he could give the number immediately without counting.

But mayor X didn't know how to give the right answer. Your friend, Mr. W was an secret officer of anti-corruption bureau, he helped mayor X on this problem and gained his trust. Do you know how Mr. Q did this?

### Input
There are several test cases.
For each test case:
The first line contains two integers, N and M ,meaning that the wall is divided into N segments and there are M operations(0 < N <= 1,000,000; 0<M<=100,000) 
Then M lines follow, each representing an operation. There are two kinds of operations, as described below: 
1) P a b c 
a, b and c are integers. This operation means that mayor X painted all segments from segment a to segment b with color c ( 0 < a<=b <= N, 0 < c <= 30).
2) Q a b
a and b are integers. This is a query operation. It means that someone asked that how many kinds of colors were there from segment a to segment b ( 0 < a<=b <= N).
Please note that the operations are given in time sequence.
The input ends with M = 0 and N = 0.

### Output
For each query operation, print all kinds of color on the queried segments. For color 1, print 1, for color 2, print 2 ... etc. And this color sequence must be in ascending order.

### Sample Input
5 10  
P 1 2 3  
P 2 3 4  
Q 2 3  
Q 1 3  
P 3 5 4  
P 1 2 7  
Q 1 3  
Q 3 4  
P 5 5 8  
Q 1 5  
0 0  

### Sample Output
4  
3 4  
4 7  
4  
4 7 8  

### 问题分析

### Code
```cpp
#include<cstdio>
#include<string>
#include<algorithm>

#define lson root<<1
#define rson root<<1|1

using namespace std;

const int MAX = 1e6+5;
struct {
    int sum;
} tree[MAX<<2];
int lazy[MAX<<2];

void pushUp(int root) {
    tree[root].sum = tree[lson].sum | tree[rson].sum;
}

void build(int left, int right, int root) {
    lazy[root] = 0;
    if (left == right) {
        tree[root].sum = 2;
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, lson);
    build(mid+1, right, rson);
    pushUp(root);
}

void pushDown(int root) {
    if (lazy[root]) {
        lazy[lson] = lazy[rson] = lazy[root];
        tree[lson].sum = tree[rson].sum = 1<<(lazy[root]-1);
        lazy[root] = 0;
    }
}

void update(int ql, int qr, int c, int left, int right, int root) {
    if (ql <= left && qr >= right) {
        lazy[root] = c;
        tree[root].sum = 1<<(c-1);
        return;
    }
    pushDown(root);
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
    pushDown(root);
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
    int n, m;
    while (~scanf("%d %d", &n, &m)) {
        if (0 == n && 0 == m) {
            break;
        }
        build(1, n, 1);
        char op[2];
        while (m--) {
            scanf("%s", op);
            if ('P' == op[0]) {
                int a, b, c;
                scanf("%d %d %d", &a, &b, &c);
                update(a, b, c, 1, n, 1);
            } else if ('Q' == op[0]) {
                int a = 0, b = 0, ans = 0;
                scanf("%d %d", &a, &b);
                int res = query(a, b, 1, n, 1);
                int flag = 0;
                for (int i = 0; i < 30; i++) {
                    if (res & (1<<i)) {
                        printf("%s%d", flag==1 ? " " : "", i+1);
                        flag = 1;
                    }
                }
                printf("\n");
            }
        }
    }
    return 0;
}
```