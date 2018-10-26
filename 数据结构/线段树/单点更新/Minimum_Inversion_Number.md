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

* 使用线段树求解逆序数的步骤
> 假定输入序列长度是 N，输入数组为`input`，序列下标从 0 开始，用 i 表示。
1. 构建一个与输入序列个数相同的输入数组`input`, 线段树数组还是开 4 倍空间，并初始化数组为 0（对本题而言，由于输入序列都是连续的，不用考虑离散化的问题）
1. 依次读入每个数，计算 [input[i], N] 区间的和。
1. 把 input[i] 对应的数组下标置为 1（如 input[i] 为 3，就把数组中索引为 3 的改为 1）
1. 遍历完整个序列之后，所有和加起来即为整个序列的逆序数。

### 问题分析
* 对本题而言
输入序列为 [1, 3, 6, 9, 0, 8, 5, 7, 4, 2]
初始化数组为 [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

注意事项 **先查询，后更新**

| 第 i 次输入 | 输入 | 数组数据 | 当前逆序数 |说明 |
|--|--|--|--|--|
| - | - | [0, 1, 2, 3, 4, 5, 6, 7, 8, 9] | - | 为了方便查看，标出索引  |
| 1 | 1 | [0, 1, 0, 0, 0, 0, 0, 0, 0, 0] | 0 | 查询 [1, 9] 之间的和，然后更新索引 1 对应的元素为 1 |
| 2 | 3 | [0, 1, 0, 1, 0, 0, 0, 0, 0, 0] | 0 | 查询 [3, 9] 之间的和，然后更新索引 3 对应的元素为 1 |
| 3 | 6 | [0, 1, 0, 1, 0, 0, 1, 0, 0, 0] | 0 | 查询 [6, 9] 之间的和，然后更新索引 6 对应的元素为 1 |
| 4 | 9 | [0, 1, 0, 1, 0, 0, 1, 0, 0, 1] | 0 | 查询 [9, 9] 之间的和，然后更新索引 9 对应的元素为 1 |
| 5 | 0 | [1, 1, 0, 1, 0, 0, 1, 0, 0, 1] | 4 | 查询 [0, 9] 之间的和，然后更新索引 0 对应的元素为 1 |
| 6 | 8 | [1, 1, 0, 1, 0, 0, 1, 0, 1, 1] | 1 | 查询 [8, 9] 之间的和，然后更新索引 8 对应的元素为 1 |
| 7 | 5 | [1, 1, 0, 1, 0, 1, 1, 0, 1, 1] | 3 | 查询 [5, 9] 之间的和，然后更新索引 5 对应的元素为 1 |
| 8 | 7 | [1, 1, 0, 1, 0, 1, 1, 1, 1, 1] | 2 | 查询 [7, 9] 之间的和，然后更新索引 7 对应的元素为 1 |
| 9 | 4 | [1, 1, 0, 1, 1, 1, 1, 1, 1, 1] | 5 | 查询 [4, 9] 之间的和，然后更新索引 4 对应的元素为 1 |
| 10| 2 | [1, 1, 1, 1, 1, 1, 1, 1, 1, 1] | 7 | 查询 [2, 9] 之间的和，然后更新索引 2 对应的元素为 1 |

所以，这个序列的**逆序数**就是 22

* 求完了逆序数，还没有结束，题目要求求的是最小逆序数，我们还需要依次将数组的第一个元素放到最后。
* 通过观察可以发现，数组变化一次，逆序数和前一次有以下关系
    1. 第一个元素移动到队尾，那么，之前在**它后面比它大**的元素就由**非逆序变为逆序**了，**它后面比它小**的元素，就由**逆序变为非逆序**了（多读几遍，理解一下）
        * 比如 [2, 1, 3] 变成 [1, 3, 2]，2 后面比 2 大的 3，之前非逆序，现在就逆序了，比 2 小的 1，逆序变成非逆序了。
    1. 那么假设上一次的逆序数是 `sum`，序列长度是 N，变化后的序列逆序数 `sum1 = sum + N - input[i] - 1 - input[i]`（由于在第一次求出逆序数之后，序列重所有的位置都被置为了 1，所以可以直接用索引的加减来计算）
        * [1, 1, input[i], 1, 1, 1]，加上 input[i] 后面比 input[i] 大的（6 - 2 - 1），减去 ipnut[i] 前面比 input[i] 小的（2）。
        * 上面这个例子中，N = 6，input[i] = 2


### Code
#### C++ 版
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

#define lson root*2+1
#define rson root*2+2

using namespace std;

const int MAX = 5005;
struct {
    int sum;
} tree[MAX<<2];
int input[MAX];

void pushUp(int root) {
    tree[root].sum = tree[lson].sum + tree[rson].sum;
}

void build(int left, int right, int root) {
    if (left == right) {
        tree[root].sum = 0;
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, lson);
    build(mid+1, right, rson);
    pushUp(root);
}

void update(int l, int c, int left, int right, int root) {
    if (left == right) {
        tree[root].sum = 1;
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

int query(int ql, int qr, int left, int right, int root) {
    if (ql <= left && qr >= right) {
        return tree[root].sum;
    }
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
    while (~scanf("%d", &size)) {
        build(0, size-1, 0);
        int sum = 0;
        for (int i = 0; i < size; i++) {
            scanf("%d", &input[i]);
            sum += query(input[i], size-1, 0, size-1, 0);
            update(input[i], 1, 0, size-1, 0);
        }

        int ans = sum;
        for (int i = 0; i < size; i++) {
            sum += size - input[i] - 1 - input[i];
            ans = min(sum, ans);
        }
        printf("%d\n", ans);
    }
    return 0;
}
```

#### GO 版
```go
package main

import "fmt"

const MAX = 5000+5

type Tree struct {
    sum int
}

var (
    tree [MAX<<2]Tree
    input [MAX]int
)

func pushUp(root int) {
    tree[root].sum = tree[root*2].sum + tree[root*2+1].sum
}

func build(left, right, root int) {
    if left == right {
        tree[root].sum = 0
        return
    }
    mid := (left + right) >> 1
    build(left, mid, root*2)
    build(mid+1, right, root*2+1)
    pushUp(root)
}

func update(l, c, left, right, root int) {
    if left == right {
        tree[root].sum = 1
        return
    }
    mid := (left + right) >> 1
    if l <= mid {
        update(l, c, left, mid, root*2)
    } else {
        update(l, c, mid+1, right, root*2+1)
    }
    pushUp(root)
}

func query(ql, qr, left, right, root int) int {
    if ql <= left && qr >= right {
        return tree[root].sum
    }
    mid := (left + right) >> 1
    res := 0
    if ql <= mid {
        res += query(ql, qr, left, mid, root*2)
    }
    if qr > mid {
        res += query(ql, qr, mid+1, right, root*2+1)
    }
    return res
}

func main() {
    var num int
    for {
        if _, err := fmt.Scanf("%d\n", &num); err != nil {
            break
        }
        build(1, num, 1)
        sum := 0
        for i := 1; i <= num; i++ {
            fmt.Scanf("%d", &input[i])
            sum += query(input[i], num, 1, num, 1)
            update(input[i], 1, 1, num, 1)
        }

        res := sum
        for i := 1; i <= num; i++ {
            sum = sum + num - input[i] - input[i] - 1
            if sum < res {
                res = sum
            }
        }
        fmt.Println(res)
    }
}
```