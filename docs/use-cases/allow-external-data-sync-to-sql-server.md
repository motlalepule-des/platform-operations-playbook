# Use Case: Allow External Data Sync to SQL Server

## Scenario

A third-party data pipeline needs to write data into a SQL Server database hosted inside a private network.

Example:

BigQuery ETL job → SQL Server

## Requirements

- External system IP must be whitelisted
- SQL Server must allow remote connections
- TCP/IP protocol enabled
- Port 1433 reachable

## Steps

1. Enable TCP/IP in SQL Server
2. Configure static port 1433
3. Open Windows Firewall inbound rule
4. Configure router port forwarding
5. Whitelist external IP address

Example:

Source IP: `34.89.7.83`

## Connectivity Test

Run:

```powershell
Test-NetConnection -ComputerName <PUBLIC_IP> -Port 1433