## More is better 

### Description
Mr Wang wants some boys to help him with a project. Because the project is rather complex, the more boys come, the better it will be. Of course there are certain requirements. 

Mr Wang selected a room big enough to hold the boys. The boy who are not been chosen has to leave the room immediately. There are 10000000 boys in the room numbered from 1 to 10000000 at the very beginning. After Mr Wang's selection any two of them who are still in this room should be friends (direct or indirect), or there is only one boy left. Given all the direct friend-pairs, you should decide the best way. 

### Input
The first line of the input contains an integer n (0 ≤ n ≤ 100 000) - the number of direct friend-pairs. The following n lines each contains a pair of numbers A and B separated by a single space that suggests A and B are direct friends. (A ≠ B, 1 ≤ A, B ≤ 10000000)

### Output
The output in one line contains exactly one integer equals to the maximum number of boys Mr Wang may keep. 

### Simple Input
4  
1 2  
3 4  
5 6  
1 6  
4  
1 2  
3 4  
5 6  
7 8  

### Simple Output
4  
2

### 问题分析
1. 每个人看成一个元素，好友关系的可以组成一个集合。
1. 问题可以抽象成，求最终生成的不相交集中，元素最多的那个集合的元素个数。
1. 用另一个数组表示每个集合中元素的个数，数组的索引就是每个不相交集中的 leader。
1. 在最终的数组中，找出元素最多的那个。

### Code
```java
import java.util.Scanner;

public class Main {
    private static final int LEN = 100000;
    private static int[] p       = new int[LEN];
    private static int[] rank    = new int[LEN];
    private static int size;
    private static int max;


    private static void init(){
        for (int i = 0; i < LEN; i++) {
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
            if (rank[x] >= rank[y]) {
                p[y]    = x;
                rank[x] += rank[y];
                rank[y] = 0;
                max     = rank[x] > max ? rank[x] : max;
            } else {
                p[x]    = y;
                rank[y] += rank[x];
                rank[x] = 0;
                max     = rank[y] > max ? rank[y] : max;
            }
        }
    }

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        while (scan.hasNext()) {
            size    = scan.nextInt();
            max     = 1;
            init();
            for (int i = 0; i < size; i++) {
                union(scan.nextInt()-1, scan.nextInt()-1);
            }
            System.out.println(max);
        }
    }
}
```