# Expose SQL Server from VM

## Purpose
Publish a SQL Server instance running inside a VM for remote access.

## Steps
1. Enable TCP/IP in SQL Server
2. Set static port 1433
3. Restart SQL Server
4. Open Windows Firewall inbound TCP 1433
5. Configure router/firewall NAT forwarding to the VM
6. Restrict external access by source IP where possible

## Verification
From an external network:

```powershell
Test-NetConnection -ComputerName <PUBLIC_IP> -Port 1433