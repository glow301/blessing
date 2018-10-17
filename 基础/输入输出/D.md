### Description
Your task is to Calculate the sum of some integers. 

### Input
Input contains multiple test cases. Each test case contains a integer N, and then N integers follow in the same line. A test case starting with 0 terminates the input and this test case is not to be processed. 

### Output
For each group of input integers you should output their sum in one line, and with one line of output for each line in input. 

### Simple Input
4 1 2 3 4  
5 1 2 3 4 5  
0 

### Simple Output
10  
15

### Code
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        while (true) {
            int count = scan.nextInt();
            if (0 == count) {
                break;
            }
            int sum   = 0;
            for (int i = 0; i < count; i++) {
                sum += scan.nextInt();
            }
            System.out.println(sum);
        }
    }
}
```

```go
package main

import "fmt"

func main() {
    var size, num, sum int
    for {
        sum = 0
        fmt.Scanf("%d", &size)
        if 0 == size {
            break
        }
        for i := 0; i < size; i++{
            fmt.Scanf("%d", &num)
            sum += num
        }
        fmt.Scanf("\n")
        fmt.Println(sum)
    }
}
```