#CN/CIDR #CN/Subnetting

Write a program that takes an IP address and subnet mask (in CIDR notation, 
e.g., 192.168.1.1/24) as input and calculates the network and broadcast addresses.

Input Format:
---------------
A String, IPAddress
An integer, CIDR

Output Format:
---------------
Space separated IP addresses, network IP and broadcast IP.

Sample Input-1:
-----------------
```
192.168.1.10
24
```

Sample Output-1:
------------------
```
192.168.1.0 192.168.1.255
```


Sample Input-2:
-----------------
```
192.0.2.1
24
```

Sample Output-2:
------------------
```
192.0.2.0 192.0.2.255
```

## Solution:

```c
#include <stdio.h>

typedef union _ip_ {int bin; char seg[4];} ip;

void print_ip(ip addr) {
    printf("%hhu.%hhu.%hhu.%hhu\n", addr.seg[3], addr.seg[2], addr.seg[1], addr.seg[0]);
}

void plen_to_network_bcast(ip addr, int plen) {
    ip mask;
    mask.bin = 0xffffffff << (32-plen);
    
    ip network, bcast;
    network.bin = addr.bin & mask.bin;
    bcast.bin = network.bin | ~mask.bin;
    
    print_ip(network);
    print_ip(bcast);
}

int main () {
    ip addr;
    scanf("%hhu.%hhu.%hhu.%hhu", &addr.seg[3], &addr.seg[2], &addr.seg[1], &addr.seg[0]);
    
    int plen=0;
    scanf("%d", &plen);
    
    plen_to_network_bcast(addr, plen);
}
```
