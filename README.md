# 1. CIDR Blocks/Prefixes

## What is CIDR?

CIDR, or Classless Inter-Domain Routing, is a compact notation used to represent a range of IP addresses.

It is written as an IP address followed by a slash and a prefix length, such as 10.0.0.0/16.

The prefix length tells how many leading bits are fixed, while the remaining bits can vary to create the address range.

Since IPv4 addresses have 32 bits and each octet has 8 bits, the total number of addresses in a CIDR block is calculated with 2^(32 − prefix length).

## CIDR Notation

A.B.C.D/x

- A.B.C.D is an IP address.
- / is the slash character.
- x is a decimal number called the prefix length.
  
For example, instead of writing:

**10.0.0.0 to 10.0.255.255**

we can write:

10.0.0.0/16

## Octets and IPv4 Bits

An **octet** is a group of 8 bits. IPv4 addresses have four octets:

**10 . 0 . 0 . 0**

Each octet contains 8 bits, so IPv4 has a total of:

**4 × 8 = 32 bits**

In the example **10.0.0.0/16**, the first two octets are fixed because the prefix length is 16. That means the first 16 bits are the network portion, and the remaining 16 bits are available for hosts. So the range becomes:

**10.0.0.0 – 10.0.255.255**

## How Many IP Addresses Are in a CIDR Block?



