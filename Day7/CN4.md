#CN/CIDR #CN/Subnetting 

In computer networking, subnetting is the process of dividing a larger IP network
into multiple smaller subnetworks (subnets). Given a base network IP address, 
a CIDR (Classless Inter-Domain Routing) prefix, and the number of subnets required, 
write a Java program that calculates the starting addresses of all the subnets.

Your program should take as input:
- A base network address (e.g., 192.168.1.0).
- A CIDR prefix (e.g., /26 means a subnet mask of 255.255.255.192).
- The number of subnets to generate.

The program should then compute and return the starting addresses of each subnet.

Input Format:
-------------
A string NIP: The base network IP address (e.g., "192.168.1.0").
An integer CIDR: The subnet mask prefix (e.g., 26 for /26).
An integer numSubnets: The number of subnets to be generated.

Output Format:
--------------
A list of subnet addresses, each representing the starting address of a subnet.


Sample Input-1:
-------------
```
192.168.1.0
26
4
```

Sample Output-1:
--------------
```
192.168.1.0/28
192.168.1.16/28
192.168.1.32/28
192.168.1.48/28
```

Explanation:
------------
A /26 subnet has 64 IP addresses. The starting addresses of 
the first four subnets are:
- 192.168.1.0/28, 
- 192.168.1.16/28, 
- 192.168.1.32/28, 
- 192.168.1.48/28

# Solution:

```c
#include <math.h>
#include <stdio.h>

typedef union _ip_ {int bin; char seg[4];} ip;

void print_cidr_ip(ip addr, int plen) {
  printf("%hhu.%hhu.%hhu.%hhu/%d\n", addr.seg[3], addr.seg[2], addr.seg[1], addr.seg[0], plen);
};

void subnets(ip addr, int plen, int nnets) {
  int mext = 0;
  while (pow(2, mext)<nnets) mext++;
  nnets = pow(2, mext);

  plen += mext;
  ip mask;
  mask.bin = 0xffffffff << (32 - plen);

  for (int i=0; i<nnets; i++) {
    ip network;
    network.bin = addr.bin & mask.bin | i << (32-plen);
    print_cidr_ip(network, plen);
  }
}

int main() {
  ip addr;
  scanf("%hhu.%hhu.%hhu.%hhu", &addr.seg[3], &addr.seg[2], &addr.seg[1], &addr.seg[0]);

  int plen;
  scanf("%d", &plen);

  int nnets;
  scanf("%d", &nnets);

  subnets(addr, plen, nnets);
}
```