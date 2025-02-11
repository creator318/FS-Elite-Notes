#CN #CN/Subnetting

Write a program that takes an IP address and subnet mask (in CIDR notation, 
e.g., 192.168.1.1/24) as input and calculates the subnet mask in dotted decimal 
Format.

Input Format:
---------------
An integer, CIDR

Output Format:
---------------
String, Subnet's IP Address


Sample Input-1:
-----------------
```
25
```

Sample Output-1:
------------------
```
255.255.255.128
```


Sample Input-2:
-----------------
```
22
```

Sample Output-2:
------------------
```
255.255.252.0
```

## Solution:

```c
#include <stdio.h>

typedef union _ip_ {int bin; char seg[4];} ip;

void plen_to_mask(int plen) {
    ip subnet;
    subnet.bin = 0xffffffff << (32-plen);
    
    printf("%hhu.%hhu.%hhu.%hhu", subnet.seg[3], subnet.seg[2], subnet.seg[1], subnet.seg[0]);
}

int main() {
    int plen=0;
    scanf("%d", &plen);
    
    plen_to_mask(plen);
}
```