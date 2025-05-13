#Coding/Greedy 

Venkatadri is a maths teacher. He is teaching matrices to his students. He is given a matrix of size m*n, and it contains only positive numbers. He has given a task to his students to find the special matrix, in the iven matrix A[m][n].
A special matrix has following property:
- The sum of elements in each row, each column and the two diagonals are equal.
- Every 1*1 matrix is called as a special matrix.
- The size of the special matrix should be a square, i.e., P*P.

Your task is to help the students to find the speical matrix with max size P.


Input Format:
-------------
Line-1: Two space separated integers M and N, size of the matrix.
Next M lines: N space separated integers m and n.

Output Format:
--------------
Print an integer, maximum size P of the special matrix.


Sample Input-1:
---------------
```
5 5
7 8 3 5 6
3 5 1 6 7
3 5 4 3 1
6 2 7 3 2
5 4 7 6 2
```

Sample Output-1:
----------------
```
3
```

Explanation:
------------
The special square is:
```
5 1 6
5 4 3
2 7 3
```


Sample Input-2:
---------------
```
4 4
7 8 3 5
3 2 1 6
3 2 3 3
6 2 3 3
```

Sample Output-2:
----------------
```
2
```

Explanation:
------------
The special square is:
```
3 3
3 3
```

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int m = sc.nextInt(), n = sc.nextInt();

    int[][] mat = new int[m][n];
    for (int i=0; i<m; i++)
      for (int j=0; j<n; j++)
        mat[i][j] = sc.nextInt();

    System.out.println(largestSpecialMatrix(mat));

    sc.close();
  }

  private static int largestSpecialMatrix(int[][] mat) {
    int m = mat.length, n = mat[0].length, maxSide = Math.min(m, n);

    for (int size=maxSide; size>0; size--) {
      for (int i=0; i<=m-size; i++) {
        for (int j=0; j<=n-size; j++) {
          if (isSpecial(mat, i, j, size)) return size;
        }
      }
    }

    return 1;
  }

  private static boolean isSpecial(int[][] mat, int i, int j, int size) {
    int sum = 0;

    for (int k=j; k<j+size; k++) sum += mat[i][k];
  
    for (int k=i+1; k<i+size; k++) {
      int rowSum = 0;
      for (int l=j; l<j+size; l++) rowSum += mat[k][l];
      if (rowSum!=sum) return false;
    }

    for (int k=j; k<j+size; k++) {
      int colSum = 0;
      for (int l=i; l<i+size; l++) colSum += mat[l][k];
      if (colSum!=sum) return false;
    }

    int diagonalSum = 0;
    for (int k=0; k<size; k++) diagonalSum += mat[i+k][j+k];
    if (diagonalSum!=sum) return false;

    diagonalSum = 0;
    for (int k=0; k<size; k++) diagonalSum += mat[i+k][j+size-1-k];
    if (diagonalSum!=sum) return false;

    return true;
  }
}
```