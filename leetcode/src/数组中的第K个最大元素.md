## 数组中的第K个最大元素
> leetcode - 215

### Description
在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

### 示例
#### 示例 1
```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```

#### 示例 2
```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```

* 你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

### 分析
#### 方法 1
先对数组排序，然后找到第 K 个最大元素。

#### 方法 2
快速选择算法
* 用类似快速排序的思想，找到一个元素，将数组分为两部分，左边的元素都小于该元素，右边的元素都大于该元素。
* 判断右边元素的个数，如果元素个数大于 K，则继续对右边元素重复上面的步骤，否则在左边找第 len(left) - K 大元素。

### Code
```go
func findKthLargest(nums []int, k int) int {
    if len(nums) < 1 {
        return 0
    }
    quickSelect(nums, k)
    return nums[len(nums) - k]
}

func quickSelect(nums []int, k int) {
    l := len(nums)
    if l < 2 {
        return 
    }

    key, low, high := nums[0], 0, l - 1
    for i := 1; i <= high; {
        if nums[i] > key {
            nums[i], nums[high] = nums[high], nums[i]
            high--
        } else {
            nums[i], nums[low] = nums[low], nums[i]
            low++; i++
        }
    }
    if l - low == k {
        return
    }
    if l - low < k {
        quickSelect(nums[:low], k - l + low)
    } else {
        quickSelect(nums[low+1:], k)
    }   
}
```