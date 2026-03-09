

# Allow Remote Connections to SQL Server

## Purpose
Allow SQL Server to accept connections from other machines on the network or internet.

## Steps
1. Open **SQL Server Management Studio**
2. Connect to the SQL Server instance
3. Right-click the server
4. Select **Properties**
5. Open the **Connections** page
6. Ensure **Allow remote connections to this server** is enabled

## SQL Authentication
If SQL logins are required:
1. Right-click server
2. Select **Properties**
3. Open **Security**
4. Choose **SQL Server and Windows Authentication mode**
5. Restart SQL Server

## Verification
Try connecting from another machine using:
- Server IP
- SQL login
- Port 1433

## Notes
Remote connections also depend on:
- TCP/IP enabled
- Firewall open
- Correct router/NAT configuration