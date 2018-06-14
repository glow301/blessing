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
    private static int maxSize   = 0;

    public static void init() {
        for (int i = 0; i < LEN; i++) {
            p[i]    = i;
            rank[i] = 1;
        }
    }

    public static int find(int x) {
        if (x != p[x]) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    public static void union(int x, int y) {
        x = find(x);
        y = find(y);
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

    public static int findMax(int[] arr) {
        int max = 1;
        for (int i = 0; i < maxSize; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        return max;
    }

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        while (scan.hasNextInt()) {
            int cases = scan.nextInt();
            maxSize   = cases;
            init();
            while (cases-- > 0) {
                int x = scan.nextInt();
                int y = scan.nextInt();
                maxSize = x > maxSize ? x : maxSize;
                maxSize = y > maxSize ? y : maxSize;
                union(x - 1, y - 1);
            }
            System.out.println(findMax(rank));
        }
    }
}
```