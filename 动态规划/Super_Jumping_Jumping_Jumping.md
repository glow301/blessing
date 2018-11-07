## Super Jumping! Jumping! Jumping! 
> HDU - 1087

### Description
wsw获得了与小姐姐约会的机会，同时也不用担心wls会发现了，可是如何选择和哪些小姐姐约会呢？wsw希望自己可以循序渐进，同时希望挑战自己的极限，我们假定每个小姐姐有一个“攻略难度值”
从攻略成功第一个小姐姐开始，wsw希望每下一个需要攻略的小姐姐难度更高，同时又希望攻略难度值之和最大，好了，现在小姐姐们排成一排，wsw只能从左往右开始攻略，请你帮助他找到最大的攻略难度和

### Input
多组输入，每组数据占一行，每行一个整数n表示小姐姐个数，接着n个数a_1, a_2, ..., a_n表示第i个的小姐姐攻略难度 (a_i在32位有符号整型范围内)，n = 0表示输入结束 (0 <= n <= 1000)。

### Output
多组输入，每组数据占一行，每行一个整数n表示小姐姐个数，接着n个数a_1, a_2, ..., a_n表示第i个的小姐姐攻略难度 (a_i在32位有符号整型范围内)，n = 0表示输入结束 (0 <= n <= 1000)。

### Simple Input
3 1 3 2  
4 1 2 3 4  
4 3 3 2 1  
0  

### Simple Output
4  
10  
3  

### Code
##### C++
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>

using namespace std;

int main() {
    int n;
    while (~scanf("%d", &n)) {
        if (0 == n) {
            break;
        }
        int dp[n];
        int arr[n];
        memset(dp, 0, sizeof(dp));
        for (int i = 0; i < n; i++) {
            scanf("%d", &arr[i]);
        }

        int ans = 0;
        for (int i = 0; i < n; i++) {
            dp[i] = arr[i];
            for (int j = 0; j < i; j++) {
                if (arr[j] < arr[i]) {
                    dp[i] = max(dp[i], dp[j] + arr[i]);
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
    for {
        n := 0
        fmt.Scanf("%d", &n)
        if 0 == n {
            break
        }

        arr := make([]int, n)
        dp  := make([]int, n)
        for i := 0; i < n; i++ {
            fmt.Scanf("%d", &arr[i])
        }

        ans := 0
        for i := 0; i < n; i++ {
            dp[i] = arr[i]
            for j := 0; j < i; j++ {
                if arr[j] < arr[i] {
                    dp[i] = max(dp[i], dp[j] + arr[i])
                }
            }
            ans = max(ans, dp[i])
        }
        fmt.Println(ans)
    }
}
```