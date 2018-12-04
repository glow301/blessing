## Jumping Cows 
> POJ - 2181

### Description
Farmer John's cows would like to jump over the moon, just like the cows in their favorite nursery rhyme. Unfortunately, cows can not jump. 

The local witch doctor has mixed up P (1 <= P <= 150,000) potions to aid the cows in their quest to jump. These potions must be administered exactly in the order they were created, though some may be skipped.

Each potion has a 'strength' (1 <= strength <= 500) that enhances the cows' jumping ability. Taking a potion during an odd time step increases the cows' jump; taking a potion during an even time step decreases the jump. Before taking any potions the cows' jumping ability is, of course, 0. 

No potion can be taken twice, and once the cow has begun taking potions, one potion must be taken during each time step, starting at time 1. One or more potions may be skipped in each turn.

Determine which potions to take to get the highest jump.

### Input
* Line 1: A single integer, P

* Lines 2..P+1: Each line contains a single integer that is the strength of a potion. Line 2 gives the strength of the first potion; line 3 gives the strength of the second potion; and so on.

### Output
* Line 1: A single integer that is the maximum possible jump.

### Sample Input
8  
7  
2  
1  
8  
4  
3  
5  
6  

### Sample Output
17

### 分析
### Code
##### C++
```cpp
#include<cstdio>

using namespace std;

const int MAXN = 150010;
int dp[MAXN];

int main(){
    int n;
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &dp[i]);
    }
    int flag = 0;
    int sum = 0;
    for (int i = 1; i <= n; i++) {
        if (dp[i] >= dp[i-1] && dp[i] >= dp[i+1] && flag == 0) {
            sum += dp[i];
            flag = 1;
        } else if (dp[i] <= dp[i-1] && dp[i] <= dp[i+1] && flag == 1) {
            sum -= dp[i];
            flag = 0;
        }
    }
    printf("%d\n", sum);

    return 0;
}
```

##### GO
```go
package main

import "fmt"

func main(){
    n := 0
    fmt.Scanf("%d\n", &n)
    dp := make([]int, n+2)
    for i := 1; i <= n; i++ {
        fmt.Scanf("%d\n", &dp[i])
    }
    ans, flag := 0, 0
    for i := 1; i <= n; i++ {
        if dp[i] >= dp[i-1] && dp[i] >= dp[i+1] && 0 == flag {
            ans += dp[i]
            flag = 1
        } else if dp[i] <= dp[i-1] && dp[i] <= dp[i+1] && 1 == flag {
            ans -= dp[i]
            flag = 0
        }
    }
    fmt.Println(ans)
}
```