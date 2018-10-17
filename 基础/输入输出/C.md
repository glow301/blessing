### Description
Your task is to Calculate a + b.

### Input
Input contains multiple test cases. Each test case contains a pair of integers a and b, one pair of integers per line. A test case containing 0 0 terminates the input and this test case is not to be processed.

### Output 
For each pair of input integers a and b you should output the sum of a and b in one line, and with one line of output for each line in input.  

### Simple Input
1 5  
10 20  
0 0

### Simple Output
6  
30

### Code
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        while(true) {
            int a = scan.nextInt();
            int b = scan.nextInt();

            if (a == 0 && b == 0 ){
                break;
            }
            System.out.println(a + b);
        }
    }
}
```

```go
package main

import "fmt"

func main() {
    var a, b int
    for {
        fmt.Scanf("%d %d\n", &a, &b)
        if a == 0 && b == 0 {
            break
        }
        fmt.Println(a+b)
    }
}
```