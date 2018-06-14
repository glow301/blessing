## How Many Tables 

### Description
Today is Ignatius' birthday. He invites a lot of friends. Now it's dinner time. Ignatius wants to know how many tables he needs at least. You have to notice that not all the friends know each other, and all the friends do not want to stay with strangers. 

One important rule for this problem is that if I tell you A knows B, and B knows C, that means A, B, C know each other, so they can stay in one table. 

For example: If I tell you A knows B, B knows C, and D knows E, so A, B, C can stay in one table, and D, E have to stay in the other one. So Ignatius needs 2 tables at least. 

### Input
The input starts with an integer T(1<=T<=25) which indicate the number of test cases. Then T test cases follow. Each test case starts with two integers N and M(1<=N,M<=1000). N indicates the number of friends, the friends are marked from 1 to N. Then M lines follow. Each line consists of two integers A and B(A!=B), that means friend A and friend B know each other. There will be a blank line between two cases. 

### Output
For each test case, just output how many tables Ignatius needs at least. Do NOT print any blanks. 

### Simple Input
2  
5 3  
1 2  
2 3  
4 5  
  
5 1  
2 5  

### Simple Output
2  
4

### 问题分析
1. 对每个人进行编号，从 0 开始，那么这些人可以组成若干个不相交集。
1. 对于可以坐在一个桌子上的人，他们应该是属于同一个不相交集。
1. 最初的时候，每个集合应该只有 1 个人，通过好友关系，不断合并，最终集合的个数，就是桌子的个数。

### Code
```java
import java.util.Scanner;

public class Main {
    
    private static int count = 0;

    public static int[] init(int num) {
        int[] p = new int[num];
        for (int i = 0; i < num; i++) {
            p[i] = i;
        }
        return p;
    }

    // 默认每个人一个桌子，每合并成功一次，减少一个
    public static int[] setUnion(int x, int y, int[] p) {
        x = find(x, p);
        y = find(y, p);
        if (x != y) {
            count--;
        }
        p[x] = y;
        return p;
    }

    public static int find(int x, int[] p) {
        while (x != p[x]) {
            x = p[x];
        }
        return x;
    }

    public static void main(String[] args){
        Scanner scan = new Scanner(System.in);

        int cases = scan.nextInt();
        while (cases-- > 0) {
            count         = scan.nextInt();
            int relation  = scan.nextInt();

            int[] p = init(count);
            while (relation-- > 0) {
                int x = scan.nextInt();
                int y = scan.nextInt();
                // 因为数组索引从 0 开始，所以需要减 1
                p = setUnion(x - 1, y - 1, p);
            }
            System.out.println(count);
        }
    }
}
```