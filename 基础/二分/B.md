## Pie 

### Description
My birthday is coming up and traditionally I'm serving pie. Not just one pie, no, I have a number N of them, of various tastes and of various sizes. F of my friends are coming to my party and each of them gets a piece of pie. This should be one piece of one pie, not several small pieces since that looks messy. This piece can be one whole pie though. 

My friends are very annoying and if one of them gets a bigger piece than the others, they start complaining. Therefore all of them should get equally sized (but not necessarily equally shaped) pieces, even if this leads to some pie getting spoiled (which is better than spoiling the party). Of course, I want a piece of pie for myself too, and that piece should also be of the same size. 

What is the largest possible piece size all of us can get? All the pies are cylindrical in shape and they all have the same height 1, but the radii of the pies can be different.

### Input
One line with a positive integer: the number of test cases. Then for each test case:
* One line with two integers N and F with 1 ≤ N, F ≤ 10 000: the number of pies and the number of friends.
* One line with N integers ri with 1 ≤ ri ≤ 10 000: the radii of the pies.

### Output
For each test case, output one line with the largest possible volume V such that me and my friends can all get a pie piece of size V. The answer should be given as a floating point number with an absolute error of at most 10^−3.

### Sample Input
3  
3 3  
4 3 3  
1 24  
5  
10 5  
1 4 2 3 4 5 6 5 4 2  

### Sample Output
25.1327  
3.1416  
50.2655  

### 问题分析
1. 假设每个人可以分到的最大面积是 S，那么 S 的取值区间一定是 [0, max(S)]（即 0，到给定最大 pie 的面积）。
1. 这样一来，可以看作面积 S 在 [0, max(s)] 上，单调递增，所以可以用二分法求解。
1. 假设有 N 个 pie，M 个人来分，那么最大面积 S 与 人数之间应该有这样的关系 M = ∑(pieSize / S)，说人话就是，每个 pie 的面积，除以当前计算的面积，向下取整，再加起来，可以看`getPieNum`的实现。
1. 对面积进行二分查找，找到满足给定人数最大的面积，即是所求。

### Code
```java
import java.util.Scanner;

public class Main {
    public static final double EPS = 1e-6;
    public static double maxSize = 0;

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        int count = scan.nextInt();
        while (count-- > 0) {
            int pieNum   = scan.nextInt();
            int friends  = scan.nextInt();
            int[] pieArr = new int[pieNum];
            for (int i = 0; i < pieNum; i++) {
                pieArr[i] = scan.nextInt();
            }
            System.out.println(calcSize(pieArr, friends));
        }
    }

    // 计算每个人分到 pie 的最大面积
    public static String calcSize(int[] pieArr, int friends) {
        double[] pieSize = getPieSize(pieArr);
        double left = 0, right = maxSize, mid = 0;

        while (right - left > EPS) {
            mid = left + (right - left) / 2;
            int num = getPieNum(mid, pieSize);
            if (num > friends) {
                left = mid + EPS;
            } else {
                right = mid;
            }
        }
        return String.format("%.4f", mid);
    }

    // 计算每个 pie 的面积
    public static double[] getPieSize(int[] pieArr) {
        double[] pieSize = new double[pieArr.length];
        for (int i = 0; i < pieArr.length; i++) {
            pieSize[i] = Math.PI * pieArr[i] * pieArr[i];
            if (pieSize[i] > maxSize) {
                maxSize = pieSize[i];
            }
        }
        return pieSize;
    }

    // 计算给定的面积，可以分给多少个人
    public static int getPieNum(double size, double[] pieSize) {
        int count = 0;
        for (double s : pieSize) {
            count += Math.floor(s / size);
        }
        return count;
    }
}
```

