## Ubiquitous Religions 

### Description
北理CS学院2015级的学长学姐们终于搬到村里了，他们住进了新的宿舍，你想知道学校给CS学院一共安排了多少宿舍，但矜持的学长学姐们是不会直接告诉你他们宿舍号的，你唯一机会是当看到两位学姐相伴而行或者两位学长相依为Gay的时候，就可以断定他们住在同一间宿舍。已知CS学院2015级共有n(n <= 50000)名同学，你看到了m(m<=n(n-1)/2)组学长学姐对，现在请问他们至多住进了多少间宿舍。

### Input
有多组数据。对于每组数据：
第一行：两个整数n和m。 
以下m行：每行包含两个整数i和j，表示推断i和j住在同一间宿舍。学生编号从1到n。 
输入的最后一行中,n = m = 0。

### Output
对于每组测试数据，输出一行，输出数据序号( 从1开始) 和宿舍的最大数量。（参见样例）

### Simple Input
10 9  
1 2  
1 3  
1 4  
1 5  
1 6  
1 7  
1 8  
1 9  
1 10  
10 4  
2 3  
4 5  
4 8  
5 8  
0 0  

### Simple Output
Case 1: 1  
Case 2: 7

### 问题分析
1. 对每个人进行编号，从 0 开始，那么这些人可以组成若干个不相交集。
1. 初始化宿舍总是等于学生人数，对输入的数据进行两两合并，每合并成功一次，宿舍总数减小 1。
1. 最终查看 rank 数组中，value 不为 0 的个数，即需哟宿舍的个数。

### Code
```java
import java.util.Scanner;

public class Main {

    private static int size = 0;
    private static int[] p;
    private static int[] rank;
    private static int count = 0;

    private static void init(int len) {
        p    = new int[len];
        rank = new int[len];
        for (int i = 0; i < len; i++) {
            p[i]    = i;
            rank[i] = 1;
        }
    }

    private static int find(int x) {
        if (x != p[x]) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    private static void union(int x, int y) {
        x = find(x);
        y = find(y);
        if (x != y) {
            // 每次合并，宿舍数量减 1
            count--;
            if (rank[x] >= rank[y]) {
                p[y]    = x;
                rank[x] += rank[y];
                rank[y] = 0;
            } else {
                p[x]    = y;
                rank[y] += rank[x];
                rank[x] = 0;
            }
        }
    }

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int counter  = 1;
        while (scan.hasNextInt()) {
            size      = scan.nextInt();
            count     = size;
            int line  = scan.nextInt();
            if (0 == size && 0 == line) {
                break;
            }
            init(size);
            while(line-- > 0) {
                union(scan.nextInt() - 1, scan.nextInt() - 1);
            }
            System.out.println("Case " + counter + ": " + count);
            counter++;
        }
    }
}
```