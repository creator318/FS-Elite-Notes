#Coding/BitManipulation 

Imagine you're designing a sequence of signals for a high-tech robot. The robot receives its instructions as list of integers, where each integer represents one byte of the command data. A complete command can be composed of 1 to 4 bytes, following these strict rules:

- For a 1-byte command, the first bit must be 0, followed by the command's code.
- For a multi-byte command (with n bytes), the first byte starts with n consecutive 1’s, immediately followed by a 0. Each of the following n – 1 bytes ust begin with the bit pattern 10.

This is how the robot interprets the byte sequences:

| Number of Bytes | Robot Signal Sequence (binary)      |
| --------------- | ----------------------------------- |
| 1               | 0xxxxxxx                            | 
| 2               | 110xxxxx 10xxxxxx                   |
| 3               | 1110xxxx 10xxxxxx 10xxxxxx          |
| 4               | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx |

Here, each x represents a bit that can be either 0 or 1.

Note: Only the least significant 8 bits of each integer in the input list of integers are used, meaning each integer stands for exactly one byte of data.

Your challenge is to verify whether the provided sequence of robot instructions strictly follows the defined encoding rules.

Sample Imput-1:
----------
```
197 130 1
```

Sample Output-2:
----------
```
true
```

Explanation: 
----------
- The array corresponds to the binary sequence: 11000101 10000010 00000001.  
- This is a valid encoding: a 2-byte command (11000101 10000010) followed by a valid 1-byte command (00000001).

Sample Input-2:
----------
```
234 140 4
```

Sample Output-2:
----------
```
false
```

Explanation:
----------
- The array corresponds to the binary sequence: 11101011 10001100 00000100.  
- The first three bits of the first byte are 1’s with a following 0, indicating a 3-byte command. The second byte starts correctly with 10, but the third byte does not begin with 10, so the command sequence is invalid.

Constraints:
----------
- 1 <= instructions.length <= $2 * 10^4$
- 0 <= instruction <= 255

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    int cmds[] = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
    
    System.out.println(isValid(cmds));
    
    sc.close();
  }
  
  private static boolean isValid(int[] cmds) {
    int n = cmds.length, i = 0;

    while (i < n) {
      int first = cmds[i], byteCount;
      
      if ((first & 0b10000000) == 0) byteCount = 1;
      else if ((first & 0b11100000) == 0b11000000) byteCount = 2;
      else if ((first & 0b11110000) == 0b11100000) byteCount = 3;
      else if ((first & 0b11111000) == 0b11110000) byteCount = 4;
      else return false;

      if (i+byteCount > n) return false;

      for (int j=1; j<byteCount; j++)
        if ((cmds[i+j] & 0b11000000) != 0b10000000)
          return false;

      i += byteCount;
    }

    return true;
  }
}
```