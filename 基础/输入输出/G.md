### Description
Your task is to Calculate a + b. 

### Input
The input will consist of a series of pairs of integers a and b, separated by a space, one pair of integers per line. 

### Output
For each pair of input integers a and b you should output the sum of a and b, and followed by a blank line. 

### Simple Input
1 5  
10 20

### Simple Output
6  
  
30  
  

### Code
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        while (scan.hasNextInt()) {
            int a = scan.nextInt();
            int b = scan.nextInt();
            System.out.println(a + b);
            System.out.println();
        }
    }
}
```

##### GO 语言版本
```go
package main

import "fmt"

func main() {
    var a, b int
    for {
        _, err := fmt.Scanf("%d %d\n",&a, &b)
        if err != nil {
            break
        }
        fmt.Printf("%d\n\n", a+b)
    }
}
```