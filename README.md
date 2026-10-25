# Networking ‐ DNS Management

# 1. What is DNS?

The main functionality of DNS is to translate easy-to-remember symbolic domain names into IP addresses.

## Two Types of DNS Servers

| Type | Role |
|------|------|
| Recursive (Non-Authoritative) | The DNS server in the middle — queries other servers on your behalf. |
| Authoritative | The servers on the right — they actually hold the records for a domain. |

> Key point: The servers on the right are authoritative and not recursive, whereas the DNS server in the center is recursive.

How DNS Resolution Works

Let's say you're operating a laptop (or a compute instance) with a browser, and you want to visit `www.example.com`.

The machine doesn't know what `www.example.com` is, so here's what happens:

1. The browser queries the DNS server (a recursive/non-authoritative server).
2. The DNS server reaches out to the root servers (`.`) and asks: "What is the IP for www.example.com?"
3. The root server responds: "Ask `.com`."
4. The DNS server asks the TLD (Top Level Domain) servers for `.com`: "What is the IP of www.example.com?"
5. The TLD server responds: "Ask example.com."
6. The DNS server reaches out to the customer's DNS servers — which are authoritative and actually hold the records.
7. The authoritative DNS server returns the answer — for example, the IP address for `www.example.com`.
8. The recursive DNS server returns this IP to the browser, and now you can see the contents of `example.com`.




