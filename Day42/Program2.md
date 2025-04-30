#Coding/Greedy  

A digital advertising company is setting up electronic billboards across the city. Each billboard screen has dimensions of rows x cols, indicating how many lines (rows) and how many characters per line (cols) the screen can display. The company has prepared an advertising slogan consisting of several words, provided as a list of strings. The slogan must repeatedly appear on the billboard, word by word, maintaining the exact original order. Each word must fit entirely on a single line without breaking. Consecutive words on the same line must be separated by exactly one blank space.

Determine how many complete times the given advertising slogan can be displayed fully on the billboard screen.

Input format:
-------------
Line 1: Space seperated words, slogon
Line 2: Two space separated integers, rows & cols

Output format:
--------------
An integer, number of times the given advertising slogan can be displayed fully on the illboard screen.


Sample Input-1:
----------
```
fast cars
2 8
```

Sample Output-1:
----------
```
1
```

Explanation:  
----------
fast----  
cars----  
(The character '-' represents empty spaces on the screen.)


Sample Input-2:
----------
```
win big now
3 7
```

Sample Output-2:
----------
```
2
```

Explanation:  
----------
win-big  
now-win  
big-now  
(The character '-' represents empty spaces on the screen.)


Sample Input-3:
----------
```
eat fresh daily
4 6
```

Sample Output-3:
----------
```
1
```
 
Explanation:  
----------
eat---  
fresh-  
daily-  
eat---  
(The character '-' represents empty spaces on the screen.)

Constraints:
----------
- 1 <= slogan.length <= 1000
- Each word in slogan consists only of lowercase English letters.
- 1 <= rows, cols <= $2*10^4$

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    String words[] = sc.nextLine().split(" ");

    int r = sc.nextInt(), c = sc.nextInt();

    System.out.println(maxRepetitions(words, r, c));
    
    sc.close();
  }

  private static int maxRepetitions(String words[], int r, int c) {
    int count = 0;

    StringBuilder sb = new StringBuilder();
    for (String w: words) {
      if (w.length()>c) return 0;
      sb.append(w).append(" ");
    }

    String s = sb.toString();

    for (int i=0; i<r; i++) {
      count += c;

      if (s.charAt(count%s.length())==' ') count++;
      else
        while (count>0 && s.charAt((count-1)%s.length())!=' ') 
          count--;
    }

    return count / s.length();
  }
}
```