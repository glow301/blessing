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

### Code
```cpp
#include<cstdio>
#include<algorithm>

using namespace std;

const int MAX = 2e5+5;
int input[MAX];
struct {
    int sum;
} tree[MAX<<2];

void pushUp(int root) {
    tree[root].sum = max(tree[root*2+1].sum, tree[root*2+2].sum);
}

void build(int left, int right, int root, int w) {
    if (left == right) {
        tree[root].sum = w;
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, root*2+1, w);
    build(mid+1, right, root*2+2, w);
    pushUp(root);
}

void update(int l, int c, int left, int right, int root) {
    if (left == right) {
        tree[root].sum -= c;
        return;
    }
    int mid = (left + right) >> 1;
    if (l <= mid) {
        update(l, c, left, mid, root*2+1);
    } else {
        update(l, c, mid+1, right, root*2+2);
    }
    pushUp(root);
}

int query(int left, int right, int root, int key) {
    if (key > tree[root].sum) {
        return -1;
    }

    if (left == right) {
        return left + 1;
    }

    int mid = (left + right) >> 1;
    if (key <= tree[root*2+1].sum) {
        return query(left, mid, root*2+1, key);
    } else {
        return query(mid+1, right, root*2+2, key);
    }
}

int main() {
    int h, w, n;
    while (~scanf("%d %d %d", &h, &w, &n)) {
        int num = 0;
        int ans = -1;
        build(0, min(n, h-1), 0, w);
        for (int i = 0; i < n; i++) {
            scanf("%d", &num);
            ans = query(0, min(n, h-1), 0, num);
            printf("%d\n", ans);
            if (ans > 0) {
                update(ans-1, num, 0, min(n, h-1), 0);
            }
        }
    }
    return 0;
}
```