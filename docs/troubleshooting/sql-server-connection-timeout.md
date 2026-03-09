# SQL Server Connection Timeout

Connection timeouts when accessing SQL Server can occur due to network, firewall, or server configuration issues. This guide helps diagnose and resolve these problems.

## Common Causes

- **Firewall blocking connections** - Windows Firewall or network firewall blocking traffic
- **SQL Server not listening** - TCP/IP protocol not enabled
- **Incorrect connection string** - Wrong server name, port, or credentials
- **Port not accessible** - SQL Server listening on non-standard port that isn't open
- **Network connectivity issues** - Network path unreachable or routing problems
- **SQL Server service stopped** - Service not running

## Diagnostic Steps

### 1. Verify SQL Server is Running

```powershell
# Check SQL Server service status
Get-Service -Name MSSQLSERVER
```

The status should show "Running". If not, start the service:

```powershell
Start-Service -Name MSSQLSERVER
```

### 2. Check TCP/IP is Enabled

TCP/IP must be enabled for remote connections. See [Enable TCP/IP](../infrastructure/sql-server/enable-tcp-ip.md).

### 3. Test Network Connectivity

Test if the port is reachable:

```powershell
Test-NetConnection -ComputerName <server-name> -Port 1433
```

If `TcpTestSucceeded` is `False`, see [TCP Test Succeeded False](./tcp-test-succeeded-false.md).

### 4. Verify Firewall Rules

Check that Windows Firewall allows SQL Server traffic. See [Open Windows Firewall for SQL Server](../infrastructure/sql-server/open-windows-firewall-for-sql-server.md).

### 5. Check Network Firewall

If the server is behind a network firewall, ensure:
- Port 1433 (default) is open
- Source IP is whitelisted

See [Firewall Whitelisting](../infrastructure/networking/firewall-whitelisting.md) and [IP Whitelisting](../infrastructure/security/ip-whitelisting.md).

## Solutions

### For Local/LAN Connections

1. Verify SQL Server service is running
2. Enable TCP/IP protocol
3. Configure static port if using named instance
4. Open Windows Firewall for SQL Server

### For Remote/Internet Connections

1. Complete all local steps above
2. Configure network firewall rules
3. Use port forwarding or tunneling if behind NAT
4. Consider using Dev Tunnels or ngrok for testing

See:
- [Expose SQL Server from VM](../infrastructure/virtual-machines/expose-sql-server-from-vm.md)
- [VM Behind NAT Access](../infrastructure/virtual-machines/vm-behind-nat-access.md)
- [Setup Dev Tunnels](../tools/dev-tunnels/setup-dev-tunnels.md)
- [Setup ngrok HTTP Tunnel](../tools/ngrok/setup-ngrok-http-tunnel.md)

### For Power BI Connections

If using Power BI:
1. Install Power BI Gateway on network with SQL Server access
2. Configure gateway with SQL Server credentials
3. Ensure gateway machine has network access to SQL Server

See [Configure Power BI SQL Server Gateway](../data-platform/powerbi/configure-powerbi-sql-server-gateway.md).

## Related Documentation

- [Enable TCP/IP](../infrastructure/sql-server/enable-tcp-ip.md)
- [Allow Remote Connections](../infrastructure/sql-server/allow-remote-connections.md)
- [Configure Static Port 1433](../infrastructure/sql-server/configure-static-port-1433.md)
- [Open Windows Firewall for SQL Server](../infrastructure/sql-server/open-windows-firewall-for-sql-server.md)
- [Test Port Connectivity](../infrastructure/networking/test-port-connectivity.md)
- [TCP Test Succeeded False](./tcp-test-succeeded-false.md)
