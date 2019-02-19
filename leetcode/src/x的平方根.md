## x 的平方根
> leetcode - 69

### Description
实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

### 示例
#### 示例 1
```
输入: 4
输出: 2
```

#### 示例 2
```
输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。
```

### 分析
用二分法不断逼近。

### Code
```go
func mySqrt(x int) int {
    low, high := 0, x
    for low <= high {
        mid := low + ((high - low) >> 1)
        if mid * mid == x {
            return mid
        }
        if mid * mid > x {
            high = mid - 1
        } else {
            low = mid + 1
        }
    }
    return low - 1
}
```
