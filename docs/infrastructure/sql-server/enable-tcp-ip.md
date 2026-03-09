# Enable TCP/IP in SQL Server

## Purpose
Enable TCP/IP networking so SQL Server can accept remote connections.

## Prerequisites
- SQL Server installed
- Access to SQL Server Configuration Manager
- Local administrator permissions

## Steps
1. Open **SQL Server Configuration Manager**
2. Go to **SQL Server Network Configuration**
3. Select **Protocols for MSSQLSERVER**
4. Right-click **TCP/IP**
5. Click **Enable**

## Restart SQL Server
1. Open **SQL Server Services**
2. Restart **SQL Server (MSSQLSERVER)**

## Verification
Run:

```powershell
Test-NetConnection -ComputerName <SERVER_IP> -Port 1433