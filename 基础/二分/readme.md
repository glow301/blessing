## 二分查找
> 二分查找是一种优化搜索的方法，它通过不断减少查找区间，逐步达到逼近结果的目的。
> 它是一种基础的，可以降低时间复杂度的算法。

### 关于二分查找先说明几点
* 给定数组必须是有序的
* 必须是顺序结构，链表就无法使用二分查找了

#### 一个最基本的例子
```java
public class BinarySearch {
    public static void main(String[] args) {
        int[] arr = {1,4,6,7,9,10,35};
        System.out.println(binarySearch(arr, 1));
        System.out.println(binarySearch(arr, 2));
        System.out.println(binarySearch(arr, 35));
    }

    public static int binarySearch(int[] array, int key) {
        int left    = 0;
        int right   = array.length;
        int mid     = 0;

        while (left <= right) {
            mid = left + (right - left) / 2;
            if (array[mid] < key) {
                left = mid + 1;
            } else if (array[mid] > key) {
                right = mid - 1;
            } else {
                return mid;
            }
        }
        return -1;
    }
}
```

* c++ 版代码
```cpp
#include<cstdio>

using namespace std;

int binarySearch(int *arr, int key, int length) {
    int left = 0, right = length;
    while (left <= right) {
        int mid = (left + right) >> 1;
        if (arr[mid] < key) {
            left = mid + 1;
        } else if (arr[mid] > key) {
            right = mid - 1;;
        } else {
            return mid;
        }
    }
    return -1;
}

int main() {
    int arr[]  = {1, 5, 7, 9, 12};
    int arr2[] = {2, 4, 5, 7, 9, 23};
    int arr3[] = {1, 4, 5, 7, 9, 23};

    printf("%d\n", binarySearch(arr, 5, sizeof(arr)));
    printf("%d\n", binarySearch(arr2, 9, sizeof(arr2)));
    printf("%d\n", binarySearch(arr3, 50, sizeof(arr3)));
}
```

#### 分析一下二分的时间复杂度
##### 第一种思路
* 把整个数组看成一颗**二叉查找树**，那么最坏的情况就是搜索到叶子节点，树的高度就是 log n，所以时间复杂度就是 $O(log N)$
##### 第二种思路
* 假设数组的长度是 n，我们最多查询 k 次可以查找到。二分每次会把查找的范围缩小一半。
    1. 第一次查找区间长度是 n。
    1. 第二次查找区间长度是 n/2（$n/2^1$）。
    1. 第三次查找区间长度是 n/4（$n/2^2$）。
    1. ...  
    
当查找区间缩小到 1 的时候，就找到了目标元素。所以，假设第 k 次查找，区间长度缩小到 1，那么就有 $n/2^k=1$ ，即 $k = log2N$
