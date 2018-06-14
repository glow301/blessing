## Copying Books 

### Description
Before the invention of book-printing, it was very hard to make a copy of a book. All the contents had to be re-written by hand by so called scribers. The scriber had been given a book and after several months he finished its copy. One of the most famous scribers lived in the 15th century and his name was Xaverius Endricus Remius Ontius Xendrianus (Xerox). Anyway, the work was very annoying and boring. And the only way to speed it up was to hire more scribers. 

Once upon a time, there was a theater ensemble that wanted to play famous Antique Tragedies. The scripts of these plays were divided into many books and actors needed more copies of them, of course. So they hired many scribers to make copies of these books. Imagine you have m books (numbered 1, 2 ... m) that may have different number of pages (p1, p2 ... pm) and you want to make one copy of each of them. Your task is to divide these books among k scribes, k <= m. Each book can be assigned to a single scriber only, and every scriber must get a continuous sequence of books. That means, there exists an increasing succession of numbers 0 = b0 < b1 < b2, ... < b k-1 <= bk = m such that i-th scriber gets a sequence of books with numbers between bi-1+1 and bi. The time needed to make a copy of all the books is determined by the scriber who was assigned the most work. Therefore, our goal is to minimize the maximum number of pages assigned to a single scriber. Your task is to find the optimal assignment.

### Input
The input consists of N cases. The first line of the input contains only positive integer N. Then follow the cases. Each case consists of exactly two lines. At the first line, there are two integers m and k, 1 <= k <= m <= 500. At the second line, there are integers p1, p2, ... pm separated by spaces. All these values are positive and less than 10000000.

### Output
For each case, print exactly one line. The line must contain the input succession p1, p2, ... pm divided into exactly k parts such that the maximum sum of a single part should be as small as possible. Use the slash character ('/') to separate the parts. There must be exactly one space character between any two successive numbers and between the number and the slash. 

If there is more than one solution, print the one that minimizes the work assigned to the first scriber, then to the second scriber etc. But each scriber must be assigned at least one book.

### Simple Input
2  
9 3  
100 200 300 400 500 600 700 800 900  
5 4  
100 100 100 100 100  

### Simple Output
100 200 300 400 500 / 600 700 / 800 900  
100 / 100 / 100 / 100 100  

### 问题分析
1. 假设每个人分到的最合适的大小是 m，那么 m 的取值区间一定是 [min(books), sum(books)]，而 m 在这个区间内是单调递增的，所以可以采用二分法求解。
1. 建立一个`分块大小`和`所需人数`的函数关系，目标是求得，在给定人数的条件下，求分块 m 的最小值。
1. 按照合适的大小，输出结果

* 目前代码提交，会报 wrong answer 的错误，本地用测试输出正常，可能有边界情况没有考虑到。
```java
import java.util.Scanner;

public class Main{
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        int line = scan.nextInt();
        while (line-- > 0) {
            int booksCount = scan.nextInt();
            int scribers   = scan.nextInt();
            int[] books    = new int[booksCount];

            for (int i = 0; i < booksCount; i++) {
                books[i] = scan.nextInt();
            }
            int partSize = findPartSize(books, scribers);
            outputRes(books, partSize, scribers);
        }
    }

    public static void outputRes(int[] books, int size, int scribers) {
        int length = books.length;
        int sum = 0, count = 0;
        for (int i = 0; i < length-1; i++) {
            sum += books[i];
            System.out.print(books[i] + " ");
            if (count < scribers-1 && sum <= size && sum + books[i] > size) {
                sum = 0;
                count++;
                System.out.print("/ ");
            }
        }
        System.out.print(books[length - 1]);
        System.out.println();
    }

    // 找到一个合适的分块大小
    public static int findPartSize(int[] books, int scribers) {
        int left = getMin(books), right = getSum(books), mid = 0; 
        int part  = 0;
        while (left <= right) {
            mid  = left + (right - left) / 2;
            part = getPartNum(books, mid);
            if (part > scribers) {
                left = mid + 1;
            } else {
                if (left == mid) {
                    break;
                }
                right = mid;
            }
        }
        return mid;
    }

    // 找到数组中最小值，作为给定区间的下限
    public static int getMin(int[] books) {
        int min = books[0], length = books.length;
        for (int i = 0; i < length; i++) {
            if (min < books[i]) {
                min = books[i];
            }
        }
        return min;
    }

    // 求得数组的和，作为给定区间的上限
    public static int getSum(int[] books) {
        int length = books.length;
        int sum    = 0;

        for (int i = 0; i < length; i++) {
            sum += books[i];
        }
        return sum;
    }

    // 计算给定的大小，可以分为多少部分
    public static int getPartNum(int[] books, int size) {
        int sum  = 0;
        int part = 0;
        for (int i = 0; i < books.length; i++) {
            sum += books[i];
            if (sum > size && sum - books[i] <= size) {
                part++;
                sum = books[i];
            }
        }
        return part;
    }
}
```