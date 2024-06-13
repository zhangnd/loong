# 华为OD机试 - 螺旋数字矩阵

https://fcqian.blog.csdn.net/article/details/135085069

## 题目描述

疫情期间，小明隔离在家，百无聊赖，在纸上写数字玩。他发明了一种写法：

给出数字个数 n （0 < n ≤ 999）和行数 m（0 < m ≤ 999），从左上角的 1 开始，按照顺时针螺旋向内写方式，依次写出2,3,....,n，最终形成一个 m 行矩阵。

小明对这个矩阵有些要求：

1. 每行数字的个数一样多

2. 列的数量尽可能少

3. 填充数字时优先填充外部

4. 数字不够时，使用单个 * 号占位

## 输入描述

两个整数，空格隔开，依次表示 n、m

## 输出描述

符合要求的唯一矩阵

## 用例

| 输入 | 9 4 |
| ---- | ---- |
| 输出 | 1 2 3<br>* * 4<br>9 * 5<br>8 7 6 |
| 说明 | 9个数字写出4行，最少需要3列 |

| 输入 | 3 5 |
| ---- | ---- |
| 输出 | 1<br>2<br>3<br>* <br>* |
| 说明 | 3个数字写5行，只有一列，数字不够用*号填充 |

## 题目解析

本题需要我们将1~n数字按照螺旋顺序填入矩阵。

本题只给出了矩阵的行数m，没有给列数，需要我们求解一个最少的列数来满足矩阵能够填入n个数字，因此列数 k = ceil(n / m)，这里的除法不是整除，并且要对除法的结果向上取整。

将数字1~n按照螺旋顺序，从矩阵matrix左上角开始填入，比较考察思维能力，具体实现如下：

- 定义变量step，初始step=1，表示当前要填入的数字，因此step ≤ n

- 定义变量x，y，初始x=0, y=0，表示要填值得矩阵位置，即初始时从矩阵左上角开始填入

然后按照顺序循环进行下面四个填值操作

1. 正序填入第X行

2. 正序填入第Y列

3. 倒序填入第X行

4. 倒序填入第Y列

## Java算法源码

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt(); // 120
        int m = scanner.nextInt(); // 7
        scanner.close();
        int k = (int)Math.ceil((double)n / m);
        int step = 1;
        int x = 0;
        int y = 0;
        int[][] arr = new int[m][k];
        while (step <= n) {
            while(y < k && step <= n && arr[x][y] == 0) {
                arr[x][y++] = step++;
            }
            y -= 1;
            x += 1;
            while (x < m && step <= n && arr[x][y] == 0) {
                arr[x++][y] = step++;
            }
            x -= 1;
            y -= 1;
            while (y >= 0 && step <= n && arr[x][y] == 0) {
                arr[x][y--] = step++;
            }
            y += 1;
            x -= 1;
            while (x > 0 && step <= n && arr[x][y] == 0) {
                arr[x--][y] = step++;
            }
            x += 1;
            y += 1;
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < k; j++) {
                System.out.print((arr[i][j] == 0 ? "*" : arr[i][j]) + " ");
            }
            System.err.println("\n");
        }
    }
}
```
