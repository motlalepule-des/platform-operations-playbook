# Configure Power BI SQL Server Gateway

## Purpose
Connect Power BI Service to a SQL Server source through the gateway.

## Requirements
- Gateway installed and online
- SQL Server reachable from gateway machine
- Database credentials available

## Steps
1. Open **Power BI Service**
2. Go to **Manage Connections and Gateways**
3. Select the gateway
4. Add a new data source
5. Choose **SQL Server**
6. Enter:
   - Server name
   - Database name
   - Authentication method
7. Save
8. Map dataset to the configured data source

## Important
The gateway machine must be able to reach SQL Server on the network.

## Verification
Run a dataset refresh from Power BI Service.