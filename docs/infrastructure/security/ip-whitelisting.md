

# IP Whitelisting

## Purpose
Restrict service access to trusted source IP addresses.

## Typical Use Cases
- External data sync service
- Power BI gateway dependencies
- Vendor-managed integrations

## Example Rule
Allow inbound TCP `1433` from `34.89.7.83` only.

## Benefits
- Reduces attack surface
- Limits exposure
- Supports controlled partner connectivity

## Best Practices
- Keep whitelist entries documented
- Remove unused IPs
- Pair with strong credentials and encryption