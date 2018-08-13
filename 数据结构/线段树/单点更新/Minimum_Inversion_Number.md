## Minimum Inversion Number

### Description
The inversion number of a given number sequence a1, a2, ..., an is the number of pairs (ai, aj) that satisfy i < j and ai > aj. 

For a given sequence of numbers a1, a2, ..., an, if we move the first m >= 0 numbers to the end of the seqence, we will obtain another sequence. There are totally n such sequences as the following: 

a1, a2, ..., an-1, an (where m = 0 - the initial seqence) 
a2, a3, ..., an, a1 (where m = 1) 
a3, a4, ..., an, a1, a2 (where m = 2) 
... 
an, a1, a2, ..., an-1 (where m = n-1) 

You are asked to write a program to find the minimum inversion number out of the above sequences. 

### Input
The input consists of a number of test cases. Each case consists of two lines: the first line contains a positive integer n (n <= 5000); the next line contains a permutation of the n integers from 0 to n-1. 

### Output
For each case, output the minimum inversion number on a single line. 

### Sample Input
10  
1 3 6 9 0 8 5 7 4 2  

### Sample Output
16


### 逆序数
> 这道题在解题之前，需要先明白，什么是 **逆序数**
> 关于**逆序数**的定义，可以自行网上搜一下，这里只做简单说明

* 可以认为**逆序数**就是一对数的大小，与排列顺序相反的数  

##### 举个例子
如一组排列 [3, 4, 2, 1]，逆序数的个数为 5，分别是 [3, 2], [3, 1], [4, 2], [4, 1], [2, 1]
1. 我们认为排列的顺序是**从小到大**，所以，如果后面出现的数，比前面的数大，就认为出现了一个**逆序**。
1. 从 3 开始找，遇到的第一个数是 4，4 > 3，不算逆序，继续，2 < 3，出现了一个逆序，后面还有 1 < 3。
1. 重复上面的步骤，从 4 开始找，直到找完整个序列，把每一次查找的逆序数加起来，就是整个排列的逆序数。 

##### 逆序数的求法
1. 毫无疑问，我们可以采用枚举的方式，从头到尾，计算出所有的逆序数。这样的时间复杂度是 O(n²)，这种求法思想很简单，就不再赘述。
1. 通过**线段树**，或者**树状数组**来解决，这种方式的优势在于，时间复杂度会降到 O(nlogn)，如果理解了思想，只用套**线段树**的模板即可。

### 问题分析

### Code
```cpp
#include<cstdio>
#include<algorithm>

using namespace std;

const int MAX = 5000;
int tree[MAX<<2];
int input[MAX];

void pushUp(int root) {
    tree[root] =  tree[root*2+1] + tree[root*2+2];
}

void build(int left, int right, int root) {
    if (left == right) {
        tree[root] = 0;
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, root*2+1);
    build(mid+1, right, root*2+2);
    pushUp(root);
}

void update(int l, int c, int left, int right, int root) {
    if (left == right) {
        tree[root] = 1;
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

int query(int start, int end, int left, int right, int root) {
    if (start <= left && end >= right) {
        return tree[root];
    }
    int mid = (left + right) >> 1;
    int l = 0, r = 0;
    if (start <= mid) {
        l = query(start, end, left, mid, root*2+1);
    }
    if (end > mid) {
        r = query(start, end, mid+1, right, root*2+2);
    }
    return l + r;
}

int main() {
    int size = 0;
    while (scanf("%d", &size) != EOF) {
        int sum = 0;
        build(0, size-1, 0);
        for (int i = 0; i < size; i++) {
            scanf("%d", &input[i]);
            sum += query(input[i], size-1, 0, size-1, 0);
            update(input[i], 1, 0, size-1, 0);
        }
    
        int ans = sum;
        for (int i = 0; i < size; i++) {
            sum += size - input[i] - 1 - input[i];
            ans = min(ans, sum);
        }
        printf("%d\n", ans);
    }
}
```