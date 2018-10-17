### Description
Your task is to Calculate a + b. 

### Input
Input contains an integer N in the first line, and then N lines follow. Each line consists of a pair of integers a and b, separated by a space, one pair of integers per line. 

### Output
For each pair of input integers a and b you should output the sum of a and b in one line, and with one line of output for each line in input.

### Simple Input
2  
1 5  
10 20  

### Simple Output
6  
30

### Code
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args){
        Scanner scan = new Scanner(System.in);

        int line = scan.nextInt();
        while(line-- > 0) {
            int a = scan.nextInt();
            int b = scan.nextInt();
            System.out.println(a + b);
        }
    }
}
```

##### GO 语言版本
```go
package main

import "fmt"

func main() {
    var size, a, b int
    fmt.Scanf("%d\n", &size)
    for i := 0; i < size; i++ {
        fmt.Scanf("%d %d\n", &a, &b)
        fmt.Println(a+b)
    }
}
```