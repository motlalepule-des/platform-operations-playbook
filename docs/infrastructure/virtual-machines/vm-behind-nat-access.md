
# VM Behind NAT Access

## Purpose
Understand how to access services hosted on a VM behind a router or NAT device.

## Problem
A VM may have a private IP such as `10.168.1.40` which is not reachable directly from the internet.

## Requirements
- Public IP on the router or firewall
- Port forwarding configured
- Firewall rule allowing the traffic
- Service listening on the expected port

## Checklist
- SQL Server TCP/IP enabled
- SQL Server listening on port 1433
- Windows Firewall allows 1433
- Router forwards 1433 to VM private IP
- External firewall or ISP does not block 1433

## Verification
Test using the public IP, not the private IP.