## Longest Ordered Subsequence 
> POJ - 2533

### Description
给出一个序列，求出这个序列的最长上升子序列。

序列A的上升子序列B定义如下：
1. B为A的子序列
1. B为严格递增序列

### Input
第一行包含一个整数n，表示给出序列的元素个数。

第二行包含n个整数，代表这个序列。
1 <= N <= 1000

### Output
输出给出序列的最长子序列的长度。

### Simple Input
7  
1 7 3 5 9 4 8  

### Simple Output
4  

### Code
##### C++
```cpp
#include<cstdio>
#include<algorithm>

using namespace std;

int main() {
    int n;
    while (~scanf("%d", &n)) {
        int arr[n];
        int dp[n];

        for (int i = 0; i < n; i++) {
            scanf("%d", &arr[i]);
        }

        int ans = 0;
        for (int i = 0; i < n; i++) {
            dp[i] = 1;
            for (int j = 0; j < i; j++) {
                if (arr[j] < arr[i]) {
                    dp[i] = max(dp[i], dp[j]+1);
                }
            }
            ans = max(ans, dp[i]);
        }
        printf("%d\n", ans);
    }
    return 0;
}
```
##### GO
```go
package main

import "fmt"

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func main() {
    n := 0
    for {
        _, err := fmt.Scanf("%d\n", &n)
        if nil != err {
            break
        }
        input := make([]int, n)
        dp    := make([]int, n)
        for i := 0; i < n; i++ {
            fmt.Scanf("%d", &input[i])
        }

        ans := 0
        for i := 0; i < n; i++ {
            dp[i] = 1
            for j := 0; j < i; j++ {
                if input[j] < input[i] {
                    dp[i] = max(dp[i], dp[j]+1)
                }
            }
            ans = max(ans, dp[i])
        }
        fmt.Println(ans)
    }
}
```