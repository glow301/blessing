### Description
Your task is to Calculate a + b. 
Too easy?! Of course! I specially designed the problem for acm beginners. 
You must have found that some problems have the same titles with this one, yes, all these problems were designed for the same aim. 

### Input
The input will consist of a series of pairs of integers a and b, separated by a space, one pair of integers per line. 

### Output 
For each pair of input integers a and b you should output the sum of a and b in one line, and with one line of output for each line in input. 

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
    public static void main(String args[]) {
        Scanner scan = new Scanner(System.in);

        while(cin.hasNext()){
            int a = scan.nextInt();
            int b = scan.nextInt();
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
        _, err := fmt.Scanf("%d %d\n", &a, &b)
        if err != nil {
            break
        }
        fmt.Println(a+b)
    }
}
```