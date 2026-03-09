# Use Case: Connect Power BI to On-Premises SQL Server

## Scenario

An organization wants to build Power BI dashboards using a SQL Server database hosted on a local server or virtual machine.

Because the database is not publicly accessible, Power BI Service cannot connect directly.

## Solution

Use the **Power BI On-Premises Data Gateway**.

## Steps

1. Install Power BI Gateway on a machine with network access to SQL Server.
2. Enable TCP/IP on SQL Server.
3. Allow remote connections in SQL Server.
4. Ensure port 1433 is open between the gateway and SQL Server.
5. Configure a SQL Server data source in Power BI Service.
6. Map the dataset to the gateway data source.

## Related Runbooks

- `install-powerbi-gateway.md`
- `configure-powerbi-sql-server-gateway.md`
- `enable-tcp-ip.md`
- `open-windows-firewall-for-sql-server.md`

## Verification

1. Trigger a manual dataset refresh in Power BI Service.
2. Confirm the refresh completes successfully.

## Common Problems

| Problem | Cause |
|------|------|
| Gateway offline | Gateway service not running |
| Login failed | Wrong SQL credentials |
| Timeout | Firewall blocking port 1433 |