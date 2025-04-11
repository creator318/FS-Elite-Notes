
In a magical kingdom, each wizard carries a certain number of enchanted crystals. 
A pair of wizards is said to have a "dominant wizard" if the first wizard, who arrived earlier at the royal gathering, possesses more than twice as many crystals as the second wizard, who arrived later.

Given an list of crystals, representing the number of enchanted crystals carried by each wizard in their order of arrival at the gathering, calculate the number of "dominant wizard" pairs.

A pair of wizards (x, y) is considered dominant if:
- 0 <= x < y < crystals.length and
- crystals\[x] > 2 × crystals\[y]

Sample Input-1:
--------
```
1 3 2 3 1
```

Sample Output-1:
--------
```
2
```

Explanation:
--------
The dominant wizard pairs are:
- Wizard 1 (3 crystals) and Wizard 4 (1 crystal), since 3 > 2 × 1
- Wizard 3 (3 crystals) and Wizard 4 (1 crystal), since 3 > 2 × 1

Sample Input-2:
--------
```
2 4 3 5 1
```

Sample Output-2:
--------
```
3
```

Explanation:
--------
The dominant wizard pairs are:
- Wizard 1 (4 crystals) and Wizard 4 (1 crystal), since 4 > 2 × 1
- Wizard 2 (3 crystals) and Wizard 4 (1 crystal), since 3 > 2 × 1
- Wizard 3 (5 crystals) and Wizard 4 (1 crystal), since 5 > 2 × 1

Constraints:
--------
- 1 <= crystals.length <= $5 × 10^4$
- $-2^{31}$ <= crystals\[i] <= $2^{31} - 1$

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    
    int crystals[] = Arrays.stream(sc.nextLine().split(" ")).mapToInt(Integer::parseInt).toArray();
    
    System.out.println(countDominantWizardPairs(crystals));

    sc.close();
  }
  
  private static int countDominantWizardPairs(int[] crystals) {
    if (crystals==null || crystals.length<=1) return 0;
        
    return mergeSort(crystals, 0, crystals.length-1);
  }
  
  private static int mergeSort(int[] arr, int l, int r) {
    int count = 0;
    
    if (l<r) {
      int m = l + (r-l)/2;
      
      count += mergeSort(arr, l, m);
      count += mergeSort(arr, m+1, r);
      
      count += merge(arr, l, r);
    }
    
    return count;
  }
  
  private static int merge(int[] arr, int l, int r) {
    int m = l + (r-l)/2;

    int left[] = new int[m-l+1], right[] = new int[r-m];
    
    for (int i=0; i<left.length; i++) left[i] = arr[l+i];
    for (int i=0; i<right.length; i++) right[i] = arr[m+i+1];
    
    int count = 0;
    for (int i=0; i<left.length; i++) {
      int j = 0;
      while (j<right.length && left[i]>2L*right[j]) j++;
      count += j;
    }
    
    int i = 0, j = 0, k = l;
    while (i<left.length && j<right.length) {
      if (left[i]<=right[j]) arr[k++] = left[i++];
      else arr[k++] = right[j++];
    }
    
    while (i<left.length) arr[k++] = left[i++];
    while (j<right.length) arr[k++] = right[j++];
    
    return count;
  }
}
```