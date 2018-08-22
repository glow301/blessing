## Lost Cows

### Description
N (2 <= N <= 8,000) cows have unique brands in the range 1..N. In a spectacular display of poor judgment, they visited the neighborhood 'watering hole' and drank a few too many beers before dinner. When it was time to line up for their evening meal, they did not line up in the required ascending numerical order of their brands. 

Regrettably, FJ does not have a way to sort them. Furthermore, he's not very good at observing problems. Instead of writing down each cow's brand, he determined a rather silly statistic: For each cow in line, he knows the number of cows that precede that cow in line that do, in fact, have smaller brands than that cow. 

Given this data, tell FJ the exact ordering of the cows.

### Input
* Line 1: A single integer, N 

* Lines 2..N: These N-1 lines describe the number of cows that precede a given cow in line and have brands smaller than that cow. Of course, no cows precede the first cow in line, so she is not listed. Line 2 of the input describes the number of preceding cows whose brands are smaller than the cow in slot #2; line 3 describes the number of preceding cows whose brands are smaller than the cow in slot #3; and so on.

### Output
* Lines 1..N: Each of the N lines of output tells the brand of a cow in line. Line #1 of the output tells the brand of the first cow in line; line 2 tells the brand of the second cow; and so on.

### Sample Input
5  
1  
2  
1  
0  

### Sample Output
5  
1  
2  
1  
0  

### 问题分析

### 代码
```cpp
#include<cstdio>
#include<algorithm>

using namespace std;

const int MAX = 8005;
int input[MAX];
struct {
    int sum;
} tree[MAX<<2];

void pushUp(int root) {
    tree[root].sum = tree[root*2].sum + tree[root*2+1].sum;
}

void build(int left, int right, int root) {
    if (left == right) {
        tree[root].sum = 1;
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, root*2);
    build(mid+1, right, root*2+1);
    pushUp(root);
}

int update(int left, int right, int root, int key) {
    if (left == right) {
        tree[root].sum = 0;
        return left;
    }
    int mid = (left + right) >> 1;
    int ans = 0;
    if (key <= tree[root*2].sum) {
        ans = update(left, mid, root*2, key);
    } else {
        ans = update(mid+1, right, root*2+1, key - tree[root*2].sum);
    }
    pushUp(root);
    return ans;
}

int main() {
    int size = 0;
    while (~scanf("%d", &size)) {
        input[1] = 0;
        for (int i = 2; i <= size; i++) {
            scanf("%d", &input[i]);
        }
        build(1, size, 1);

        int ans[size];
        for (int i = size; i > 0; i--) {
            ans[i] = update(1, size, 1, input[i]+1);
        }

        for (int i = 1; i <= size; i++) {
            printf("%d\n", ans[i]);
        }
    }
}
```