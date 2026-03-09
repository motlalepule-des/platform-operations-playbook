# Configure SQL Server Static Port 1433

## Purpose
Set SQL Server to listen on a fixed TCP port, usually 1433.

## Steps
1. Open **SQL Server Configuration Manager**
2. Go to **SQL Server Network Configuration**
3. Open **Protocols for MSSQLSERVER**
4. Double-click **TCP/IP**
5. Open the **IP Addresses** tab
6. Scroll to **IPAll**
7. Clear **TCP Dynamic Ports**
8. Set **TCP Port** to `1433`

## Restart
Restart the SQL Server service after saving changes.

## Verification
Check connectivity:

```powershell
Test-NetConnection -ComputerName <SERVER_IP> -Port 1433