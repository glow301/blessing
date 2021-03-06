## Buy Tickets

### Description
Railway tickets were difficult to buy around the Lunar New Year in China, so we must get up early and join a long queue…

The Lunar New Year was approaching, but unluckily the Little Cat still had schedules going here and there. Now, he had to travel by train to Mianyang, Sichuan Province for the winter camp selection of the national team of Olympiad in Informatics.

It was one o’clock a.m. and dark outside. Chill wind from the northwest did not scare off the people in the queue. The cold night gave the Little Cat a shiver. Why not find a problem to think about? That was none the less better than freezing to death!

People kept jumping the queue. Since it was too dark around, such moves would not be discovered even by the people adjacent to the queue-jumpers. “If every person in the queue is assigned an integral value and all the information about those who have jumped the queue and where they stand after queue-jumping is given, can I find out the final order of people in the queue?” Thought the Little Cat.

### Input
There will be several test cases in the input. Each test case consists of N + 1 lines where N (1 ≤ N ≤ 200,000) is given in the first line of the test case. The next N lines contain the pairs of values Posi and Vali in the increasing order of i (1 ≤ i ≤ N). For each i, the ranges and meanings of Posi and Vali are as follows:

* Posi ∈ [0, i − 1] — The i-th person came to the queue and stood right behind the Posi-th person in the queue. The booking office was considered the 0th person and the person at the front of the queue was considered the first person in the queue.
* Vali ∈ [0, 32767] — The i-th person was assigned the value Vali.
There no blank lines between test cases. Proceed to the end of input.

### Output
For each test cases, output a single line of space-separated integers which are the values of people in the order they stand in the queue.

### Sample Input
4  
0 77  
1 51  
1 33  
2 69  
4  
0 20523  
1 19243  
1 3890  
0 31492  

### Sample Output
77 33 69 51  
31492 20523 3890 19243  

### 问题分析
* 这个问题，和之前的奶牛排序的问题类似，整体思路也是从后向前，逐渐推出整个序列。
* 区别在于，输入的不再是一个单个的数字，而是一个结构体来维护，需要维护输入的**索引**和具体**值**。

### Tips 
这个题有一个需要注意的地方，就是，最后输出的时候，尽量只调用一次 printf，调用两次有可能超时。

### Code
```cpp
#include<cstdio>
#include<algorithm>

#define lson (root<<1)+1
#define rson (root<<1)+2

using namespace std;

const int MAX = 2e5+5;
struct {
    int pos, val;
} input[MAX];
struct {
    int sum;
} tree[MAX<<2];

void pushUp(int root) {
    tree[root].sum = tree[lson].sum + tree[rson].sum;
}

void build(int left, int right, int root) {
    if (left == right) {
        tree[root].sum = 1;
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, lson);
    build(mid+1, right, rson);
    pushUp(root);
}

int update(int left, int right, int root, int pos) {
    if (left == right) {
        tree[root].sum = 0;
        return left;
    }
    int mid = (left + right) >> 1;
    int ans = 0;
    if (pos <= tree[lson].sum) {
        ans = update(left, mid, lson, pos);
    } else {
        ans = update(mid+1, right, rson, pos-tree[lson].sum);
    }
    pushUp(root);
    return ans;
}

int main() {
    int size = 0;
    while (~scanf("%d", &size)) {
        int res[size];
        for (int i = 0; i < size; i++) {
            scanf("%d %d", &input[i].pos, &input[i].val);
        }
        build(0, size-1, 0);
        int idx = 0;
        for (int i = size-1; i >= 0; i--) {
            idx = update(0, size-1, 0, input[i].pos+1);
            res[idx] = input[i].val;
        }
        for (int i = 0; i < size; i++) {
            printf("%d%s", res[i], i == size-1 ? "\n" : " ");
        }
    }
}
```
