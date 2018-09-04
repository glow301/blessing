## Just a Hook

### Description
In the game of DotA, Pudge’s meat hook is actually the most horrible thing for most of the heroes. The hook is made up of several consecutive metallic sticks which are of the same length. 

Now Pudge wants to do some operations on the hook. 

Let us number the consecutive metallic sticks of the hook from 1 to N. For each operation, Pudge can change the consecutive metallic sticks, numbered from X to Y, into cupreous sticks, silver sticks or golden sticks. 
The total value of the hook is calculated as the sum of values of N metallic sticks. More precisely, the value for each kind of stick is calculated as follows: 

For each cupreous stick, the value is 1. 
For each silver stick, the value is 2. 
For each golden stick, the value is 3. 

Pudge wants to know the total value of the hook after performing the operations. 
You may consider the original hook is made up of cupreous sticks. 

### Input
The input consists of several test cases. The first line of the input is the number of the cases. There are no more than 10 cases. 
For each case, the first line contains an integer N, 1<=N<=100,000, which is the number of the sticks of Pudge’s meat hook and the second line contains an integer Q, 0<=Q<=100,000, which is the number of the operations. 
Next Q lines, each line contains three integers X, Y, 1<=X<=Y<=N, Z, 1<=Z<=3, which defines an operation: change the sticks numbered from X to Y into the metal kind Z, where Z=1 represents the cupreous kind, Z=2 represents the silver kind and Z=3 represents the golden kind. 

### Output
For each case, print a number in a line representing the total value of the hook after the operations. Use the format in the example. 

### Sample Input
1  
10  
2  
1 5 2  
5 9 3  

### Sample Output
Case 1: The total value of the hook is 24. 

### Code
```cpp
#include<cstdio>
#include<algorithm>
#include<cstring>

#define lson root*2
#define rson root*2+1

using namespace std;

const int MAX = 1e5+5;
int lazy[MAX<<2];
struct {
    int sum;
} tree[MAX<<2];


void pushUp(int root) {
    tree[root].sum = tree[lson].sum + tree[rson].sum;
}

void pushDown(int left, int right, int root) {
    if (lazy[root]) {
        int mid = (left + right) >> 1;
        lazy[lson] = lazy[rson] = lazy[root];
        tree[lson].sum = lazy[root] * (mid - left + 1);
        tree[rson].sum = lazy[root] * (right - mid);
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
        tree[root].sum = c * (right - left + 1);
        lazy[root] = c;
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

int main() {
    int cases;
    scanf("%d", &cases);
    for (int i = 1; i <= cases; i++) {
        int size, line;
        scanf("%d", &size);
        scanf("%d", &line);
        build(1, size, 1);

        int x, y, z;
        while (line--) {
            scanf("%d %d %d", &x, &y, &z);
            update(x, y, z, 1, size, 1);
        }
        printf("Case %d: The total value of the hook is %d.\n", i, tree[1].sum);
    }
    return 0;
}
```