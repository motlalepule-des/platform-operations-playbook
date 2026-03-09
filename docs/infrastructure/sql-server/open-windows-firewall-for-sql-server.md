

# Open Windows Firewall for SQL Server

## Purpose
Allow inbound SQL Server traffic through Windows Firewall.

## Steps
1. Open **Windows Defender Firewall with Advanced Security**
2. Select **Inbound Rules**
3. Click **New Rule**
4. Choose **Port**
5. Select **TCP**
6. Enter `1433`
7. Choose **Allow the connection**
8. Apply to required profiles
9. Name the rule `SQL Server TCP 1433`

## Optional
Add outbound rule if required by policy.

## Verification
From another machine:

```powershell
Test-NetConnection -ComputerName <SERVER_IP> -Port 1433