# Port Forwarding

## Purpose
Expose a service hosted on a private VM or server behind NAT.

## Example
Forward public port `1433` to internal server `10.168.1.40:1433`

## Steps
1. Log in to the router or firewall
2. Open **Port Forwarding** or **NAT Rules**
3. Create a new rule:
   - External Port: `1433`
   - Internal IP: `10.168.1.40`
   - Internal Port: `1433`
   - Protocol: `TCP`
4. Save and apply configuration

## Verification
From outside the local network:

**```powershell**
Test-NetConnection -ComputerName <PUBLIC_IP> -Port 1433