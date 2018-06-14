## 畅通工程 

### Description
某省调查城镇交通状况，得到现有城镇道路统计表，表中列出了每条道路直接连通的城镇。省政府“畅通工程”的目标是使全省任何两个城镇间都可以实现交通（但不一定有直接的道路相连，只要互相间接通过道路可达即可）。问最少还需要建设多少条道路？ 

### Input
测试输入包含若干测试用例。每个测试用例的第1行给出两个正整数，分别是城镇数目N ( < 1000 )和道路数目M；随后的M行对应M条道路，每行给出一对正整数，分别是该条道路直接连通的两个城镇的编号。为简单起见，城镇从1到N编号。 
注意:两个城市之间可以有多条道路相通,也就是说 
3 3 
1 2 
1 2 
2 1 
这种输入也是合法的 
当N为0时，输入结束，该用例不被处理。 

### Output
对每个测试用例，在1行里输出最少还需要建设的道路数目。

### Simple Input
4 2
1 3
4 3
3 3
1 2
1 3
2 3
5 2
1 2
3 5
999 0
0

### Simple Output
1
0
2
998

### 问题分析
1. 每一个城镇看成是一个元素，互相联通的城镇可以组成若干个不相交集。
1. 那么不相交集的个数减 1，就是需要修建的道路的个数。

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

    public static int find(int x, int[] p) {
        while (x != p[x]) {
            x = p[x];
        }
        return x;
    }

    public static int[] setUnion(int x, int y, int[] p) {
        x = find(x, p);
        y = find(y, p);
        if (x != y) {
            count--;
        }
        p[x] = y;
        return p;
    }
    
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        
        while (true) {
            count     = scan.nextInt();
            if (0 == count) {
                break;
            }

            int roads = scan.nextInt();
            int[] p   = init(count);
            while (roads-- > 0) {
                int x = scan.nextInt();
                int y = scan.nextInt();
                p = setUnion(x - 1, y - 1, p);
            }
            System.out.println(count - 1);
        }
    }
}
```