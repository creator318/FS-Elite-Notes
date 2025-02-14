#CN/CIDR #CN/Subnetting 

In networking, a subnet is a portion of a network with a defined range of IP addresses.
Two subnets overlap if they share some common IP addresses. Given two network
IP addresses with their respective CIDR notations, write a program that determines
whether these subnets overlap.

Your program should take as input:
- IP1: The first subnet’s network address.
- CIDR1: The CIDR notation (prefix length) for the first subnet.
- IP2: The second subnet’s network address.
- CIDR2: The CIDR notation (prefix length) for the second subnet.

The program should return true if the subnets overlap and false otherwise.

Input Format:
-------------
A string IP1: The first network IP address (e.g., "192.168.1.0").
An integer CIDR1: The subnet mask prefix (e.g., 24 for /24).
A string IP2: The second network IP address (e.g., "192.168.1.128").
An integer CIDR2: The subnet mask prefix for the second subnet.

Output Format:
--------------
A boolean value, if the two subnets overlap or not.

Sample Input-1:
-------------
```
192.168.1.0
24
192.168.1.128
25
```

Sample Output-1:
--------------
```
true
```

Explanation:
------------
A /26 subnet has 64 IP addresses. The starting addresses of
the first four subnets are:
- 192.168.1.0
- 192.168.1.64
- 192.168.1.128
- 192.168.1.192

Sample Input-2:
-------------
```
10.0.0.0
16
10.1.0.0
16
```

Sample Output-2:
--------------
```
false
```

## Solution:

```c
#include <stdio.h>
#include <stdlib.h>

typedef union _ip_ {int bin; char seg[4];} ip;

void check_overlap(ip addr[], int plen[]) {
  int max = plen[0] > plen[1] ? 0 : 1;
  int min = max ^ 1;

  ip mask;
  mask.bin = 0xffffffff << (32 - plen[max]);

  ip minnetwork;
  minnetwork.bin = addr[min].bin & mask.bin;

  for (int i = 0; i < abs(plen[0] - plen[1]); i++) {
    ip network;
    network.bin = addr[min].bin & mask.bin | i << (32 - plen[max]);
    if (network.bin == minnetwork.bin) {
      printf("true\n");
      return;
    }
  }
  printf("false\n");
}

int main() {
  ip addr[2];
  int plen[2];
  for (int i = 0; i < 2; i++) {
    scanf("%hhu.%hhu.%hhu.%hhu", &addr[i].seg[3], &addr[i].seg[2], &addr[i].seg[1], &addr[i].seg[0]);
    scanf("%d", &plen[i]);
  }

  check_overlap(addr, plen);
}
```