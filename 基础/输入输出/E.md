### Description
Your task is to calculate the sum of some integers. 

### Input
Input contains an integer N in the first line, and then N lines follow. Each line starts with a integer M, and then M integers follow in the same line. 

### Output
For each group of input integers you should output their sum in one line, and with one line of output for each line in input. 

### Simple Input
2  
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

        int line = scan.nextInt();
        for (int i = 0; i < line; i++) {
            int count = scan.nextInt();
            int sum = 0;
            for (int j = 0; j < count; j++) {
                sum += scan.nextInt();
            }
            System.out.println(sum);
        }
    }
}
```

##### GO 语言版本
```go
package main

import "fmt"

func main() {
    var size, num, sum int
    fmt.Scanf("%d\n", &size)
    for i := 0; i < size; i++ {
        var count int
        fmt.Scanf("%d", &count)
        sum = 0
        for j := 0; j < count; j++ {
            fmt.Scanf("%d", &num)
            sum += num
        }
        fmt.Scanf("\n")
        fmt.Println(sum)
    }
}
```