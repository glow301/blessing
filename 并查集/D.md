## The Suspects 

### Description
警察抓贩毒集团。有不同类型的犯罪集团，人员可能重复，集团内的人会相互接触。现在警察在其中一人（0号）身上搜出毒品，认为与这个人直接接触或通过其他人有间接接触的人都是嫌疑犯。问包括0号犯人共有多少嫌疑犯？

### Input
多样例输入。
每个测试用例以两个整数n和m开头，其中n为人数，m为犯罪集团数。你可以假定0 < n <= 30000和0 <= m <= 500。在所有的情况下，每个人都有自己独特的整数编号0到n−1, 且0号是公认的嫌疑犯。
接下来m行输入，每个犯罪集团一行。每一行从一个整数k开始，它本身表示集团内成员的数量。按照成员的数量，在这个组中有k个整数表示人员。一行中的所有整数都被至少一个空格隔开。
n = 0且m = 0时输入结束。

### Output
对于每个样例，输出嫌疑犯人数。

### Simple Input
100 4  
2 1 2  
5 10 13 11 12 14  
2 0 1  
2 99 2  
200 2  
1 5  
5 1 2 3 4 5  
1 0  
0 0

### Simple Output
4  
1  
1  

### 问题分析
1. 将给定的人进行合并操作，这里合并不是之前到两个元素到合并了，需要稍微变化一下，比如数组剩余元素都和第一个元素合并。
1. 在最终的数组中，找到 0 的根节点，再查到该节点到元素格式即可。

### Code
```java
import java.util.Scanner;

public class Main {

    private static final int LEN = 30000;
    private static int[] p       = new int[LEN];
    private static int[] rank    = new int[LEN];

    private static void init(int size) {
        for (int i = 0; i < size ; i++) {
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

    private static void unionArr(int[] arr) {
        int base = arr[0];
        for (int i = 1; i < arr.length; i++) {
            int x = find(base);
            int y = find(arr[i]);

            if (x != y) {
                if (rank[x] >= rank[y]) {
                    p[y] = x;
                    rank[x] += rank[y];
                } else {
                    p[x] = y;
                    rank[y] += rank[x];
                }
            }
        }
    }

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        while (scan.hasNextInt()) {
            int number = scan.nextInt();
            int groups = scan.nextInt();
            if (0 == number && 0 == groups) {
                break;
            }
            init(number);
            while (groups-- > 0) {
                int count = scan.nextInt();
                int[] row = new int[count];
                while (count-- > 0) {
                    row[count] = scan.nextInt();
                }
                unionArr(row);
            }
            System.out.println(rank[find(0)]);
        }
    }
}
```