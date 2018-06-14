## 二分查找
> 二分查找是一种优化搜索的方法，它通过不断减少查找区间，逐步达到逼近结果的目的

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