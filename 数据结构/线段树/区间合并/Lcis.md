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

### code
```cpp
#include<cstdio>
#include<algorithm>

using namespace std;

const int MAX = 100000;

struct{
    int lsum, rsum, sum, l, r;
} tree[MAX<<2];

int input[MAX];

void pushUp(int root, int left, int right) {
    tree[root].l = tree[root*2+1].l;
    tree[root].r = tree[root*2+2].r;
    tree[root].lsum = tree[root*2+1].lsum;
    tree[root].rsum = tree[root*2+2].rsum;

    tree[root].sum = max(tree[root*2+1].sum, tree[root*2+2].sum);

    int mid = (left + right) >> 1;
    // 左子树右端点 小于 右子树左端点
    if (tree[root*2+1].r < tree[root*2+2].l) {
        // 区间长度 等于 连续长度，延伸左连续长度至右边
        if (mid - left + 1 == tree[root*2+1].sum) {
            tree[root].lsum += tree[root*2+2].lsum;
        }
        // 区间长度 等于 连续长度，连续右连续和到左边
        if (right - mid == tree[root*2+2].sum) {
            tree[root].rsum += tree[root*2+1].rsum;
        }
        tree[root].sum = max(tree[root].sum, tree[root*2+1].rsum + tree[root*2+2].lsum);
    }
}

void build(int left, int right, int root) {
    if (left == right) {
        tree[root].lsum = tree[root].rsum = tree[root].sum = 1;
        tree[root].l = tree[root].r = input[left];
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, root*2+1);
    build(mid+1, right, root*2+2);
    pushUp(root, left ,right);
}

void update(int l, int c, int left, int right, int root) {
    if (left == right) {
        tree[root].l = tree[root].r = c;
        return;
    }
    int mid = (left + right) >> 1;
    if (l <= mid) {
        update(l, c, left, mid, root*2+1);
    } else {
        update(l, c, mid+1, right, root*2+2);
    }
    pushUp(root, left, right);
}

int query(int start, int end, int left, int right, int root) {
    if (start <= left && end >= right) {
        return tree[root].sum;
    }
    int mid = (left + right) >> 1;
    int l = 0, r = 0;
    if (start <= mid) {
        l = query(start, end, left, mid, root*2+1);
    }
    if (end > mid) {
        r = query(start, end, mid+1, right, root*2+2);
    }
    int ans = max(l, r);
    if (tree[root*2+1].r < tree[root*2+2].l) {
        // 连续和要在，区间长度，和连续和之间取最小值，防止连续长度超过区间查找长度
        int llcis = min(mid-start+1, tree[root*2+1].rsum);
        int rlcis = min(end-mid, tree[root*2+2].lsum);
        return max(llcis + rlcis, ans);
    }
    return ans;
}

int main() {
    int line = 0;
    scanf("%d", &line);
    while (line--) {
        int size, ops;
        scanf("%d %d", &size, &ops);
        for (int i = 0; i < size; i++) {
            scanf("%d", &input[i]);
        }
        build(0, size, 0);

        char op[3];
        int op1, op2;
        for (int i = 0; i < ops; i++) {
            scanf("%s %d %d", op, &op1, &op2);
            if('Q' == op[0]) {
                printf("%d\n", query(op1, op2, 0, size, 0));
            }
            if ('U' == op[0]) {
                update(op1, op2, 0 ,size, 0);
            }
        }
    }
    return 0;
}
```