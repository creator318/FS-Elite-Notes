#CN/IPv4Classes

Write a program that determines the class of a given IPv 4 address. 
The classification follows the standard IP address classes:
- Class A: IP addresses where the first octet is in the range 1-127.
- Class B: IP addresses where the first octet is in the range 128-191.
- Class C: IP addresses where the first octet is in the range 192-223.
- Multicast (Class D): IP addresses where the first octet is in the range 224-239.

Input Format:
-------------
A string IP: The first network IP address (e.g.,0-239).

Output Format:
--------------
Class that the given IP address belongs to


Sample Input-1:
-------------
192.168.1.0

Sample Output-1:
--------------
```
Class C
```

Explanation:
------------
The first octet 192 is within the Multicast range.

## Solution:

```c
#include <stdio.h>
#include <stdlib.h>

typedef union _ip_ {int bin; char seg[4];} ip;

void class(ip addr) {
  if (addr.seg[0]<=127) printf("Class A");
  else if (addr.seg[0]<=191) printf("Class B");
  else if (addr.seg[0]<=223) printf("Class C");
  else if (addr.seg[0]<=239) printf("Multicast");
  // else printf("Reserved");

  printf("\n");
}

int main() {
  ip addr;
  scanf("%hhu.%hhu.%hhu.%hhu", &addr.seg[3], &addr.seg[2], &addr.seg[1], &addr.seg[0]);

  class(addr);
}
```