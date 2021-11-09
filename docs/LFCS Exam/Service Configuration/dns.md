---
title: Maintain a DNS zone
date: 2021-11-09
authors:
  - Muse Sisay
---

- DNS server can be authoritative or not.
- DNS serves can be Forwarding or Recursive DNS.

## Forwarding vs Recursive DNS
### Forwarding DNS
- forwards the request to an upstream or configured dns server.
### Recursive DNS 
- Mean while  will traverse the path of the domain across the Internet to deliver the answer to the question.

1. Your recursive server will send a query to the DNS root servers: "Who is handling `.net`?”
2. The root server answers with a referral to the TLD servers for `.net`.
3. Your recursive server will send a query to one of the TLD DNS servers for `.net`: "Who is handling `pi-hole.net`?”
4. The TLD server answers with a referral to the authoritative name servers for `pi-hole.net`.
5. Your recursive server will send a query to the authoritative name servers: “What is the IP of `pi-hole.net`?”
6. The authoritative server will answer with the IP address of the domain `pi-hole.net`.
7. Your recursive server will then cache answer to its request.

Example taken from [Pi-hole unbound setup](https://docs.pi-hole.net/guides/dns/unbound/)