## Color the ball
> HDU 1556

### Description
N个气球排成一排，从左到右依次编号为1,2,3....N.每次给定2个整数a b(a <= b),lele便为骑上他的“小飞鸽"牌电动车从气球a开始到气球b依次给每个气球涂一次颜色。但是N次以后lele已经忘记了第I个气球已经涂过几次颜色了，你能帮他算出每个气球被涂过几次颜色吗？

### Input
每个测试实例第一行为一个整数N,(N <= 100000).接下来的N行，每行包括2个整数a b(1 <= a <= b <= N)。 
当N = 0，输入结束。

### Output
每个测试实例输出一行，包括N个整数，第I个数代表第I个气球总共被涂色的次数。

### Sample Input
3  
1 1  
2 2  
3 3  
3  
1 1  
1 2  
1 3  
0 

### Sample Output
1 1 1  
3 2 1  

### 问题分析
### Code
```cpp
#include<cstring>
#include<cstdio>
#include<algorithm>

#define lson root<<1
#define rson root<<1|1

using namespace std;

const int MAX = 1e5+5;
struct {
    int sum;
} tree[MAX<<2];
int lazy[MAX<<2];

void pushUp(int root) {
    tree[root].sum = tree[lson].sum + tree[rson].sum;
}

void build(int left, int right, int root) {
    lazy[root] = 0;
    if (left == right) {
        tree[root].sum = 0;
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, lson);
    build(mid+1, right, rson);
    pushUp(root);
}

void pushDown(int left, int right, int root) {
    if (lazy[root]) {
        int mid = (left + right) >> 1;
        lazy[lson] += lazy[root];
        lazy[rson] += lazy[root];
        tree[lson].sum += lazy[root] * (mid - left + 1);
        tree[rson].sum += lazy[root] * (right - mid);
        lazy[root] = 0;
    }
}

void update(int ql, int qr, int c, int left, int right, int root) {
    if (ql <= left && qr >= right) {
        lazy[root] += c;
        tree[root].sum += c * (right - left + 1);
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
    int res = 0;
    if (ql <= mid) {
        res += query(ql, qr, left, mid, lson);
    }
    if (qr > mid) {
        res += query(ql, qr, mid+1, right, rson);
    }
    return res;
}

int main() {
    int size;
    while(~scanf("%d", &size)) {
        if (0 == size) {
            break;
        }
        build(1, size, 1);
        int num1, num2;
        for (int i = 1; i <= size; i++) {
            scanf("%d %d", &num1, &num2);
            update(num1, num2, 1, 1, size, 1);
        }
        int res = 0;
        for (int i = 1; i <= size; i++) {
            res = query(i, i, 1, size, 1);
            printf("%d%s", res, i==size ? "\n" : " ");
        }
    }
}
```

* 这个题还可以用数组来解决
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

#define lson root<<1
#define rson root<<1|1

using namespace std;

int arr[100005] = {0};

int main() {
    int size;
    while (~scanf("%d", &size)) {
        if(0 == size) {
            break;
        }
        memset(arr, 0, sizeof(arr));
        int num1, num2;
        for (int i = 0; i < size; i++) {
            scanf("%d %d", &num1, &num2);
            arr[num1]++;
            arr[num2+1]--;
        }
        int sum = 0;
        for (int i = 1; i <= size; i++) {
            sum += arr[i];
            printf("%d%s", sum, i==size ? "\n" : " ");
        }
    }
    return 0;
}
```