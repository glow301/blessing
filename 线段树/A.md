## I Hate It 

### Description
很多学校流行一种比较的习惯。老师们很喜欢询问，从某某到某某当中，分数最高的是多少。 
这让很多学生很反感。 

不管你喜不喜欢，现在需要你做的是，就是按照老师的要求，写一个程序，模拟老师的询问。当然，老师有时候需要更新某位同学的成绩。

### Input
本题目包含多组测试，请处理到文件结束。 
在每个测试的第一行，有两个正整数 N 和 M ( 0<N<=200000,0<M<5000 )，分别代表学生的数目和操作的数目。 
学生ID编号分别从1编到N。 
第二行包含N个整数，代表这N个学生的初始成绩，其中第i个数代表ID为i的学生的成绩。 
接下来有M行。每一行有一个字符 C (只取'Q'或'U') ，和两个正整数A，B。 
当C为'Q'的时候，表示这是一条询问操作，它询问ID从A到B(包括A,B)的学生当中，成绩最高的是多少。 
当C为'U'的时候，表示这是一条更新操作，要求把ID为A的学生的成绩更改为B。 

### Output
对于每一次询问操作，在一行里面输出最高成绩。

### Sample Input
5 6  
1 2 3 4 5  
Q 1 5  
U 3 6  
Q 3 4  
Q 4 5  
U 2 9  
Q 1 5  

### Sample Output
5  
6  
5  
9  

### 问题分析

### Code
```java
import java.util.Scanner;

public class Main {
    private static final int MAX = 200010;
    private static int[] sum     = new int[MAX<<2];
    private static int[] arr     = new int[MAX];

    private static void pushUp(int root) {
        int left  = sum[root*2+1];
        int right = sum[root*2+2];
        sum[root] = left > right ? left : right;
    }

    private static void build(int left, int right, int root) {
        if (left == right) {
            sum[root] = arr[left];
            return;
        }
        int mid = (left + right) >> 1;
        build(left, mid, root*2+1);
        build(mid+1, right, root*2+2);
        pushUp(root);
    }

    private static void update(int l, int c, int left, int right, int root) {
        if (left == right) {
            sum[root] = c;
            return;
        }
        int mid = (left + right) >> 1;
        if (l <= mid) {
            update(l, c, left, mid, root*2+1);
        } else {
            update(l, c, mid+1, right, root*2+2);
        }
        pushUp(root);
    }

    private static int query(int start, int end, int left, int right, int root) {
        int leftMax  = 0;
        int rightMax = 0;
        if (start <= left && end >= right) {
            return sum[root];
        }
        int mid   = (left + right) >> 1;
        if (start <= mid) {
            leftMax =  query(start, end, left, mid, root*2+1);
        }
        if (end > mid) {
            rightMax = query(start, end, mid+1, right, root*2+2);
        }
        return leftMax > rightMax ? leftMax : rightMax;
    }

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        while (scan.hasNext()) {
            int stuNum = scan.nextInt();
            int opNum  = scan.nextInt();
            int index  = 0;
            while (stuNum-- > 0) {
                arr[index] = scan.nextInt();
                index++;
            }
            build(0, index-1, 0);
            while (opNum-- > 0) {
                String op = scan.next();
                int op1   = scan.nextInt();
                int op2   = scan.nextInt();
                if (op.equals("Q")) {
                    System.out.println(query(op1 - 1, op2 - 1, 0, index-1, 0));
                } else if (op.equals("U")) {
                    update(op1 - 1, op2, 0, index-1, 0);
                }
            }
        }
    }
}
```