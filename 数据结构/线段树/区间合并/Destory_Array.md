### Descrption
给定一个有n个数的序列a1,a2, ..., an  
你每次可以将序列中一个数删去，剩下的数会被分割为一段一段连续的数列  
给定一个删除数的顺序，问在每次删除之后，剩下的连续的数列中，数列和的最大值为多少  

### Input
第一行输入仅有一个正整数n(1<=n<=100000)  
第二行包括n个整数a1,a2, ..., an(0<=ai<=1,000,000,000)  
第三行为n个正整数的一个全排列，为删除元素的顺序。  

### Output
输出n行  
第i行输出一个整数，表示执行到第i个删除操作时，当前的连续数列的和的最大值。  

### Example
Input  
4  
1 3 2 5  
3 4 1 2 

Output  
5  
4  
3  
0  

Input  
5  
1 2 3 4 5  
4 2 3 5 1  

Output  
6  
5  
5  
1  
0  

Input  
8  
5 5 4 4 6 6 5 5  
5 2 8 7 1 3 4 6  

Output  
18  
16  
11  
8  
8  
6  
6  
0  

### 问题分析
这个题属于线段树**区间合并**的问题，问题的关键就在于 pushUp 这个函数
与单点更新问题区别在于
1. 每一个节点需要保存 4 个信息
    1. 区间左起最大连续和（简称左和）。
    1. 区间右起最大连续和（简称右和）。
    1. 当前区间最大连续和（**左和**，**右和**，**左子树右和+右子树左和**，这三个值的最大值）。
    1. 当前区间是否为满（区间不包含 0 视为满）。
1. pushUp 每次更新，自下而上求出整个区间的最大连续和，所以这个题没有 query 函数，直接返回根结点的 sum 就是答案。
1. 注意事项
    1. 建树时，默认所有节点为满，因为初始化时，没有 0。
    1. pushUp 操作时，需要注意几点
        1. 根结点的**左和**默认等于**左子树的左和**，但是当左子树满的时候，左和要再加上右子树的左和（相当于左和向右延伸）。
        1. 根结点的右和同上。
        1. 根节点的**最大连续和**，等于 **左子树最大连续和**，**右子树连续最大和**，**左子树右和+右子树左和**，三者的最大值。
        1. 左子树和右子树中，有任意一个不满，根结点不满

### Code
```cpp
#include<cstdio>
#include<algorithm>

#define lson root*2+1
#define rson root*2+2

using namespace std;

const int MAX = 1e5;
long long input[MAX];
struct {
    long long sum, lsum, rsum;
    bool full;
} tree[MAX<<2];

void pushUp(int root) {
    tree[root].lsum = tree[lson].lsum;
    tree[root].rsum = tree[rson].rsum;
    tree[root].sum  = max(tree[lson].sum, tree[rson].sum);

    if (tree[lson].full) {
        tree[root].lsum += tree[rson].lsum;
    }
    if (tree[rson].full) {
        tree[root].rsum += tree[lson].rsum;
    }
    if (!tree[lson].full || !tree[rson].full) {
        tree[root].full = false;
    }
    tree[root].sum = max(tree[root].sum, tree[lson].rsum + tree[rson].lsum);
}

void build(int left, int right, int root) {
    tree[root].full = true;
    if (left == right) {
        tree[root].sum = tree[root].lsum = tree[root].rsum = input[left];
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, lson);
    build(mid+1, right, rson);
    pushUp(root);
}

void update(int l, int c, int left, int right, int root) {
    if (left == right) {
        tree[root].sum = tree[root].lsum = tree[root].rsum = 0;
        tree[root].full = false;
        return;
    }
    int mid = (left + right) >> 1;
    if (l <= mid) {
        update(l, c, left, mid, lson);
    } else {
        update(l, c, mid+1, right, rson);
    }
    pushUp(root);
}

int main() {
    int size = 0;
    scanf("%d", &size);
    for (int i = 0; i < size; i++) {
        scanf("%d", &input[i]);
    }
    build(0, size-1, 0);

    int num = 0;
    for (int i = 0; i < size; i++) {
        scanf("%d", &num);
        update(num-1, 0, 0, size-1, 0);
        printf("%lld\n", tree[0].sum);
    }
    return 0;
}
```
