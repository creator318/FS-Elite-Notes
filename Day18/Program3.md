#Coding/TwoPointer #Coding/Strings 

"Emphatic Pronunciation" of a given word is where we take the word and replicate some of the letter to emphasize their impact.

Instead of saying 'oh my god', someone may say "ohhh myyy goddd", We define emphatic pronunciation of a word, which is derived by replicating a group (or single) of letters in the original word. 

So that the replicated group is atleast 3 characters or more and greater than or equal to size of original group. 
For example Good -> Goood is an emphatic pronunciation, but Goodd is not because in Goodd the 'd' are only occuring twice consecutively.
 
In the question you are given the "Emphatic pronunciation" word, you have to findout how many words can legal result in the "emphatic pronunciation" word.

Input Format:
-------------
Line-1 -> A String contains a single word, Emphatic Pronunciation word
Line-2 -> Space seperated word/s

Output Format:
--------------
Print an integer as your result


Sample Input-1:
---------------
```
goood
good goodd
```

Sample Output-1:
----------------
```
1
```


Sample Input-2:
---------------
```
heeelllooo
hello hi helo
```

Sample Output-2:
----------------
```
2
```

## Solution:

```java
import java.util.*;

public class Solution {
  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);

    String em = sc.nextLine();
    String words[] = sc.nextLine().split(" ");

    System.out.println(countExpressive(em, words));

    sc.close();
  }

  private static int countExpressive(String em, String[] words) {
    int count = 0;
    for (String word : words) if (isEmpathetic(em, word)) count++;
    return count;
  }

  private static boolean isEmpathetic(String em, String word) {
    int i=0, j=0;
    while (i<em.length() && j<word.length()) {
      if (em.charAt(i)==word.charAt(j)) {
        int emlen = getRepeatedLength(em, i), wlen = getRepeatedLength(word, j);
        if ((emlen<3 && emlen!=wlen) || (emlen>=3 && emlen<wlen)) return false;
        i += emlen; j += wlen;
      } else return false;
    }
    return i==em.length() && j==word.length();
  }

  private static int getRepeatedLength(String str, int i) {
    int j = i;
    while (j < str.length() && str.charAt(j) == str.charAt(i)) j++;
    return j - i;
  }
}
```

