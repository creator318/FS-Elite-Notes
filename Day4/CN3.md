#CN/CIDR #CN/Subnetting

Given two IP addresses IP 1 and IP 2, and a subnet mask, your task is to check 
Whether IP-1 and IP-2 belongs to same host range or not.

Input Format:
---------------
Two space separated strings, IP 1 and IP 2.
An integer, CIDR (subnet mask).

Output Format:
---------------
A boolean result.

Sample Input-1:
-----------------
```
192.168.1.10 192.168.1.20
24
```

Sample Output-1:
------------------
```
true
```


Sample Input-2:
-----------------
```
192.0.2.1 192.0.3.253
24
```

Sample Output-2:
------------------
```
false
```

## Solution:

```c
#include <stdio.h>

typedef union _ip_ {int bin; char seg[4];} ip;

void compare_networks(ip a, ip b, int plen) {
    ip mask;
    mask.bin = 0xffffffff << (32-plen);
    
    ip neta, netb;
    neta.bin = a.bin & mask.bin;
    netb.bin = b.bin & mask.bin;
    
    printf((neta.bin == netb.bin) ? "true\n" : "false\n");
}

int main() {
    ip a, b;
    scanf("%hhu.%hhu.%hhu.%hhu", &a.seg[3], &a.seg[2], &a.seg[1], &a.seg[0]);
    scanf("%hhu.%hhu.%hhu.%hhu", &b.seg[3], &b.seg[2], &b.seg[1], &b.seg[0]);
    
    int plen=0;
    scanf("%d", &plen);
    
    compare_networks(a, b, plen);
}
```