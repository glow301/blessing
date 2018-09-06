## A Simple Problem with Integers 
> POJ - 3468

### Description
You have N integers, A1, A2, ... , AN. You need to deal with two kinds of operations. One type of operation is to add some given number to each number in a given interval. The other is to ask for the sum of numbers in a given interval.

### Input
The first line contains two numbers N and Q. 1 ≤ N,Q ≤ 100000.
The second line contains N numbers, the initial values of A1, A2, ... , AN. -1000000000 ≤ Ai ≤ 1000000000.
Each of the next Q lines represents an operation.
"C a b c" means adding c to each of Aa, Aa+1, ... , Ab. -10000 ≤ c ≤ 10000.
"Q a b" means querying the sum of Aa, Aa+1, ... , Ab.

### Output
You need to answer all Q commands in order. One answer in a line.

### Sample Input
10 5  
1 2 3 4 5 6 7 8 9 10  
Q 4 4  
Q 1 10  
Q 2 4  
C 3 6 3  
Q 2 4  

### Sample Output
4  
55  
9  
15  

### Hint
The sums may exceed the range of 32-bit integers.

### Code
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

#define lson root*2
#define rson root*2+1

using namespace std;

const int MAX = 1e5+5;
int input[MAX];
long long lazy[MAX<<2];
struct {
    long long sum;
} tree[MAX<<2];

void pushUp(int root) {
    tree[root].sum = tree[lson].sum + tree[rson].sum;
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

void  build(int left, int right, int root) {
    lazy[root] = 0;
    if (left == right) {
        tree[root].sum = input[left];
        return; 
    }
    int mid = (left + right) >> 1;
    build(left, mid, lson);
    build(mid+1, right, rson);
    pushUp(root);
}

void update(int ql, int qr, int c, int left, int right, int root) {
    if (ql <= left && qr >= right) {
        tree[root].sum += c * (right - left + 1);
        lazy[root] += c;
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

long long query(int ql, int qr, int left, int right, int root) {
    if (ql <= left && qr >= right) {
        return tree[root].sum;
    }
    pushDown(left, right, root);
    int mid = (left + right) >> 1;
    long long l = 0, r = 0;
    if (ql <= mid) {
        l += query(ql, qr, left, mid, lson);
    }
    if (qr > mid) {
        r += query(ql, qr, mid+1, right, rson);
    }
    return l + r;
}

int main() {
    int size, line;
    while (~scanf("%d %d", &size, &line)) {
        for (int i = 1; i <= size; i++) {
            scanf("%d", &input[i]);
        }
        build(1, size, 1);

        char action[2];
        int num1, num2, num3;
        while (line--) {
            scanf("%s", action);
            if (0 == strcmp(action, "Q")) {
                scanf("%d %d", &num1, &num2);
                printf("%lld\n", query(num1, num2, 1, size, 1));
            } else if (0 == strcmp(action, "C")) {
                scanf("%d %d %d", &num1, &num2, &num3);
                update(num1, num2, num3, 1, size, 1);
            }
        }
    }
    return 0;
}
```