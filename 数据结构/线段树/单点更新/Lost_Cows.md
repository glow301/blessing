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
2  
4  
5  
3  
1  

### 问题分析
#### 思路
* 这个题可以看成是给奶牛排队，每一个奶牛前面有多少个比它序号小的，可以理解为把奶牛插入到队列的指定位置。那么，最后一个插队的奶牛，就有最高的优先级，它插入的位置，一定就是在队列中最终的位置。
* 那么就可以将序列倒叙输入，逐步计算出完整的序列。

1. 排在第一位的，它前面一定没有其他奶牛了，所以，默认第一可以认为前面的奶牛数量为 0。
1. 假设输入序列为 [1, 2, 1, 0], 将序列倒叙输入
    1. 第一次输入 0，表示在它前面，比它小的数为 0，那就是说，现在需要找到当前序列中**最小的元素**，即为 1 （总共 5 个数，[1, 2, 3, 4, 5]），这时需要把 `1` 删掉，因为`1`已经插入了，这时序列中的元素为[2, 3, 4, 5]
    1. 第二次输入 1，表示在它前面，比它小的数为 1，即求当前序列中**第2小的数**（前面有一个比自己小的，自己的序号就是 2 ），结果为 3，然后删掉 3，这时序列为 [2, 4, 5]
    1. 第三次输入 2，表示前面有 2 个比它小的，求当前第 3 小的数，结果为 5 ，然后删掉 5，序列变为 [2, 4]
    1. 依次类推...

### Tips
* 线段树维护数据
    1. 节点初始化都为 1，那么每个节点的索引，即表示当前元素是第 k 大。
    1. 每次删除元素时，将当前元素置为 0。
    1. 当第 k 大的元素落在**右子树**时，第 k 大要变成 k-tree[lson] sum 大。

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
