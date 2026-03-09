
# BigQuery to SQL Server Connectivity Checklist

## Purpose
Validate all prerequisites for a data sync process writing into SQL Server.

## Checklist
- SQL Server remote connections enabled
- TCP/IP enabled
- SQL Server static port configured
- Firewall inbound rule open for 1433
- External/public access configured if required
- Source system IP whitelisted
- SQL login available
- Database permissions granted
- Connectivity tested successfully

## Example External Source
- Source IP: `34.89.7.83`
- Destination Port: `1433`

## Validation Command
```powershell
Test-NetConnection -ComputerName <PUBLIC_IP_OR_HOST> -Port 1433