## I Hate It 

### Description
很多学校流行一种比较的习惯。老师们很喜欢询问，从某某到某某当中，分数最高的是多少。 
这让很多学生很反感。 

不管你喜不喜欢，现在需要你做的是，就是按照老师的要求，写一个程序，模拟老师的询问。当然，老师有时候需要更新某位同学的成绩。

### Input
本题目包含多组测试，请处理到文件结束。    
在每个测试的第一行，有两个正整数 N 和 M ( 0<N<=200000,0<M<5000 )，分别代表学生的数目和操作的数目。  
学生ID编号分别从1编到N。  
第二行包含N个整数，代表这N个学生的初始成绩，其中第i个数代表ID为i的学生的成绩。   
接下来有M行。每一行有一个字符 C (只取'Q'或'U') ，和两个正整数A，B。   
当C为'Q'的时候，表示这是一条询问操作，它询问ID从A到B(包括A,B)的学生当中，成绩最高的是多少。   
当C为'U'的时候，表示这是一条更新操作，要求把ID为A的学生的成绩更改为B。   

### Output
对于每一次询问操作，在一行里面输出最高成绩。

### Sample Input
5 6  
1 2 3 4 5  
Q 1 5  
U 3 6  
Q 3 4  
Q 4 5  
U 2 9  
Q 1 5  

### Sample Output
5  
6  
5  
9  

### 问题分析
对于此题，只涉及单点更新，每次在 pushUp 的时候，求出**左孩子**和**右孩子**两者最大值，更新到 root 即可。
其他直接套用线段树模板即可。

### Code
#### java 版
```java
import java.util.Scanner;

public class Main {
    private static final int MAX = 200000;
    private static int[] sum     = new int[MAX<<2];
    private static int[] arr     = new int[MAX];

    private static void pushUp(int root) {
        int left  = sum[root*2+1];
        int right = sum[root*2+2];
        sum[root] = left > right ? left : right;
    }

    private static void build(int left, int right, int root) {
        if (left == right) {
            sum[root] = arr[left];
            return;
        }
        int mid = (left + right) >> 1;
        build(left, mid, root*2+1);
        build(mid+1, right, root*2+2);
        pushUp(root);
    }

    private static void update(int l, int c, int left, int right, int root) {
        if (left == right) {
            sum[root] = c;
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

    private static int query(int start, int end, int left, int right, int root) {
        int leftMax  = 0;
        int rightMax = 0;
        if (start <= left && end >= right) {
            return sum[root];
        }
        int mid   = (left + right) >> 1;
        if (start <= mid) {
            leftMax =  query(start, end, left, mid, root*2+1);
        }
        if (end > mid) {
            rightMax = query(start, end, mid+1, right, root*2+2);
        }
        return leftMax > rightMax ? leftMax : rightMax;
    }

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        while (scan.hasNext()) {
            int stuNum = scan.nextInt();
            int opNum  = scan.nextInt();
            int index  = 0;
            while (stuNum-- > 0) {
                arr[index] = scan.nextInt();
                index++;
            }
            build(0, index-1, 0);
            while (opNum-- > 0) {
                String op = scan.next();
                int op1   = scan.nextInt();
                int op2   = scan.nextInt();
                if (op.equals("Q")) {
                    System.out.println(query(op1 - 1, op2 - 1, 0, index-1, 0));
                } else if (op.equals("U")) {
                    update(op1 - 1, op2, 0, index-1, 0);
                }
            }
        }
    }
}
```

#### c++ 版
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

#define lson root<<1
#define rson root<<1|1

using namespace std;

const int MAX = 2e5+5;
struct {
    int sum;
} tree[MAX<<2];
int input[MAX];

void pushUp(int root) {
    tree[root].sum = max(tree[lson].sum, tree[rson].sum);
}

void build(int left, int right, int root) {
    if (left == right) {
        tree[root].sum = input[left];
        return;
    }
    int mid = (left + right) >> 1;
    build(left, mid, lson);
    build(mid+1, right, rson);
    pushUp(root);
}

void update(int l, int c, int left, int right, int root) {
    if (left == right) {
        tree[root].sum = c;
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
    int l = 0, r = 0;
    if (ql <= mid) {
        l = query(ql, qr, left, mid, lson);
    }
    if (qr > mid) {
        r = query(ql, qr, mid+1, right, rson);
    }
    return max(l, r);
}

int main() {
    int size, line;
    while(~scanf("%d %d", &size, &line)) {
        for (int i = 1; i <= size; i++) {
            scanf("%d", &input[i]);
        }
        build(1, size, 1);
        while (line--) {
            char op[2];
            int x, y;
            scanf("%s %d %d", op, &x, &y);
            if ('Q' == op[0]) {
                printf("%d\n", query(x, y, 1, size, 1));
            } else {
                update(x, y, 1, size, 1);
            }
        }
    }
    return 0;
}
```

#### Go 语言版本
```go
package main

import "fmt"

const MAX = 2e5+5

type Tree struct {
    sum int
}

var (
    tree [MAX<<2]Tree
    input [MAX]int
)

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func pushUp(root int) {
    tree[root].sum = max(tree[root<<1].sum, tree[root<<1|1].sum)
}

func build(left, right, root int) {
    if left == right {
        tree[root].sum = input[left]
        return
    }
    mid := (left + right) >> 1
    build(left, mid, root<<1)
    build(mid+1, right, root<<1|1)
    pushUp(root)
}

func update(l, c, left, right, root int) {
    if left == right {
        tree[root].sum = c
        return;
    }
    mid := (left + right) >> 1
    if l <= mid {
        update(l, c, left, mid, root<<1)
    } else {
        update(l, c, mid+1, right, root<<1|1)
    }
    pushUp(root)
}

func query(ql, qr, left, right, root int) int {
    if ql <= left && qr >= right {
        return tree[root].sum
    }
    mid := (left + right) >> 1
    l, r := 0, 0
    if ql <= mid {
        l = query(ql, qr, left, mid, root<<1)
    }
    if qr > mid {
        r = query(ql, qr, mid+1, right, root<<1|1)
    }
    return max(l, r)
}

func main() {
    var n, m int
    for {
        if _, err := fmt.Scanf("%d %d", &n, &m); nil != err {
            break
        }
        var num int
        for i := 1; i <= n; i++ {
            fmt.Scanf("%d", &num)
            input[i] = num
        }
        build(1, n, 1)

        op, x, y := "", 0, 0
        for i := 0; i < m; i++ {
            fmt.Scanf("%s %d %d", &op, &x, &y)
            if "Q" == op {
                fmt.Println(query(x, y, 1, n, 1))
            } else {
              update(x, y, 1, n, 1)
            }
        }
    }
}
```