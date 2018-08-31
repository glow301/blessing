## Billbord

### Description
每年开学，正是各大社团招新之际。
 
每个社团为了吸引更多的小学妹小学弟就会派出身强体壮的华师男去海报墙上粘贴海报。
 
开学之初，高为h，宽为w的海报墙还是空的。
 
然后，华师男轮流粘贴高为1，宽为wi的海报。

贴海报时，机智的华师男总是会优先选择最上面的位置来帖，而且在所有最上面的可能位置中，他会选择最左面的位置。但是不能把已经贴好的海报盖住并且不能超出海报墙的范围。
 
机智的华师男能够自然能够秒秒钟得出自己的海报应该粘贴在第几行啦。

### Input
有多组样例，但是不会超过40个。
 
对于每组样例，第一行包含3个整数h，w，n(1 <= h,w <= 10^9; 1 <= n <= 200,000) -海报墙的高度和宽度，以及海报的张数。 

下面n行每行一个整数 wi (1 <= wi <= 10^9) -  代表第i张海报的宽度.

### Output
对于每张海报，输出它被贴在第几行，顶部是第一行。如果某广告不能贴不下了，则输出-1。

### Sample Input
3 5 5  
2  
4  
3  
3  
3  

### Sample Output
1  
2  
1  
3  
-1  

### 问题分析
1. 将每一行都看成一个节点（节点数量，要取**海报数量**和**墙高度**的**最小值**，因为可能墙很高，但是没几张海报，导致数组越界），每个节点上维护**剩余可用长度**。
1. pushUp 的时候，将可用最大剩余长度 pushUp 上去。根节点就是当前整个最大可用的长度。
1. 每次查询的时候，判断输入长度，是否超过整个数组可用长度，如果超过，直接返回 -1。
1. 更新数据时，将当前节点的可用长度，减去输入的数值，再把结果 pushUp 上去。

### Code
```cpp
#include<cstdio>
#include<algorithm>

#define lson root*2+1
#define rson root*2+2

using namespace std;

const int MAX = 2e5;
int input[MAX];
struct {
    int len;
} tree[MAX<<2];

void pushUp(int root) {
    tree[root].len = max(tree[lson].len, tree[rson].len);
}

void build(int left, int right, int root, int w) {
    if (left == right) {
        tree[root].len = w;
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, lson, w);
    build(mid+1, right, rson, w);
    pushUp(root);
}

void update(int l, int c, int left, int right, int root) {
    if (left == right) {
        tree[root].len -= c;
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

int query(int left, int right, int root, int w) {
    if (w > tree[root].len) {
        return -1;
    }
    if (left == right) {
        return left+1;
    }
    int mid = (left + right) >> 1;
    if (w <= tree[lson].len) {
        return query(left, mid, lson, w);
    } else {
        return query(mid+1, right, rson, w);
    }
}

int main() {
    int h, w, n;
    while (~scanf("%d %d %d", &h, &w, &n)) {
        int size = min(h, n);
        build(0, size-1, 0, w);
        int num = 0, res = 0;
        while (n--) {
            scanf("%d", &num);
            res = query(0, size-1, 0, num);
            if (res >= 0) {
                update(res-1, num, 0, size-1, 0);
            }
            printf("%d\n", res);
        }
    }
    return 0;
}
```