## Hotel
> POJ - 3667

### Description
火山经营着一个停车场，假设的停车场有N车位（编号1-N）都在一条线上，最初所有车位都没停车。经常有人来定车位，
他要定连续的k(1 ≤ k ≤ N)个车位。火山要确定是否能够满足客人的要求，如果能，他要将这k个连续的车位安排在编
号最小的地方停下。若不能，则客人不停在火山的停车场。在某一时间，有些车会被车主开走了。火山的停车场很大，
火山想让学弟学妹写个程序帮助他。

### Input
第1行输入 N 和 M。N是车位个数，M是（停车和开走车操作的总次数）
接下来M行，先输入操作类型（1或2）
若是1，表示有人来停车，再输入k
若是2，再输入l,r, 表示区间[l,l+r) 的车被开走了。
N (1 ≤ N ≤ 50,000)      M (1 ≤ M < 50,000)

### Output
当输入为1时，若火山的停车场有连续的k个车位，那么输出第一辆车停的最小的编号，否则输出0。

### Sample Input
10 6  
1 3  
1 3  
1 3  
1 3  
2 5 5  
1 6  

### Sample Output
1  
4  
7  
0  
5  

### 问题分析
### 错误记录
1. `pushUp` 和 `pushDown` 没有更新 `lsum` 和 `rsum`
1. `query`函数中，查询跨端点区间的时候， 没有 `return mid-lson.rsum+1`，写成了 `return query(left, mid, rson, key)`
1. `update` 的时候，`sum` 固定写成了 `c`, 应该是`(right - left) * c`,
1. `pushUp` 判断条件写错，应该是判断**当前节点**的最左/最有连续和
1. lazy 标记用错，对于此题 lazy 有可能为 0 ，所以不能使用 0 作为判断条件，是否 pushDown。

### Code
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

#define lson root<<1
#define rson root<<1|1

using namespace std;

const int MAX = 5e4+5;
struct {
    int lsum, rsum, sum;
} tree[MAX<<2];
int lazy[MAX<<2];

void pushUp(int left, int right, int root) {
    tree[root].lsum = tree[lson].lsum;
    tree[root].rsum = tree[rson].rsum;
    tree[root].sum  = max(tree[lson].sum, tree[rson].sum);

    int mid = (left + right) >> 1;
    if (tree[root].lsum == mid - left + 1) {
        tree[root].lsum += tree[rson].lsum;
    }
    if (tree[root].rsum == right - mid) {
        tree[root].rsum += tree[lson].rsum;
    }
    tree[root].sum = max(tree[root].sum, tree[lson].rsum + tree[rson].lsum);
}

void pushDown(int left, int right, int root) {
    if (lazy[root] != -1) {
        int mid = (right + left) >> 1;
        lazy[lson] = lazy[rson] = lazy[root];
        tree[lson].sum = tree[lson].lsum = tree[lson].rsum = lazy[root] * (mid - left + 1);
        tree[rson].sum = tree[rson].lsum = tree[rson].rsum = lazy[root] * (right - mid);
        lazy[root] = -1;
    }
}

void build(int left, int right, int root) {
    lazy[root] = -1;
    if (left == right) {
        tree[root].lsum = tree[root].rsum = tree[root].sum = 1;
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, lson);
    build(mid+1, right, rson);
    pushUp(left, right, root);
}

void update(int ql, int qr, int c, int left, int right, int root) {
    if (ql <= left && qr >= right) {
        lazy[root] = c;
        tree[root].sum = tree[root].lsum = tree[root].rsum = (right - left + 1) * c;
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
    pushUp(left, right, root);
}

int query(int left, int right, int root, int key) {
    if (tree[1].sum < key) {
        return 0;
    }
    if (left == right) {
        return left;
    }
    pushDown(left, right, root);
    int mid = (left + right) >> 1;
    if (tree[lson].sum >= key) {
        return query(left, mid, lson, key);
    }
    if (tree[lson].rsum + tree[rson].lsum >= key) {
        return mid - tree[lson].rsum + 1;
    } else {
        return query(mid+1, right, rson, key);
    }
}

int main() {
    int n, m;
    while (~scanf("%d %d", &n, &m)) {
        build(1, n, 1);
        while (m--) {
            int op, num1, num2;
            scanf("%d", &op);
            if (1 == op) {
                scanf("%d", &num1);
                int res = query(1, n, 1, num1);
                if (res != 0) {
                    update(res, res+num1-1, 0, 1, n, 1);
                }
                printf("%d\n", res);
            } else if (2 == op) {
                scanf("%d %d", &num1, &num2);
                update(num1, num1+num2-1, 1, 1, n, 1);
            }
        }
    }
}
```