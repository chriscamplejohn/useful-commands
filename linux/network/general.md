# Intro

These are commands I have found useful for working with networks on Linux.

## Glossary

| Term | Description |
| ---- | ----------- |
| CIDR | Classless Inter-Domain Routing - Routing using the classless system (any unknown address is sent via the default route) |
| CIDR Notation | Also known as slash notation - used to specify the subnet mask for CIDR routers |

## Tools

| Tool | Description |
| ---- | ----------- |
| []

### Discover IPv4 devices on the network

Change the start address and CIDR (-r) as required

```netdiscover -r 10.0.2.1/24``

### Network ping scan



```nmap -sn 192.168.1.1/24```