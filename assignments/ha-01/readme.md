# Homework Assignment 1 (due Feb 12th 11:59pm)

This assignment takes you on a deep dive into **binary files**, 
**number systems**, and the power of of **bitwise operators** in `C`.  

The assignment is worth a total of 100 points.  Feel free to drop by our 
office hours or post your questions directly on the `Ed Discussion` forum.

## 1. HexDump (45 pts)
Write a program that reads in a *binary file* and dumps its contents 
to the *standard output*.  It should work similar to the `hexdump` utility.  

> [!TIP]
> Go over the [example code](http://www.cplusplus.com/reference/cstdio/fread/) 
> within the `fread` documentation.  Master the following functions `fopen/fclose`,
> `fseek/ftell/rewind`, and `malloc/free`.

Take notice of the general advise for file manipulation:

- create pointers for both the input file and the buffer in memory
- open the file in `rb` mode and check for successful file opening
- determine file size and allocate memory accordingly
- load the file contents into the buffer
- free the memory dynamically allocated   

### Input
Your program will receive the following command line arguments:
```text
<fname> File name for the binary file
<N>     Number of bytes to show on each row (N>0)
<base>  Base for displaying the binary data
        - 'x' for lowercase hexadecimal
        - 'd' for unsigned integer
```

The line below shows an example of invoking your program upon a file 
named `file.bin`, revealing 16 bytes per row in lowercase hexadecimal
form:
```bash
$ ./hexdump file.bin 16 x
```

### Output
Your program should output the contents of the binary file to the 
`stdout` using the following instructions:

- each row of the output displays the index of the first byte, followed
  by a maximum of `N` bytes using the notation indicated by `<base>`
- the index is displayed as an 8-digit hexadecimal number padded with
  zeros on the left

Observe the examples below, revealing the 20 bytes within `file.bin`.

```bash
$ ./hexdump file.bin 16 x
00000000 cf fa ed fe 07 00 00 01 03 00 00 00 02 00 00 00
00000010 0e 00 00 00
$
$ ./hexdump file.bin 8 x
00000000 cf fa ed fe 07 00 00 01 
00000008 03 00 00 00 02 00 00 00
00000010 0e 00 00 00
```

## `2. IPv4 Addresses (55 pts)`
In this problem, your program will decode a databank of *IP addresses* and 
their respective *subnet masks*.  

Your mission:

- **Open the databank:** Read in the binary file containing the encoded addresses
- **Decode the binary data:** Each 5-byte sequence stores an IP address using 4 bytes
  and the number of bits in the subnet mask using 1 byte
- **Display the information:** Output the following for each pair (IP, mask):
  - IP address
  - subnet mask
  - network address
  - the usable IP range for hosts

### What is an IP address?
An IP address is a 32-bit number used to uniquely identify a device on a 
TCP/IP network.  It can be written in two ways:

- *dotted-decimal format:* four decimal numbers separated by periods,
  e.g., `131.128.81.111`
- *binary format:* a 32-bit sequence of bits, e.g., `10000011 10000000 01010001 01101111`

To convert between formats:

- *binary to dotted-decimal:* divide the bit sequence into 4 8-bit sections (`octects`)
  and convert each octect to decimal
- *dotted-decimal to binary:* convert each decimal number to a sequence of 8-bits
  (padded with zeros) and combine them

### Network and Host Addresses

IP addresses have two parts:

- *network address:* identifies an specific network within the *TCP/IP network*
- *host address:* identifies an specific device within that network

Packets (data) are first routed to the destination network using the network address, 
and then delivered to the correct device using the host address.

### Subnet Mask

A `subnet mask` is a 32-bit number used to split an IP address into its network 
address and host address.  This mask consists of consecutive 1s followed by
consecutive 0s,  It can be expressed in dotted-decimal format just like an IP,
e.g., `11111111 11111111 11111111 00000000` is represented by `255.255.255.0`.  

> [!NOTE]
> The 1s indicate the bits used for the network address, while the 0s indicate
> the bits used for the host address.  Bitwise operations between the subnet
> mask and the IP address extract these parts.

### CIDR Notation

A compact way to express an IP address and its subnet mask.  In this notation, 
an IP address is followed by '/' and a decimal number.  The decimal number is 
the count of leading 1s in the subnet mask.  For example, in `131.128.81.111/24` 
24 indicates the count of leading 1s in the mask.

### From Binary Encoding to IP and Network Addresses

The illustration below shows a detailed example.  Note that the `binary encoding` 
in the figure is the representation used to store IP addresses in the binary 
databank.

![IPv4 addresses and subnet masks](ipv4-encoding.jpg)

### Host Range Calculation

The following figure shows a how to calculate the range of available host addresses, 
given an IP address and a subnet mask.  Note that the first and last addresses of the 
available range are not considered valid host addresses as they are reserved as 
`network address` and `broadcast address`. 

![Available host range](host-range.jpg)

> [!TIP]
> When working on this assignment you might find useful this
> [IP subnet calculator](https://www.calculator.net/ip-subnet-calculator.html)
> that can give you the correct calculations.

### Input
Your program will receive the following command line arguments:

```text
<fname> File name for the binary file
```

The line below shows an example of invoking your program upon a file 
named `file.bin`.

```bash
$ ./decode-ipv4 file.bin
```

### Output
Your program should output the databank information to the `stdout`.  For each 
consecutive 5 bytes in the binary file, display the corresponding IP/Subnet information.  
Each chunk of 5 bytes encodes an IP (4 bytes) followed by the number of 1s in the 
subnet mask (1 byte).

Observe the example below, revealing the IPs encoded in `file.bin` (file size is 15 bytes).

```bash
$ ./decode-ipv4 file.bin
IP address:        222.110.11.20
Subnet mask:       255.255.255.192
Usable IP range:   222.110.11.1 - 222.110.11.62
Network address:   222.110.11.0
Broadcast address: 

IP address:        148.31.27.2
Subnet mask:       255.255.255.0
Usable IP range:   148.31.27.1 - 148.31.27.254
Network address:   148.31.27.0
Broadcast address: 

IP address:        32.10.10.1
Subnet mask:       255.252.0.0
Usable IP range:   32.8.0.1 - 32.11.255.254
Network address:   32.8.0.0
Broadcast address: 
```

## Submission and Grading

This assignment uses an autograder. For each of the questions you either pass 
the test cases (full points) or not (zero points). To ensure compatibility 
with the autograder, your program must follow the specifications provided 
above, in particular, using the exact input/output requirements.

> [!IMPORTANT]
> You are **required** to provide meaningful comments and use proper
> coding style and indentation.

You will submit two files named `hexdump.c` and `decode-ipv4.c` via 
Gradescope.  The autograder runs `gcc` on a `linux` machine.  It is 
imperative that you ensure your code compiles and executes properly 
with the `gcc` compiler.

> [!CAUTION]
> Remember, academic integrity is of utmost importance.  Any attempts at
> cheating or plagiarism will result in a forfeiture of credit.  Potential
> further actions include failing the class or referring the case for
> disciplinary measures.
