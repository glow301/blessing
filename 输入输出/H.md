### Description
Your task is to calculate the sum of some integers. 

### Input
Input contains an integer N in the first line, and then N lines follow. Each line starts with a integer M, and then M integers follow in the same line. 

### Output
For each group of input integers you should output their sum in one line, and you must note that there is a blank line between outputs. 

### Simple Input
3  
4 1 2 3 4  
5 1 2 3 4 5  
3 1 2 3

### Simple Output
10  
  
15  
  
6  

### Code
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        int line = scan.nextInt();
        for (int i = 0; i < line; i++) {
            int count = scan.nextInt();
            int sum = 0;
            for (int j = 0; j < count; j++) {
                sum += scan.nextInt();
            }
            System.out.println(sum);
            if (i != line - 1) {
                System.out.println();
            }
        }
    }
}
```