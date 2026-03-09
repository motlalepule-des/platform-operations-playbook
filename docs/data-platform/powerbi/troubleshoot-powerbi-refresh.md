# Troubleshoot Power BI Refresh

## Common Issues
- Gateway offline
- Invalid credentials
- SQL Server unreachable
- Port 1433 blocked
- Dataset not mapped to gateway source

## Checks
1. Confirm gateway status is online
2. Confirm SQL Server is reachable from gateway host
3. Confirm credentials are valid
4. Confirm server and database names match exactly
5. Confirm dataset is mapped to the correct data source

## Network Test
From the gateway machine:

```powershell
Test-NetConnection -ComputerName <SQL_SERVER_IP> -Port 1433