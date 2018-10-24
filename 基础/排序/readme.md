## 排序

### 冒泡排序
> 冒泡排序是一种简单的排序算法，通过交换两个**相邻**元素的位置来进行排序
* 时间复杂度 $O(n^2)$
* 空间复杂度 $O(n)$

##### C++
```cpp
void bubbleSort(int array[], int left, int right) {
    for (int i = left; i < right; i++) {
        for (int j = 0; j < right - i; j++) {
            if (array[j] > array[j+1]) {
                swap(array[j], array[j+1]);
            }
        }
    }
}
```
##### GO
```go
func bubbleSort(arr []int) {
    right := len(arr) - 1
    for i := 0; i < right; i++ {
        for j := 0; j < right - i; j++ {
            if arr[j] > arr[j+1] {
              arr[j], arr[j+1] = arr[j+1], arr[j]
            }
        }
    }
}
```

### 选择排序
> 选择排序的思想是，在未排序的数组中，找到 最大/最小 的元素，放到数组起始位置，再从剩下的元素中，找到 最大/最小 的元素，以此类推。
* 时间复杂度 $O(n^2)$
* 空间复杂度 $O(n)$

##### C++
```cpp
void selectSort(int array[], int left, int right) {
    for (int i = left; i < right; i++) {
        int min = i;
        for (int j = i + 1; j <= right; j++) {
            if (array[j] < array[min]) {
                min = j;
            }
        }
        if (min != i) {
            swap(array[i], array[min])
        }
    }
}
```

##### GO
```go
func selectSort(arr []int) {
    length := len(arr)
    for i := 0; i < length - 1; i++ {
        min := i
        for j := i + 1; j < length; j++ {
            if arr[j] < arr[min] {
                min = j
            }
        }
        if min != i {
            arr[i], arr[min] = arr[min], arr[i]
        }
    }
}
```

### 直接插入排序
> 直接插入排序的思想是，对数组从前往后扫描，构建有序序列，如果当前元素小于**已排序**序列中的最后元素，那么从已排序序列中从后向前扫描，找到合适的位置插入。
* 时间复杂度 $O(n^2)$
* 空间复杂度 $O(n)$

##### C++
```cpp
void insertSort(int array[], int left, int right) {
    for (int i = left + 1; i < right; i++) {
        int tmp = array[i];
        int j = i - 1;
        while (j >= left && tmp < array[j]) {
            array[j+1] = array[j];
            j--;
        }
        array[j+1] = tmp;
    }
}
```

##### GO
```go
func insertSort(arr []int) {
    length := len(arr)
    for i := 1; i < length; i++ {
        tmp := arr[i]
        j := i - 1
        for j >= 0 && tmp < arr[j] {
            arr[j+1] = arr[j]
            j--
        }
        arr[j+1] = tmp
    }
}
```

### 快速排序
> 快速排序是面试中常见的一个排序算法，基本思想是使用分治的思想，使用一个元素（通常是数组第一个元素）作为标志 记为`key`，对数组进行一次划分，`key`左侧的元素都小于`key`，`key`右侧的元素都大于 `key`，然后对左右两个数组再进行调用。
* 时间复杂度 $O(log N)$
* 空间复杂度 $O(n)$

todo 为什么快排的时间复杂度是 $O(nlog N)$

##### C++
```cpp
void quickSort(int array[], int left, int right) {
    if (left >= right) {
        return;
    }
    int low = left, high = right;
    int mid = array[left];
    while (low < high) {
        while (array[high] >= mid && low < high) {
            high--;
        }
        array[low] = array[high];
        while (array[low] < mid && low < high) {
            low++;
        }
        array[high] = array[low];
    }
    array[low] = mid;
    quickSort(array, left, low-1);
    quickSort(array, low+1, right);
}
```

##### GO
```go
func quickSort(arr []int) {
    if len(arr) <= 1 {
        return
    }
    mid := arr[0]
    low, high := 0, len(arr)-1
    for i := 1; i <= high; {
        if arr[i] > mid {
            arr[i], arr[high] = arr[high], arr[i]
            high--
        } else {
            arr[i], arr[low] = arr[low], arr[i]
            low++
            i++
        }
    }
    quickSort(arr[:low])
    quickSort(arr[low+1:])
}
```

### 堆排序
> 堆排序是利用堆这种数据结构所设计的一种排序算法，主要涉及到建堆，调整堆的操作（以大顶堆为例）
* 时间复杂度 $O(log N)$
* 空间复杂度 $O(1)$

todo 关于堆的简介

##### C++
```cpp
void maxHeapify(int arr[], int start, int end) {
    int root = start;
    int son  = root*2+1;
    while (son <= end) {
        if (son+1 <= end && arr[son] < arr[son+1]) {
            son++;
        }
        if (arr[root] > arr[son]) {
            return;
        } else {
            swap(arr[root], arr[son]);
            root = son;
            son  = root*2+1;
        }
    }
}

void heapSort(int arr[], int len) {
    for (int i = len/2-1; i >= 0; i--) {
        maxHeapify(arr, i, len-1);
    }
    for (int i = len-1; i > 0; i--) {
        swap(arr[0], arr[i]);
        maxHeapify(arr, 0, i-1);
    }
}
```

##### GO
```go
func maxHeapify(arr []int, start, end int) {
    root := start
    son  := root*2+1
    for son <= end {
        if son+1 <= end && arr[son] < arr[son+1] {
            son++
        }
        if arr[root] > arr[son] {
            return
        } else {
            arr[root], arr[son] = arr[son], arr[root]
            root = son
            son  = root*2+1
        }
    }
}

func heapSort(arr []int) {
    length := len(arr)
    for i := length/2-1; i >= 0; i-- {
        maxHeapify(arr, i, length-1)
    }
    for i := length-1; i > 0; i-- {
        arr[0], arr[i] = arr[i], arr[0]
        maxHeapify(arr, 0, i-1)
    }
}
```