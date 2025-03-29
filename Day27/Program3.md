Imagine you're managing a busy warehouse where every product is delivered in pairs to ensure proper stocking. However, due to a mix-up at the shipping dock, two unique product IDs ended up without their matching pair, while all other products arrived as complete pairs. Your task is to identify these two solitary product IDs.

You're given list of product IDs. In this list, every product ID appears exactly twice except for two IDs that appear only once. Return these two unique product IDs in any order.

You must design an algorithm that runs in linear time and uses only constant extra space.

Sample Input-1:
----------
```
101 102 101 103 102 105  
```

Sample Output-1: 
----------
```
[103, 105] 
```
 
Explanation:
----------
The IDs 103 and 105 appear only once, while all other IDs appear twice. \[105, 103] is also an acceptable answer.

Sample Input-2:
-----------
```
121 136
```

Sample Output-2:
----------
```
[121, 136] 
```

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    int vals[] = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
    
    System.out.println(findUnique(vals));
    
    sc.close();
  }

  private static List<Integer> findUnique(int[] vals) {
    int xor = 0;
    for (int id: vals) xor ^= id;

    int diffBit = xor & -xor;
    int unique[] = {0, 0};

    for (int id: vals) {
      if ((id & diffBit)!=0) unique[0] ^= id;
      else unique[1] ^= id;
    }

    return Arrays.asList(unique[0], unique[1]);
  }
}
```