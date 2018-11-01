## Longest Common Subsequence 
> UVA - 10405

### Description
给定两个字符串序列，打印两个序列之最长公共子序列的长度。

例如，两个序列 abcdgh 和 aedfhr 的最长公共子序列是 adh，长度是 3。

### Input
输入包含成对的行。 每对的第一行包含了第一个字符串，第二行包含了第二个字符串。每个字符串位于单独的一行，包含的字符数不超过 1,000 个。

### Output
对于输入的每对字符串，在一行中打印它们的最长公共子序列的长度。

### Simple Input
bcacbcabbaccbab  
bccabccbbabacbc  
a1b2c3d4e  
zz1yy2xx3ww4vv  
abcdgh  
aedfhr  
abcdefghijklmnopqrstuvwxyz  
a0b0c0d0e0f0g0h0i0j0k0l0m0n0o0p0q0r0s0t0u0v0w0x0y0z0  
abcdefghijklmnzyxwvutsrqpo  
opqrstuvwxyzabcdefghijklmn  

### Simple Output
11  
4  
3  
26  
14  

### Code
##### C++
```cpp
#include<iostream>
#include<cstring>
#include<algorithm>

using namespace std;

const int MAXN = 1005;
int dp[MAXN][MAXN];

int main() {
    char str1[MAXN];
    char str2[MAXN];
    while (cin.getline(str1, MAXN) && cin.getline(str2, MAXN)) {
        memset(dp, 0, sizeof(dp));
        int l1 = strlen(str1), l2 = strlen(str2);
        for (int i = 1; i <= l1; i++) {
            for (int j = 1; j <= l2; j++) {
                if (str1[i-1] == str2[j-1]) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                } else {
                    dp[i][j] = max(dp[i][j-1], dp[i-1][j]);
                }
            }
        }
        cout << dp[l1][l2] << endl;
    }
    return 0;
}
```

##### Go
```go
package main

import (
    "fmt"
    "os"
    "bufio"
)

const MAXN = 1005

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func main() {
    scanner := bufio.NewScanner(os.Stdin)
    for scanner.Scan() {
        str1 := scanner.Text()
        scanner.Scan()
        str2 := scanner.Text()

        l1, l2 := len(str1), len(str2)
        var dp [MAXN][MAXN]int
        for i := 1; i <= l1; i++ {
            for j := 1; j <= l2; j++ {
                if str1[i-1] == str2[j-1] {
                    dp[i][j] = dp[i-1][j-1] + 1
                } else {
                    dp[i][j] = max(dp[i][j-1], dp[i-1][j])
                }
            }
        }
        fmt.Println(dp[l1][l2])
    }
}
```