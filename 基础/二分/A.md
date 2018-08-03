## Can you solve this equation? 

### Description
Now,given the equation 8*x^4 + 7*x^3 + 2*x^2 + 3*x + 6 == Y,can you find its solution between 0 and 100; 
Now please try your lucky.

### Input
The first line of the input contains an integer T(1<=T<=100) which means the number of test cases. Then T lines follow, each line has a real number Y (fabs(Y) <= 1e10);

### Output
For each test case, you should just output one real number(accurate up to 4 decimal places),which is the solution of the equation,or “No solution!”,if there is no solution for the equation between 0 and 100.

### Sample Input
2  
100  
-4

### Sample Output
1.6152  
No solution!

### 问题分析
1. 对给定方程求导，可以发现，在 [0, 100] 区间上， 函数单调递增。所以可以用二分法求解。
1. 判断边界值的情况，分别令 x=0, x=100，区间之外的认为无解。
1. 在 [0, 100] 内，用二分法求解。

### Code
```java
import java.util.Scanner;

public class Main {
    public static final double EPS = 1e-7;

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        int line = scan.nextInt();
        while (line-- > 0) {
            int y = scan.nextInt();
            if (y >= calc(0) && y <= calc(100)) {
                findSolution(y);
            } else {
                System.out.println("No solution!");
            }
        }
    }

    public static void findSolution(int y) {
        double left  = 0;
        double right = 100;
        while (right - left > EPS) {
            double mid = left + (right - left) / 2;
            double res = calc(mid);
            if (res > y) {
                right = mid - EPS;
            } else {
                left = mid + EPS;
            }
        }
        System.out.printf("%.4f", left);
        System.out.println();
    }

    private static double calc(double x) {
        return 8*x*x*x*x + 7*x*x*x + 2*x*x + 3*x +6;
    }
}
```