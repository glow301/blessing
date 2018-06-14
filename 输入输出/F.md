### Description
Your task is to calculate the sum of some integers. 

### Input
Input contains multiple test cases, and one case one line. Each case starts with an integer N, and then N integers follow in the same line. 

### Output
For each test case you should output the sum of N integers in one line, and with one line of output for each line in input. 

### Simple Input
4 1 2 3 4  
5 1 2 3 4 5

### Simple Output
10  
15

### Code
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        while(scan.hasNextInt()) {
            int count = scan.nextInt();
            int sum = 0;
            for (int i = 0; i < count; i++) {
                sum += scan.nextInt();
            }
            System.out.println(sum);
        }
    }
}
```